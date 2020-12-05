# FIFO

----

## NV_NVDLA_fifo

```
class NV_NVDLA_fifo(depth: Int, width: Int,
                    ram_type: Int = 0, 
                    distant_wr_req: Boolean = false, 
                    io_wr_empty: Boolean = false, 
                    io_wr_idle: Boolean = false,
                    io_wr_count: Boolean = false,
                    io_rd_idle: Boolean = false,
                    useRealClock: Boolean = false) extends Module

```

### Interfaces

```
//clk
val clk = Input(Clock())

val wr_pvld = Input(Bool())
val wr_prdy = Output(Bool())
val wr_pd = Input(UInt(width.W))

val wr_count =  if(io_wr_count) Some(Output(UInt(log2Ceil(depth+1).W))) else None
val wr_empty = if(io_wr_empty) Some(Output(Bool())) else None
val wr_idle = if(io_wr_idle) Some(Output(Bool())) else None

val rd_pvld = Output(Bool())
val rd_prdy = Input(Bool())  
val rd_pd = Output(UInt(width.W))

val rd_idle = if(io_rd_idle) Some(Output(Bool())) else None

val pwrbus_ram_pd = Input(UInt(32.W))
```

### Descriptions

|       |  Description  |
| ----- | ------- | 
| `ram_type`   |  commonly to be set as zero    | 
| `distant_wr_req` |  quality impact on write request from a distance | 
| `useRealClock` |  internal clock set to be default as clock, all possiblities of clock gating turns off  | 













