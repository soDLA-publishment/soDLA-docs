
# Firesim Instruction

---

Note: once the instances was launched, your AWS account is charged for its use. You
should “stop” your manager instance when you are not using it for more than a few hours—
especially at night. This will make sure our allocated AWS credits will sufficent.

## Login in the AWS Instance

You can see a folder firesim-nvdla on the sever,
First run these commands:

```
cd firesim-nvdla
source sourceme-f1-manager.sh
```

Then, boot the fpga sever:

```
firesim launchrunfarm
firesim infrasetup
firesim runworkload
```

## Login to FPGA Server

Finally, link to fpga sever:
in another terminal, 
ssh to the ip address given by `firesim manager` 

```
ssh IP 
screen -r fsim
```

Username: root
Password: firesim

More details please check 
https://github.com/CSL-KU/firesim-nvdla and https://docs.fires.im/en/latest/index.html 


## Run NVDLA Simulation

```
cd darknet-nvdla
./solo.sh
```

## Test soDLA

1.  soDLA RTL Replacement:

a)	The replaced code is in nvidia-dla-blocks/vsrc. After the replacement, modify the vsrc.mk script in nvidia-dla-blocks, comment out the path of the original module, and add the path of the newly replaced file.

b)	Replace the name of the module in the Verilog code, and change the generated module from NVDLA to soDLA to distinguish it. If connection modules such as wrappers are used, add the path of these connection modules in a).

c)	Modify the script in this directory to make it correspond to the new Verilog file (aws-fpga-firesim/hdk/cl/developer_designs/cl_firesim/build/scripts/encrypt.tcl)

2.	Use firesim to write fpga：

a)	Before writing, change the config_*.ini in the firesim/deploy directory and comment out all the examples that do not need to be built.

b)	After burning, use scp to download all the log and backup to the local as a local backup.

3.	Test the darknet function: refer to the flow in firesim-nvdla, if darknet can correctly identify the target in the picture, the module will function normally.


