# Advanced Plotting Tool
This program is a command-line tool for advanced figure plotting.

### Commands
+ `plot <series_name> [<label> <color> <linestyle> <side>]`: Add a series to the plot
+ `title <title>`: Set the title of the plot
+ `xlabel <label>`: Set the x-axis label
+ `yleftlabel/ylabel <label>`: Set the left y-axis label
+ `yrightlabel <label>`: Set the right y-axis label
+ `yticks <ticks> [<labels>]`: Set the y-axis ticks and labels
+ `legend <loc>`: Set the legend location
+ `save <path>`: Save the plot to the path and close the figure
+ `exit`: Exit plotting

The series name should be in the format of `<results>:<attribute>:<instances>:<start_time>`, where
+ "results" is the result folder,
+ "attribute" is the attribute to be plotted, can be:

|CS|EV & PVW|GEN & LINE|BUS & ESS|
|---|---|---|---|
|cs_load|ev_soc|gen_active|bus_active_gen|
|cs_wait_count|ev_cost|gen_reactive|bus_reactive_gen|
|cs_net_load|ev_status|gen_costp|bus_active_load|
|cs_price_buy|ev_cpure|line_active|bus_reactive_load|
|cs_price_sell|ev_earn|line_reactive|bus_shadow_price|
|cs_discahrge_load|pvw_p|line_current|ess_p|
|cs_v2g_cap|pvw_cr||ess_soc|

+ "instances" is the instance name, such as
    "CS1", "CS2", `<all>`, `<fast>`, `<slow>` for CS,
    "v1", "v2" for EV,
    "G1", "G2" for generator,
    "B1", "B2" for bus,
    etc.,
+ "start_time" is the starting time of the series. "0" by default. This is optional.

### Example
```
plot "results:cs_load:CS1" "CS1 Load" "blue" "-" "left"
plot "results:cs_load:CS2" "CS2 Load" "red" "--" "left"
title "CS1 & CS2 Load"
xlabel "Time"
yleftlabel "Load/kWh"
legend
save "test.png"
```

You can load the commands from a file as an argument in the command prompt/terminal.