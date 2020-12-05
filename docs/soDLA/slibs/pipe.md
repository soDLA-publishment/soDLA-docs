# Pipe

----

## Bubble Collapse

```
class NV_NVDLA_BC_pipe(WIDTH:Int) extends Module

```

### Interfaces

```
val clk = Input(Clock()) 

val vi = Input(Bool())
val ro = Output(Bool())
val di = Input(UInt(WIDTH.W)) 
            
val vo = Output(Bool()) 
val ri = Input(Bool())
val dout = Output(UInt(WIDTH.W))
```

### Descriptions

```
//  pipe -bc                          
//   ----------------------------- 
//    di,vi,ro      
//    v     ^        
//    |     |       
//   _|_    |        
//  |>__|   |      
//    | - >(*)        
//    |     |      
//    |     |       
//    do,vo,ri

```

Insert one stage of registers on the path of `valid` and `data`, for the timing closure.


## Bubble Collapse for Vector Input

```
class NV_NVDLA_BC_VEC_pipe(DIM:Int, WIDTH:Int) extends Module 

```

### Interfaces

```
val clk = Input(Clock()) 

val di = Input(Vec(DIM, UInt(WIDTH.W)))
val vi = Input(Bool())
val ri = Input(Bool())

val dout = Output(Vec(DIM, UInt(WIDTH.W)))
val vo = Output(Bool()) 
val ro = Output(Bool())
```


### Descriptions

```
//  pipe -bc                          
//   ----------------------------- 
//    di,vi,ro      
//    v     ^        
//    |     |       
//   _|_    |        
//  |>__|   |      
//    | - >(*)        
//    |     |      
//    |     |       
//    do,vo,ri     
```

Insert one stage of registers on the path of `valid` and `vector of data`, for the timing closure.


## Bubble Collapse with Input Skid

```
class NV_NVDLA_BC_IS_pipe(WIDTH:Int) extends Module

```

### Interfaces

```
val clk = Input(Clock()) 

val vi = Input(Bool())
val ro = Output(Bool())
val di = Input(UInt(WIDTH.W)) 
    
val vo = Output(Bool()) 
val ri = Input(Bool())
val dout = Output(UInt(WIDTH.W))
```

### Descriptions

```
// ----------
// Basic Pipe
// ----------
// 
// SKID
//
//   -is                           
//   ----------------------------- 
//    di,vi              ro       
//      v                ^        
//    __|  __            |        
//   | _|_|_ |           |        
//   | \___/-|------     |        
//   |  _|_  | |  _|_   _|_       
//   | |>__| | | |>__| |>__|      
//   |__ |___| |   |     |        
//     _|_|_   |   ------|        
//     \___/---          |        
//      _|_______________|_       
//     |     bc pipe       |      
//     |___________________|      
//       |               |        
//       v               ^        
//    do,vo               ri      
```


Insert stages of registers on the path of `valid`, `ready`and `data`, by adding one skid before bc pipe, for the timing closure.


## Bubble Collapse with Output Skid


### Interfaces

```
val clk = Input(Clock()) 

val vi = Input(Bool())
val ro = Output(Bool())
val di = Input(UInt(WIDTH.W)) 
    
val vo = Output(Bool()) 
val ri = Input(Bool())
val dout = Output(UInt(WIDTH.W))
```

### Descriptions

```
// ----------
// Basic Pipe
// ----------
// 
// SKID
//
//   -is                           
//   ----------------------------- 
//    di,vi              ro       
//      v                ^     
//      _|_______________|_       
//     |      bc pipe      |      
//     |___________________|        
//    __|  __            |        
//   | _|_|_ |           |        
//   | \___/-|------     |        
//   |  _|_  | |  _|_   _|_       
//   | |>__| | | |>__| |>__|      
//   |__ |___| |   |     |        
//     _|_|_   |   ------|        
//     \___/---          |        
//       |               |        
//       v               ^        
//    do,vo               ri  
```    

Insert stages of registers on the path of `valid`, `ready`and `data`, by adding one skid after bc pipe, for the timing closure.





















