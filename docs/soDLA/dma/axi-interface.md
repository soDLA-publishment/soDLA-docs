# What is AXI?

---

Below is from video tutorial by Dillon Huff, 
Muhammad Hadir helps me to write this into docs:

If we generally talk about communicating with the memory, following are the signals that one might think of to read/write from/to the memory:

<img class="sram-interface" src="/img/soDLA-jpgs/sram-interface.png" /> 

The host can read some data by issuing an address on the `read_addr` port and then read the response data after some fixed clock cycles from the `read_data` port.

The host can also write to the memory by providing a valid signal to indicate that the data is valid on the `write_valid` port and providing 
the actual data and address on the `write_data` and `write_address` ports respectively which will then be written into the memory after some fixed clock cycles.

The AXI is just a generalisation of this fixed simple protocol for communicating with the memory. By fixed protocol it means that the above protocol has 
some fixed limitations to it:

##  Limitations in the simple interface mentioned above:

### 1) Reads and writes have fixed latencies
For example, in the read scenario there is no signal from the memory that indicates that the `read_data` contains the valid data from the address mentioned in
the `read_addr` port. The latencies have to be fixed in order for the host to know exactly when the response data coming from `read_data` is valid. Secondly, the
write operation also follows the same limitation. There is no signal that indicates the host that the data sent is written into the memory and the changes are
now reflected and can be viewed by issuing a read command.

### 2) Assumption that memory is always ready to do reads and writes
There is an assumption in the above protocol that memory will always be ready to service the request from the host in case of both reads or writes.
There are no signals either from the read port or from the write port that indicate that the memory is not ready to service the request from the host.

### 3) Assumption that host is always ready to receive data
The other assumption is that the host will always be ready to receive the response from the memory in case of reads (response data) or writes (write acknowledge).
There is no signal to indicate the memory that the host is not yet ready to receive a response from your side.

### 4) Reads and writes never fail
This fixed yet simple protocol assumes that the read operation or the write operation will never fail. So, by providing the `read_addr` it is guaranteed that after
some fixed clock cycles the data coming out from the `read_data` is valid and the operation did not result in any error. There is no signal available to indicate
errors for this operation. Same follows for the write operations as well. Issuing a write operation is assumed to changed the contents of the memory after
some fixed clock cycles and there is no signal to indicate that the operation failed to write and change the contents of the memory.

### 5) Reads and writes are issued one at a time
This interface supports issuing a read or a write request one at a time. So you would issue an address on the `read_addr` port wait some cycles, read the
data from the `read_data` port then issue another address on the `read_addr` port and so on. Same goes for the write operation as well.

So, AXI is generally the communication protocol that helps to overcome these limitations by providing a set of general ports for reads and writes.

## AXI to the rescue!
AXI allows the following functionality as compared to a fixed protocol discussed above:

* Reads and writes can have variable latencies.

* Memory can have the option to indicate the host that it is not ready to communicate.

* Host can have the option to indicate the memory that it is not ready to receive the data.

* Reads and writes to the memory can fail.

* Reads and writes can be issued in streams called _bursts_.

To provide such functionality AXI provides more general ports for reads and writes as compared to the one discussed above and groups them together called _channels_.

Here are the channels that the AXI provides:

<img class="axi channels" src="/img/soDLA-jpgs/axi-channels.png" /> 

Notice how we are using fat arrows to indicate that there are bundles of wires and multiple ports inside each channel. 

The `read address` channel itself contains multiple signals for providing address and other functionalities and the `read data` channel contains the
data read from the memory as well as some other signals which indicate if an error occured or not.

Same goes with the `write address` channel which is responsible for providing the address, the `write data` channel for providing the data to be written inside
the memory and an additional `write response` channel. This channel provides the indication whether the data was writting successfuly or not.

## AXI Read Burst
AXI allows reading the memory in contiguous locations and this feature is known as _bursts_. So for example if ten contiguous 4 bytes are to be read from the memory at address locations: 0,4,8,12,16,24,28,32,36,40 then this can be done by providing a starting address and the bytes to be read while communicating with the memory without providing the address again and again for every next 4 bytes. Each transfer that happens during a burst is called a _beat_. In AXI this kind of burst is known as _incremental burst_ where the address is calculated itself according to the fixed offset provided by the host.

