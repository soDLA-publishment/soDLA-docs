# Retiming

----

## Usage 

Need to be setup before, after or within a computational intensive module. 


`valid`, `mask`, `data` need to be instantiated by 

```
val in_rt_dat_pvld_d = Wire(Bool()) +: 
                        Seq.fill(conf.CMAC_IN_RT_LATENCY)(RegInit(false.B))
val in_rt_dat_mask_d = Wire(Vec(conf.CMAC_ATOMC, Bool())) +: 
                        Seq.fill(conf.CMAC_IN_RT_LATENCY)(RegInit(VecInit(Seq.fill(conf.CMAC_ATOMC)(false.B)))) 
val in_rt_dat_data_d = retiming(Vec(conf.CMAC_ATOMC, UInt(conf.CMAC_BPE.W)), conf.CMAC_IN_RT_LATENCY)
```

## Implementation

```
for(t <- 0 to conf.CMAC_IN_RT_LATENCY-1){
    in_rt_dat_pvld_d(t+1) := in_rt_dat_pvld_d(t)
    when(in_rt_dat_pvld_d(t)|in_rt_dat_pvld_d(t+1)){
        in_rt_dat_mask_d(t+1) := in_rt_dat_mask_d(t)
        in_rt_dat_pd_d(t+1) := in_rt_dat_pd_d(t)
    }
}
```






























