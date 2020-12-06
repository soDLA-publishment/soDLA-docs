# Ping-pong Scheme

----

## NV_NVDLA_XXX_reg

```
class NV_NVDLA_XXX_reg(implicit val conf: nvdlaConfig) extends Module

```
Need to instantiate one `NV_NVDLA_BASIC_REG_single`, two `NV_NVDLA_XXX_REG_dual`,  `NV_NVDLA_CSB_LOGIC` within this module. `NV_NVDLA_XXX_reg` module need to be modified from existing soDLA NV_NVDLA_XXX_reg examples, according to the exactly requirements such as performance counter, interrupt counter.

<img class="soDLA" src="/img/soDLA-jpgs/ping-pong-diagram.jpg" /> 

### Interfaces

```
//general clock
val nvdla_core_clk = Input(Clock())      

//csb2xxx
val csb2xxx = new csb2dp_if

//reg2dp
val reg2dp_op_en = Output(Bool())
val reg2dp_field = new xxx_reg_dual_flop_outputs
val dp2reg_done = Input(Bool())

//slave cg op
val slcg_op_en = Output(UInt(conf.XXX_SLCG_NUM.W))
```

`csb2dp_if` definition, need to be connected to `NV_NVDLA_csb_master`

```
class csb2dp_if extends Bundle{
    val req = Flipped(DecoupledIO(UInt(63.W)))
    val resp = ValidIO(UInt(34.W))
}

```

`reg2dp_field` is the config output to the data processor.
`xxx_reg_dual_flop_outputs` need to be defined in the config

```
class xxx_reg_dual_flop_outputs extends Bundle{
    val config_0 = Output(UInt(5.W))
    val config_1 = Output(UInt(5.W))
    .
    .
    .
    val config_n-1 = Output(UInt(5.W))
    val config_n = Output(UInt(5.W))
}
```
`XXX_SLCG_NUM` is the total amount of modules within one tile.

### Module Description

|       |  Description  |
| ----- | ------- | 
| `NV_NVDLA_CSB_LOGIC`   |  csb logic to register   | 
| `NV_NVDLA_BASIC_REG_single` |   single register group  | 
| `NV_NVDLA_XXX_REG_dual` |   duplicated register group  | 

### Add-ons

Performance counters and interrupt counters, please refer to csc, sdp, pdp, cdp.

### Workflow

<img class="soDLA" src="/img/soDLA-jpgs/ping-pong-configuration-flow.jpg" /> 






























