# Quick Start

## A. Setup the environment

1. **Setup Python**: Visit `https://www.python.org/download` to get Python (version >=3.9 is required. Older version may not run this program normally).

2. **Download code**. You can directly download code by clicking the `download` button on GitHub page, or you can setup `git` and clone the repository by one of the following command. The official tutorial about using `git` is [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
```bash
# Only clone the release version of v2sim
git clone -b main --single-branch https://github.com/fmy-xfk/v2sim.git
# Clone both v2sim, v2sim-ux and their develop branch
git clone https://github.com/fmy-xfk/v2sim.git
```

3. **Setup necessary packages**. Ensure you have installed `pip` together with Python. **Note**: As for package `libsumo`, its version must be same as the vesion of SUMO!
```bash
python -m ensurepip
python -m pip install -r requirements.txt
```

## B. Create a case
There are 3 pre-defined cases in the `cases` folder: `std_12nodes`, `std_37nodes`, and `std_Nanjing`. You can utilize the 3 cases directly, or create a new case from scratch.

**Full tutorial for creating a new case:**

1. **Download transportation network**: Use `gui_osmWizard.py` to download the road network. Your browser will be opened and a map will be displayed after running this script. <br> ![alt text](/imgs/1.png)
    + You can input a city name and click `Search` to turn to it, or you can input the latitude and longitude and click `Go to`. `Use current location` may not work due to the restriction of network and modern browser.
    + By default, the whole map shown in your screen will be downloaded. You can tick the checkbox `Select area` to only select a rectangle area. 
    + You can adjust items `Options` by your preference.
    + In the second tab, only `highway` is ticked by default. Other options are not recommended since V2Sim will not simulate their motion.
    + Click the `Generate Scenario` button after proper configuration. The case will be generated in `cases` folder with a name of current time. `gui_main.py` will automatically open after the case is generated. It looks like the following image: <br> ![alt text](/imgs/0.png)
    + If the generated case is not selected automatically, click the `Add project...` link, select the folder you have created, and then open it. You will see something like this: <br> ![alt text](/imgs/3.png)

**Note**: If you want to alter the road network, please use `netedit` of SUMO. You can use command `netedit` in your command prompt to start the program if Python has been added to the environment variable `PATH`.

2. **Download charging station positions**: If the area you downloaded is in **China Mainland**, you can download charging station positions in this program. Otherwise, please skip this step.
+ Switch to `CS Downloader` page and type an AMap(Chinese: 高德地图) developer key in the given input box. **ATTENTION: You must apply for a key for web service (Chinese: Web服务) on the [AMap official site](https://lbs.amap.com/). This function will never work without a key.** 
+ Click `Download` to get CS positions from AMap. Please wait patiently while downloading the CS positions. If there are too many CSs in the given area, they may not be all downloaded due to the restriction of AMap.
+ A successful result is shown below (The address are all Chinese since they are located in China):

![alt text](/imgs/4.png)

+ After download the CS positions, please close the project editor and reopen it to avoid any potential error in the editor.

3. **Generate charging stations**: Switch to `Fast CS` and `Slow CS` respectively to generate differnt types of charging stations. We strongly recommend you to generate CS from the downloaded positions if you are using a real-world road network. Click the `Generate` button to generate CS.

+ Do **NOT** click `Generate` repeatedly even if it seems not working. The progress will be shown in the command prompt instead of popping up another window.

+ **Generating Fast CS and CS is neccessary.**

+ A successful result is like this:

![alt text](/imgs/5.png)

4. **Edit grid**: Switch to `Network` page to view the road netowrk (in blue) and distribution network (in black). Hold the mouse's right button to pan. Distribution network can be moved and edited by left click. **However, editing road network is not available here. Please use SUMO NetEdit.**

![alt text](/imgs/11.png)

(Slim rectangle = Bus, Triangle = PV / Wind turbine, Square = Energy Stoarge, Circle = Generator)

5. **Generate vehicles**: Switch to `Vehicles` page to generate vehicles. We strongly recommend you to generate trips from the buildings' contours and types if you are using a real-world road network. 

+ Do **NOT** click `Generate` repeatedly even if it seems not working. The progress will be shown in the command prompt instead of popping up another window.

6. **Start simulation**: Make sure the `FCS`, `SCS`,` Road Network`, and `Vehicles` are not `None` in the left column. Then go back to `Simulation` page, tick your desired statistic items, and click `Start Simulation!`.


## C. Simulation
The window will shut down once you clicked `Start Simulation!`. Please wait patiently during simulation. It may cost several hours when simulate a large real-world network. You can watch the progress and estimated time required displayed in the command prompt.

If you want to run several simulation parallelly (which can fully utilize your CPU), you can use `gui_para.py`. This function is quite useful 
when you want to change a specific parameter to measure its implication. Like the following image:
![alt text](/imgs/10.png)

### D. View the results
After the simulation is done, run `gui_viewer.py` and open the case. The results is in the case folder, with a subfolder named `results`. It will be something like this:

![alt text](/imgs/6.png)

#### Plotting
Tick the items you want to draw the corresponding figures. Click the button `Plot` to draw figures. Figures of results will be stored in `results\<case_name>\figures`.

**NOTE:**  Some items may not be available becasuse the corresponding statistic item is not selected when configuring the project, and thus it is not produced. 

**Compare two results:** Use `gui_cmp.py` to show two results in the same page, which makes it easier for you to identify the difference. Just like the following image:

![gui_cmp](/imgs/9.png)

**Better Plotting:** The GUI version of plotting is quite limited. You can visit [Wiki](https://github.com/fmy-xfk/v2sim/wiki) for better plotting tools: `cmd_advplot.py` and `cmd_plot.py`. The former one provides highly-customizable plotting experience, while the latter one enables plotting for batch of result folders.

#### Grid Information
You can also collect the data of the power grid at a specific time in page `Grid`. Enter the time point and then click `Collect`.

![alt text](/imgs/7.png)

#### Trips viewer
Trips are also counted in `Trips` page. You can filter them by giving the conditions in the bar attached to the bottom of the window. You can also save the filtered results to a specific file.

![alt text](/imgs/8.png)

#### State viewer
If you have ticked "Save state ..." before simulation, the state will be saved in the result folder. You can go through the status of FCS, SCS, and EV in the page. 

![alt text](/imgs/12.png)