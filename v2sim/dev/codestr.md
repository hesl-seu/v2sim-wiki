# Code Structure

In this repository, the core code of V2Sim is store in the folder `v2sim`. Besides, `sim_single.py` provides the method to start up a simulation instance. This passage will introduce the structure of the folder `v2sim`, which can be deployed as a package, as introduced in the last passage.

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