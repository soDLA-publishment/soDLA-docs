# Delay Model

----

When rdma send request, wdma can get the responce data quickly to avoid blocking.

## FIFO Depth Estimation in RDMA

FIFO should neither to be full nor empty. 

soDLA delay model is basic,

```
depth = input_latency * output_throughput / input_thoughput
```

However, you can change the depth by mutiply a coefficient, such as 1/1.1 or 1/1.2. The depth should cover the average latency.

In the `project_spec`, corresponds to 

```
val NVDLA_VMOD_XXX_RDMA_LATENCY_FIFO_DEPTH  = max(4,ceil(NVDLA_PRIMARY_MEMIF_LATENCY*NVDLA_VMOD_XXX_RDMA_OUTPUT_THROUGHPUT)
```
`NVDLA_PRIMARY_MEMIF_LATENCY` is the delay defined by NoC team.

## WDMA Pattern







