# DMA Design in soDLA

----

In addition to the [DMA Techniques](https://vvviy.github.io/2019/01/11/NVDLA-Source-Code-Analysis/) written by Max. 

Here are the additional elaborations.

## DMA Transfer

DMAs in soDLA/cora are connected to the axi4 by nocif.

### RDMA

#### RDMA Request

RDMA ig

RDMA request a data, host get ingressed,  (size of data block, memory address), for example, rdma request a block size of 2, starting from memory address, the host should return a block of data from memory address. 

#### RDMA Respond

RDMA eg

RDMA receive a data, host egress, (mask bit of data, data), for example, memory can transfer 256 bits of data per time, but a processor can only process 128 bits of data per time, the mask bits if 256/128 = 2, the mask bits indicate which batch of data to be processed. But in our designs, mask bits 1, means the processor should process all the data within one batch. 

### WDMA

cmd and dat flow respond to the host seperately. Dat arrives first, then cmd.

#### WDMA dat

WDMA write a data to host, (dirty bit, mask bit of data, data), dirty bit means whether the write process is valid or not. 

#### WDMA cmd

WDMA write a cmd to host, (dirty bit, size of data block, memory address)










