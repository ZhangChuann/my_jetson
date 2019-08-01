# 1. 官方文档介绍信息

<img src="https://github.com/ZhangChuann/my_jetson/raw/master/docs/images/3_nano_config.png">

官网介绍Nano为128 Cuda core, 472GFLops float16.
算力 = cuda_core x Boost_Frequency x 4 = 472GFLOPS;
这里需要乘4，猜想是针对half float，一个cycle可以完成四个计算；

# 2. CUDA命令解析信息
需要先添加nvcc到环境变量才能自己编译运行cuda程序：添加如下三行到~/.bashrc

```
export PATH=/usr/local/cuda-10.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda-10.0
```
source ~/.bashrc
nvcc -V 查看是否成功

## 2.1 查看使用情况
```
~/workspace/my_jetson/our_app$ tegrastats
RAM 590/3965MB (lfb 708x4MB) 
CPU [3%@102,0%@102,0%@102,1%@102] 
EMC_FREQ 0% 
GR3D_FREQ 0% 
PLL@26C 
CPU@27.5C 
PMIC@100C 
GPU@28C 
AO@38C 
thermal@27.75C 
POM_5V_IN 888/888 
POM_5V_GPU 0/0 
POM_5V_CPU 121/121

```
## 2.2 运行bandwidthTest程序查看带宽信息

```
[CUDA Bandwidth Test] - Starting...
Running on...

 Device 0: NVIDIA Tegra X1
 Quick Mode

 Host to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)        Bandwidth(MB/s)
   33554432                     6374.2

 Device to Host Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)        Bandwidth(MB/s)
   33554432                     9196.7

 Device to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)        Bandwidth(MB/s)
   33554432                     15739.3

 PAGEABLE Memory Transfers
   Transfer Size (Bytes)        Bandwidth(MB/s)
   33554432                     2188.8

 Device to Host Bandwidth, 1 Device(s)
 PAGEABLE Memory Transfers
   Transfer Size (Bytes)        Bandwidth(MB/s)
   33554432                     1513.9

 Device to Device Bandwidth, 1 Device(s)
 PAGEABLE Memory Transfers
   Transfer Size (Bytes)        Bandwidth(MB/s)
   33554432                     13694.7


Result = PASS

NOTE: The CUDA Samples are not meant for performance measurements. Results may vary when GPU Boost is enabled.

```

## 2.3 运行deviceQuary 查看GPU性能

```
./deviceQuery Starting...

 CUDA Device Query (Runtime API) version (CUDART static linking)

Detected 1 CUDA Capable device(s)

Device 0: "NVIDIA Tegra X1"
  CUDA Driver Version / Runtime Version          10.0 / 10.0
  CUDA Capability Major/Minor version number:    5.3
  Total amount of global memory:                 3965 MBytes (4157145088 bytes)
  ( 1) Multiprocessors, (128) CUDA Cores/MP:     128 CUDA Cores
  GPU Max Clock rate:                            922 MHz (0.92 GHz)
  Memory Clock rate:                             13 Mhz
  Memory Bus Width:                              64-bit
  L2 Cache Size:                                 262144 bytes
  Maximum Texture Dimension Size (x,y,z)         1D=(65536), 2D=(65536, 65536), 3D=(4096, 4096, 4096)
  Maximum Layered 1D Texture Size, (num) layers  1D=(16384), 2048 layers
  Maximum Layered 2D Texture Size, (num) layers  2D=(16384, 16384), 2048 layers
  Total amount of constant memory:               65536 bytes
  Total amount of shared memory per block:       49152 bytes
  Total number of registers available per block: 32768
  Warp size:                                     32
  Maximum number of threads per multiprocessor:  2048
  Maximum number of threads per block:           1024
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
  Maximum memory pitch:                          2147483647 bytes
  Texture alignment:                             512 bytes
  Concurrent copy and kernel execution:          Yes with 1 copy engine(s)
  Run time limit on kernels:                     Yes
  Integrated GPU sharing Host Memory:            Yes
  Support host page-locked memory mapping:       Yes
  Alignment requirement for Surfaces:            Yes
  Device has ECC support:                        Disabled
  Device supports Unified Addressing (UVA):      Yes
  Device supports Compute Preemption:            No
  Supports Cooperative Kernel Launch:            No
  Supports MultiDevice Co-op Kernel Launch:      No
  Device PCI Domain ID / Bus ID / location ID:   0 / 0 / 0
  Compute Mode:
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 10.0, CUDA Runtime Version = 10.0, NumDevs = 1
Result = PASS
```
## 2.4 运行UnifiedMemoryPerf用例

```
GPU Device 0: "NVIDIA Tegra X1" with compute capability 5.3

Running ........................................................

Overall Time For matrixMultiplyPerf

Printing Average of 20 measurements in (ms)
Size_KB  UMhint UMhntAs  UMeasy   0Copy MemCopy CpAsync CpHpglk CpPglAs
4         0.308   0.944   0.200   0.208   0.365   0.242   0.544   0.314
16        0.786   1.501   0.515   0.630   0.615   0.501   1.046   0.804
64        1.968   2.871   2.909   3.444   2.790   2.481   3.598   3.174
256       5.254   7.342  10.568  12.865  20.085  20.509  23.544   8.305
1024     26.805  29.484  30.431  34.155  25.238  25.648  38.818  37.833
4096     98.402  97.441  96.255 158.547 100.545  97.850 166.603 167.053
16384   603.977 600.533 614.032 889.940 620.057 617.803 909.230 891.692

```

