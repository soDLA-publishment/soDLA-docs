# ARB

----

## NV_NVDLA_arb

```
class NV_NVDLA_arb(n:Int, wt_width:Int, io_gnt_busy:Boolean) extends Module

```

### Interfaces

```
//clk
val clk = Input(Clock())

val req = Input(Vec(n, Bool()))
val wt = Input(Vec(n, UInt(wt_width.W)))
val gnt_busy = if(io_gnt_busy) Some(Input(Bool())) else None
val gnt = Output(Vec(n, Bool()))
```

### Descriptions

Arbiter with weighted input.


`gnt_busy` is off:

<img class="cora" src="/img/soDLA-jpgs/slibs/arb_test0.jpg" /> 

`gnt_busy` is on:

<img class="cora" src="/img/soDLA-jpgs/slibs/arb_test1.jpg" /> 
























