# Generate and Edit EVCSs

It is easy to generate and edit EVCSs in the GUI.

## Generate EVCSs
Switch to `Fast CS`(FCS) and `Slow CS`(SCS) respectively to generate differnt types of charging stations. We strongly recommend you to generate CS from the downloaded positions if you are using a real-world road network. Click the `Generate` button to generate CS. Both FCS and SCS have to be generated for simulation.

There is a big labelframe in the bottom half of this page, including some important parameters for CS generation. 

![alt text](../imgs/5.png)

In the first line, you will see **Number of slots** in each CS and **Random seed**. The former parameter indicates how many chargers in a single CS, and the latter parameter indicates the result of randomization, which is helpful in reproducible experiments.

There are 3 options for **CS Source Edges**:
+ **All edges**: Use all reachable edges as CSs. This is the default option but not recommend for real world cases, since it may create too much CSs.

+ **Edges determined by downloaded CS positions**: Use the result of CS Downloader. Downloaded CS positions are attached to the nearest edges. **NOTE**: This option is only available when `CS CSV Descriptor` is not None in the sidebar and the road network is created from OSMWebWizard.

+ **Edges determined by buildings' contours**: Use the center of specific types of polygons stored in the road network. These calculated center points will be attached to nearest edges. **NOTE**: This option is only available when `Polygon Descriptor` is not None in the sidebar.

There are 3 options for **Generation Mode**:
+ **All available**: Generate all CSs. This is the default option. 

+ **Given**: Generate CSs according to the given CS positions. Assign CS in the text box, separated by comma `,`. No spaces are allowed.

+ **Random N**: Randomly pick $N$ CSs to generate. Input $N$ in the text box.

There are 2 options for **Users' Buying/Selling Price**:
+ **5 segments random**: For each day, 5 segments of price are randomly generated. The price floats around a given base price. Read source code `v2sim\trafficgen\__init__.py: TrafficGenerator._CS.write` for details.

+ **Fixed**: The price is fixed to a given value during the whole simulation. Input the price in the text box.

There are 4 options for **Bus Selection Mode**:

+ **By bus location**: The CS will be assigned to the nearest bus. This is the default option. **NOTE**: This option is only available when grid is defined and all the buses' locations are specified in the grid. **If not all the buses' locations are specified in the grid, this option will be STILL available, and it will work in an unpredictable way!**

+ **All available**: Assign CS to a bus randomly selected from all available buses.

+ **Given**: Assign CS to a bus randomly selected from the given buses.

+ **Random N**: Randomly select $N$ buses from all available buses, and assign CS to a bus randomly selected from the $N$ buses.

After selecting your preference, you can click `Generate` to create CSs. Do not click `Generate` repeatedly if the generation process is not finished yet. The progress is shown in the command prompt.

If you are familiar with command prompt, you can use `v2sim-gen-cs` or `v2simux-gen-cs` to generate CSs, whose introduction is stored [here](v2sim/tools/gen_cs).

### Edit an EVCS
On the top of this page, all the EVCSs in the case are listed. It may be blank if EVCSs are not generated. Double clicking on the items to edit them. 

**Note that not all the properties is editable.** For example, editing `Edge`/`Node` column directly in the GUI is not allowed, since it may corrupt the case. Directly open the file in an editor (such as Notepad or VSCode, etc) to edit it. The CS definition file (*.scs.xml for SCS, *.fcs.xml for FCS) follows the XML format. Please visit [CS Format](../cs) to get the details.

Below the items are a **counter** (`Count: 10` in the image) on the left and a **search box** (with a button `Find`) on the right. Input the edge of CS in the search box and click `Find` to quickly locate the CS.
