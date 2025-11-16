# Code Structure

In this repository, the core code of V2Sim is store in the folder `v2sim`. Besides, `v2sim/tools/sim_single.py` provides the method to start up a simulation instance. This passage will introduce the structure of the folder `v2sim`, which can be deployed as a package, as introduced in the last passage.

+ V2Sim Package
  + traffic: the core module
    + cs.py: charging station, both FCS and SCS are included, inheriting from a base class CS.
    + cslist.py: wrapper for CS collection, supporting collective operations.
    + ev.py: electric vehicle and trip.
    + evdict.py: wrapper for EV collection, supporting collective operations.
    + trip.py: trip logger trip event listener, and trip logging file reader.
    + utils.py: helper and miscellaneous functions.
    + inst.py: wrapper for the whole traffic simulation with EVs and CSs.
  + trafficgen: module for data generation
  + statistics: module for logging data
  + plugins: module for plugin and external code calling
  + plotkit: module for results plotting
  + locale: language helper
  + sim_core.py: V2Sim simulation instance wrapper

## cs.py
+ **SCS (Slow Charging Station)**: Provides slow charging, typically with lower power and longer charging times. Vehicles are categorized using `chi` and `free`:
    - `chi`: Vehicles that are currently charging, utilizing the `MaxPCAllocator`.
    - `free`: Vehicles that are fully charged, available for V2G discharge to the power grid.
    - **SCS does not implement queuing.** Due to the typically long charging times at slow stations, waiting queues are generally not considered. Vehicles remain connected after being fully charged to be available for V2G dispatch, hence there is no dedicated queuing state.
    - **Only SCS executes the specific V2G logic**, checking if vehicles are willing to participate in V2G.

+ **FCS (Fast Charging Station)**: Provides fast charging, typically with higher power and shorter charging times. Vehicle states are divided into two categories: `chi` and `buf`.
    - `chi`: Vehicles that are currently charging.
    - `buf`: Vehicles that are waiting to charge.
    - When a charger becomes available, vehicles in the `buf` queue enter the `chi` state to charge sequentially. Vehicles leave the charging station immediately after being fully charged. The freed charger is then allocated to the next vehicle in the waiting queue, following a first-come, first-served, "charge-and-leave" principle.
    - **FCS does not support V2G.** Charging power is dynamically allocated to each charging vehicle by the allocator. Therefore, only the `MaxPCAllocator` is called, and V2G-related functions like `V2GAllocator` are not invoked.

+ For details on `MaxPCAllocator` and `V2GAllocator`, please refer to the [Code Extensions](dev/exts) section.

## ev.py
The EV module consists of four classes: `Trip`, `VehStatus`, `EV`, and `ChargeRatePool`. These four parts work together through inter-class collaboration to implement the complete simulation process.

### EV
+ Initialization Phase: Configures basic attributes and initializes state (defaults to Parking status) by inputting the EV's ID, trip list, battery and energy consumption parameters, etc.

+ Trip Execution Phase: The EV switches to the next trip via `next_trip`. After SUMO confirms departure, it enters the Driving state. During driving, power is deducted in real-time and mileage is updated. Upon reaching the destination, it returns to the Parking state, cycling until all trips are completed.

+ Special Scenario Handling Phase:
  - If power is insufficient during driving, the EV switches to the Charging state, charges according to a specified strategy, calculates costs, and resumes the trip after charging is complete.
  - If charging / V2G conditions are met while parked, it actively enters the Charging / V2G state.
  - If the battery is depleted, it first enters the Depleted state, and resumes charging after being rescued to a charging station.

### Trip
The Trip class is used to describe a single travel task, primarily describing attributes such as where the vehicle is going, when it departs, the origin and destination, and the route.

| Attribute | Significance |
|---|---|
| trip_id | The ID of this trip, used for identification |
| depart_time | Departure time (simulation timestamp, unit usually seconds) |
| from_TAZ | Origin traffic analysis zone (This parameter does not participate in the simulation and has no practical meaning) |
| to_TAZ | Destination traffic analysis zone (This parameter does not participate in the simulation and has no practical meaning) |
| route | Path, consisting of a series of road IDs |
| fixed_route | Whether the route is fixed (Whether route replanning is allowed; this parameter currently cannot be used normally) |
| additional | User-defined optional additional information dictionary (e.g., number of passengers, vehicle type, other labels, etc.) |

### VehStatus
The Class VehStatus is an enumeration class inheriting from `enum.IntEnum`. Its core function is to clearly define the 5 core operational states of an electric vehicle (EV class). The following are the five state types for the vehicle:

| Value | Name | Meaning | Typical Occurrence Scenario |
|---|---|---|---|
| 0 | Driving | Driving state | The vehicle is moving, destination is the trip end (Trip.toTAZ) or the target charging station (EV._cs) |
| 1 | Pending | Pending start state | A start command has been sent to SUMO (traffic simulation software), but the vehicle has not actually started moving within SUMO |
| 2 | Charging | Charging state | The vehicle is stationary at a charging station, replenishing power via fast charging (EV._efc_rate) or slow charging (EV._esc_rate) |
| 3 | Parking | Parking/Stopped state | The vehicle is stationary and not charging, including: intervals between trips, standby period before the first trip, parking period after the last trip ends |
| 4 | Depleted | Battery depleted state | Battery power (EV._elec) drops to 0, the vehicle cannot continue driving, requiring emergency charging or rescue |

### ChargeRatePool
The code contains a pool function `ChargeRatePool`. Its core purpose is to uniformly store, call, and extend different charging rate calculation logics, making the EV's charging rate strategy configurable and extensible, avoiding hardcoding the rate calculation within the `EV` class. The code pre-populates the `_pool` with two of the most commonly used charging rate logics by default, corresponding to two internal functions.

| Strategy | Corresponding Function | Core Logic | Applicable Scenarios |
|---|---|---|---|
| Equal | `_EqualChargeRate` | No correction, directly returns the input `rate` | Ideal fast-charging scenarios (SOC does not affect rate) |
| Linear | `_LinearChargeRate` | Segmented correction: Returns `rate` when `EV.SOC ≤ 0.8`; Returns `rate × (3.4 - 3×EV.SOC)` when `EV.SOC > 0.8` | Relatively realistic fast-charging scenarios (Rate decays more noticeably as SOC increases) |

Example: If the base rate `rate = 0.01 kWh/s` (i.e., 36 kW), and the EV's SOC = 0.9, the actual rate under the `Linear` strategy is: `0.01 × (3.4 - 3 × 0.9) = 0.01 × 0.7 = 0.007 kWh/s` (i.e., 25.2 kW), which aligns with the characteristic of decreasing rate in the later stages of real fast charging.