<img class="axi-burst-read" src="/img/soDLA-jpgs/axi-burst-read.png" /> 

<img class="axi-burst-read" src="/img/soDLA-jpgs/axi-read-prototype.jpg" /> 

Let's look at the _incremental burst_ through the AXI read channel. We have two channels for a read operation _read address_ and _read data_. The reason of calling them channels is that each channel contains multiple signals for different purposes. 

### Handshaking mechanism during AXI Read Burst
Both the read channels, _read address_ and _read data_ contains some signals that are used as a handshaking protocol for the communication to begin between the host and the device.

<img class="axi-read-handshaking" src="/img/soDLA-jpgs/axi-read-handshaking.png" /> 

The `arvalid` and `arready` signals used in the _read address_ channel are the basic control signals for the handshaking protocol in the _read address_ channel. The `arready` signal is sent by the memory indicating the host that the device is ready to accept the host request and the `arvalid` signal is sent by the host indicating that the host is sending a valid request. Hence, when both `arready` and `arvalid` signals are high, i.e the device is ready to accept the request and the host is sending a valid request then the request transaction is assumed to be accepted and the communication can be established.

Same goes with the `rvalid` and `rready` signals used in the _read data_ channel. The `rready` signal this time is sent by the host indicating the memory that it is now ready to accept the response data from the memory and the `rvalid` signal indicates that the memory is sending a valid data. When both the signals are high it is assumed that the read data request is accepted and the memory can send a new data response to the host.

### Read burst configuration signals
For the read burst AXI has the following signals in the _read address_ channel as well as in the _read data_ channel:

<img class="axi-read-configuration" src="/img/soDLA-jpgs/axi-read-configuration.png" /> 

### AXI read address channel signals

| Name | From | To | Description |
| ---- | ---- | ---- | ----------  |
| arburst | Host | Device | the type of burst to do, INCREMENTAL, WRAP, FIXED in bit encodings | 
| araddr | Host | Device |  the start address of the burst | 
| arlen | Host | Device |  the number of transfers to perform in a burst | 
| arsize | Host | Device |  the number of bytes in each transfer. *Cannot be larger than the width of the data bus | 
| arready | Device | Host | handshaking protocol signals | 
| arvalid | Host | Device | handshaking protocol signals | 

AxLEN Encoding:

the burst length in AXI4 is defined as AxLEN[7:0] + 1, the burst length in AXI3 is defined as AxLEN[3:0] + 1

AxSIZE Encoding:

| AxSIZE | Bytes in transfer |
| ---- | ---- |
| 0b000 | 1 |
| 0b001 | 2 | 
| 0b010 | 4 |
| 0b011 | 8 |
| 0b100 | 16 |
| 0b101 | 32 |
| 0b110 | 64 |
| 0b111 | 128 |

### AXI read data channel signals

| Name | From | To | Description |
| ---- | ---- | ---- | ----------  |
| rdata | Device | Host | contains the actual data which the host requested | 
| rresp | Device | Host |  contains the response encoded in bits, indicating whether the read completed successfuly or failed | 
| rlast | Device | Host |  the flag indicating the data read is the last one in the read burst | 
| rready | Host | Device |  handshaking protocol signals  | 
| rvalid | Device | Host | handshaking protocol signals | 

### AXI read burst example

<img class="cora" src="/img/soDLA-jpgs/read_burst_example.jpg" /> 


## AXI Write Burst 

<img class="cora" src="/img/soDLA-jpgs/axi-write-burst.jpg" /> 

### AXI Write Stobe
<img class="cora" src="/img/soDLA-jpgs/axi-strobe-wire.jpg" /> 

### AXI Write Burst Example

AxLEN Encoding:

the burst length in AXI4 is defined as AxLEN[7:0] + 1, the burst length in AXI3 is defined as AxLEN[3:0] + 1

AxSIZE Encoding:

| AxSIZE | Bytes in transfer |
| ---- | ---- |
| 0b000 | 1 |
| 0b001 | 2 | 
| 0b010 | 4 |
| 0b011 | 8 |
| 0b100 | 16 |
| 0b101 | 32 |
| 0b110 | 64 |
| 0b111 | 128 |

<img class="cora" src="/img/soDLA-jpgs/axi-write-burst-example.jpg" /> 



