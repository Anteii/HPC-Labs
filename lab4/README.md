
# String Mass Search

## Table of contents

* [Description](#description)
* [Implementation description](#implementation-description)
* [Results](#results)

## Description
This jupyter notebook was executed in google colab for several reasons:
- It provides access to Linux VM with CUDA compatible GPU (Tesla T4 in my instance)
- Ability to compile and execute C++/CUDA programs
- Almost single interface to write and execute C++ and python (for visualization)
- Cloud platform & easy push to git

Jupyter notebook consists of four chapters:
- Notebook Setup (check CUDA version and isntance's GPU)
- Python (CPU) (python implementation of search algorithm)
- C++ (CPU & CUDA)
- Visualization (plots with python matplotlib)

All following text is applicable to C++ code only (not python).

## Implementation description

Features:
- Sequential and parallel implementations were nicely tangled in a single .cu file
- Modern C++ code (C++11) almost everywhere
- Sequential implementation was parameterized, thus alphabet may be of any type
- Text and substrings generation is held on CPU
- Substrings are generated by uniformly sampling characters from alphabet
- Atomic substraction in kernel
- Reproducible experiments (fixed random seed)

Algoritm (both versions) can be divided in three stages:
- make Map <character> <-> <list of pairs <substring_ind, pos_in_substring>> for all characters of an alphabet
- make Matrix of size substringNumber x textLength and fill each line with length of a corresponding substring
- for all characters (i=0..textLength) from the text iterate over a corresponding vector of pairs <substring_ind, pos_in_substring> and substract 1 from Matrix[substring_ind][i-pos_in_substring].

All zeros (with indices i, j) in Matrix are indicators of a i-th substring starting from j-th character in the text.

In parallel implementation Matrix modifications were performed on GPU. To do so we need to provide following data to the GPU:
- Text
- Matrix
- Map <character> <-> <list of pairs <substring_ind, pos_in_substring>>

As we can't just pass map to a kernel, it's been flatten in a array and splitted in two sections:
- Meta info. Contains triplets [character, offset to pairs, number of pairs].
- Pairs. Contains merged arrays of arbitary lengths with pairs.

Kernel itself is quite trivial, each thread correspond to a different character from a text and iterate over corresponding pairs. Not a best implementation, though still greatly outperform sequential algorithm.

## Results
Below are results obtained from experiments.

cpu_gpu_times_small_n_subs.png
speed_up_increasing_all.png
speed_up_increasing_text_size.png

<p align="center">
  Executions times for CPU and GPU versions<br>
  <img width="600" height="400" src="https://github.com/Anteii/HPC-Labs/blob/main/lab4/resources/cpu_gpu_times_small_n_subs.png"/>
</p>

<p align="center">
  Speed up for GPU version (increasing only text size)<br>????????
  <img width="600" height="400" src="https://github.com/Anteii/HPC-Labs/blob/main/lab4/resources/speed_up_increasing_text_size.png">
</p>

<p align="center">
  Speed up for GPU version (increasing text size, number of substrings, substring max length)<br>????
  <img width="600" height="400" src="https://github.com/Anteii/HPC-Labs/blob/main/lab4/resources/speed_up_increasing_all.png">
</p>
