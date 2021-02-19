# SoC Implementation flow with pads, On-chip PLL block (avsdpll_3v3), and PLL control circuit

First we set-up the project: Go to `~/openlane_working_dir/openlane` and execute the following:
```javascript 
export PDK_ROOT=<absolute path to where skywater-pdk and open_pdks reside>
```
```javascript 
docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) openlane:rc6
```
```javascript 
 ./flow.tcl -design chip_io -init_design_config
 ./flow.tcl -design vsdPLLSoC -init_design_config
```
This will create an initialize config.tcl file inside the design **chip_io** and **vsdPLLSoC**.

## Adding verilog files inside src folder

Add the .v files inside the `~/chip_io/src` and `~/vsdPLLSoC/src` which is shown in verilog folder. The create a _lef_ folder inside src and add the lef files inside it. LEF files are shown inside lef folder.

## The flow for chip_io design

Now we execute the following command to run the interactive flow:
```javascript 
 ./flow.tcl -design chip_io -tag chip_io -interactive
```
This command created a tag named chip_io inside `chip_io/runs/`.
 
Then we setup openlane package and prepare the design:
```javascript 
package require openlane 0.9
```
```javascript 
prep -design chip_io -tag chip_io -overwrite
```

## Setup the LEF file for chip_io
```javascript 
set lefs [glob $::env(DESIGN_DIR)/src/lef/*.lef]
```
```javascript 
add_lefs -src $lefs
```

## Synthesis
```javascript 
run_synthesis
```

## The flow for vsdPLLSoC design

Now we execute the following command to run the interactive flow:
```javascript 
 ./flow.tcl -design vsdPLLSoC -tag vsdPLLSoC -interactive
```
This command created a tag named chip_io inside `vsdPLLSoC/runs/`.
 
Then we setup openlane package and prepare the design:
```javascript 
package require openlane 0.9
```
```javascript 
prep -design vsdPLLSoC -tag vsdPLLSoC -overwrite
```

## Setup the LEF file for vsdPLLSoC
```javascript 
set lefs [glob $::env(DESIGN_DIR)/src/lef/*.lef]
```
```javascript 
add_lefs -src $lefs
```

## Synthesis
```javascript 
run_synthesis
```

## Floorplan
```javascript
run_floorplan
```

## Layout in magic

To view the floorplan layout in magic we use the following command:
```javascript
magic -T /home/arun/Desktop/vsdflow/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic lef read ../../tmp/merged.lef def read vsdPLLSoC.floorplan.def &
```
Floorplan layout looks something like this:

![](/images/floorplan1.png)

![](/images/floorplan2.png)

![](/images/floorplan3.png)

![](/images/floorplan4.png)





