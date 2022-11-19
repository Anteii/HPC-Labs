# Lab report (Vector reduction)

## Table of contents

* [Description](#description)
* [Implementation description](#implementation-description)
* [Results](#results)

## Description
This jupyter notebook was executed in google colab for several reasons:
- It provides access to Linux VM with CUDA compatible GPU (Tesla T4 in my instance)
- Ability to compile and execute C++/CUDA programs
- Cloud platform

All cells that starts with magic %%cu is interpreted as C++/CUDA code and therefore compiled with nvcc

Jupyter notebook consists of three chapters:
- Notebook Setup (installing necessary pluging and GPU test)
- Vector reduction (C++/CUDA program implementation and execution)
- Visualization (plots with python matplotlib)

## Implementation description

Implementation is placed in chapter "Vector reduction" in the jupyter notebook.

Features:
- Reduction for given vector size is conducted for several random vectors. Considered mean time of all runs.
- Arbitrary vector size
- Reduction in shared memory
- One kernel - two invocations
- Type of result is long
- Reproducible experiments (fixed random seed)

The folowing auxiliary functions were declared and defined :
- randomFill (samples vector from normal distribution)
- serializeArr (writes vector to a txt file)
- checkError (detect whether error occurred after CUDA call)
- printVec (print vector in a console)
- experiment (incapsulates single experiment logic, conducts experiment several time, considers mean time of all runs)

Reduction functions:
- reduceVecCpu (vector reduction on CPU, sequential algorithm)
- reduceVecGpu (manages device memory, communications, invocates kernel, measures time)
- reduceVecKernel (kernel)

Kernel is parameterized (with C++ templates) for the following reasons:
- Size of planned to allocate shared memory should be known at a compile time
- Types of vector and buffer may vary (int/long)

Ruduction algoritm can be illustrated with simple image

<p align="center">
  <img width="420" height="280" src="https://github.com/Anteii/HPC-Labs/blob/main/lab2/resources/reduction.png">
</p>

In image:
- Orange parts are blocks
- Purple is a grid

In first kerenl invocation we reduce vector to the grid size and than to the block size.

In second we reduce it to a scalar value.

This way in the first step we can keep all threads busy and in the second we leverage fast (compared to global) shared memory. 

## Results
There were conducted experiments with different vectors sizes.



<p align="center">
  Reduction time for CPU and GPU
  <img width="600" height="300" src="https://github.com/Anteii/HPC-Labs/blob/main/lab2/resources/times_of_dim.png"/>
</p>


<p align="center">
       Speed up for GPU version     
  <img width="600" height="300" src="https://github.com/Anteii/HPC-Labs/blob/main/lab2/resources/speed_up_of_dim.png">
</p>
