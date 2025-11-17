# View the results
After the simulation is done, the results will be stored in the case folder, with a subfolder named `results`. Run `v2sim-gui` or `v2simux-gui` to show the start-up window and select the case you have just simulated. Then click `View Results` and a new window will pop up to show the results. It will be something like this:

![alt text](../imgs/6.png)

## Plotting
Tick the items you want to draw the corresponding figures. Click the button `Plot` to draw figures. Figures of results will be stored in `results\<case_name>\figures`.

**NOTE:** Some items may not be available becasuse the corresponding statistic item is not selected when configuring the project, and thus it is not produced. 

**Compare two results:** In start-up window, turn to "Tool" menu and select "Result comparer..." item to show two results in the same page, which makes it easier for you to identify the difference. Just like the following image:

![gui_cmp](../imgs/9.png)

**Better Plotting:** The GUI version of plotting is quite limited. You can read `v2sim-advplot` and `v2sim-plot` for better plotting. The former one provides highly-customizable plotting experience, while the latter one enables plotting for batch of result folders.

## Grid Information
You can also collect the data of the power grid at a specific time in page `Grid`. Enter the time point and then click `Collect`.

![alt text](../imgs/7.png)

## Trips viewer
Trips are also counted in `Trips` page. You can filter them by giving the conditions in the bar attached to the bottom of the window. You can also save the filtered results to a specific file.

![alt text](../imgs/8.png)

## State viewer
If you have ticked "Save state ..." before simulation, the state will be saved in the result folder. You can go through the status of FCS, SCS, and EV in the page. 

![alt text](../imgs/12.png)
