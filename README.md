# Summary

## Benchmarking Results Summary

| **Benchmark**                            | **Kernel Time (s)** | **FPS (Kernel)** | **GFLOPS (Kernel)** | **Total Time (s)** | **FPS (Total)** |
| ---------------------------------------- | ------------------- | ---------------- | ------------------- | ------------------ | --------------- |
| **0. Naive CPU**                         | 0.157471            | 6.35             | 0.479               | -                  | -               |
| **1. Naive GPU**                         | 0.000490849         | 2037.29          | 153.81              | 0.0152102          | 65.75           |
| **2. GPU Constant Memory**               | 0.00041218          | 2426.13          | 183.166             | 0.0139474          | 71.70           |
| **3. GPU Tiled Processing**              | 0.000467193         | 2140.44          | 161.598             | 0.0142315          | 70.27           |
| **4. GPU with Pinned Memory**            | 0.000491125         | 2036.14          | 153.724             | 0.0140036          | 71.41           |
| **5. GPU Const Mem + Pinned Mem**        | 0.000411405         | 2430.69          | 183.511             | 0.0133342          | 74.99           |
| **6. GPU Tiled Processing + Pinned Mem** | 0.000467578         | 2138.68          | 161.465             | 0.0133539          | 74.88           |

---

## **Objective Key Insights**

### **1. GPU acceleration achieves a massive speedup over the CPU**

-   The **Naive GPU kernel** is **321x faster** than the **Naive CPU** (0.157471s vs. 0.000490849s).

### **2. Using Constant Memory Improves Performance**

-   GPU constant memory (**Exp. 2**) reduces kernel execution time by **16%** compared to Naive GPU (**0.00041218s vs. 0.000490849s**).
-   FPS increased by **19%** (**2426.13 vs. 2037.29**).
-   GFLOPS increased by **19%** (**183.166 vs. 153.81**).

### **3. Tiled Processing Also Boosts Efficiency**

-   Tiling (**Exp. 3**) improves performance over naive GPU but is **9% slower** than constant memory.
-   GFLOPS in **Exp. 3** is **161.598**, compared to **183.166** in **Exp. 2**.

### **4. Pinned Memory Significantly Reduces Total Execution Time**

-   GPU with pinned memory (**Exp. 4**) reduces **total execution time** by **8%** over Naive GPU (**0.0140036s vs. 0.0152102s**).
-   FPS (total) improves from **65.75 to 71.41**.

### **5. Best Overall Performance: GPU with Both Constant and Pinned Memory (Exp. 5)**

-   **Fastest kernel execution time**: **0.000411405s**.
-   **Highest FPS (kernel)**: **2430.69** (19% increase over Naive GPU).
-   **Highest GFLOPS (kernel)**: **183.511**.

### **6. Pinned Memory + Tiled Processing (Exp. 6) Performs Slightly Worse than Exp. 5**

-   **Kernel time in Exp. 6** is **13.6% slower** than Exp. 5 (**0.000467578s vs. 0.000411405s**).
-   **FPS (total) is nearly identical**: **74.88 (Exp. 6) vs. 74.99 (Exp. 5)**.

---

## **Benchmarking Details**

### **0. Naive CPU Benchmark**

```bash
g++ -o ../output/00/cpu_benchmark 00_cpu_conv2d_benchmark.cpp ../src/cpu_conv2d.cpp ../src/utils.cpp -Iinclude -Ilib -O2
```

**Result:**

```
CPU Benchmarking details
------------------------
Time for kernel execution (seconds): 0.157471
FPS (total): 6.35037
GFLOPS (kernel): 0.479437
```

---

### **1. Naive GPU Benchmark**

```bash
nvcc -arch=sm_75 -rdc=true -o ../output/01/gpu_benchmark 01_gpu_conv2d_benchmark.cu ../src/gpu_conv2d.cu ../src/utils.cpp -Iinclude -Ilib -O2 -w
```

**Result:**

```
GPU Benchmarking details
------------------------
Time (kernel): 0.000490849
FPS (kernel): 2037.29
GFLOPS (kernel): 153.81
Time (total): 0.0152102
FPS (total): 65.7455
```

---

### **2. GPU Constant Memory Benchmark**

```bash
nvcc -arch=sm_75 -rdc=true -o ../output/02/gpu_constMem 02_gpu_conv2d_constMem_benchmark.cu ../src/gpu_conv2d_constMem.cu ../src/utils.cpp -Iinclude -Ilib -O2 -w
```

**Result:**

```
GPU (constant memory) Benchmarking details
------------------------------------------
Time (kernel): 0.00041218
FPS (kernel): 2426.13
GFLOPS (kernel): 183.166
Time (total): 0.0139474
FPS (total): 71.6981
```

---

### **3. GPU Tiled Processing Benchmark**

```bash
nvcc -arch=sm_75 -rdc=true -o ../output/03/gpu_tiled 03_gpu_conv2d_tiled_benchmark.cu ../src/gpu_conv2d_tiled.cu ../src/utils.cpp -Iinclude -Ilib -O2 -w
```

**Result:**

```
GPU (Tiling) Benchmarking details
---------------------------------
Time (kernel): 0.000467193
FPS (kernel): 2140.44
GFLOPS (kernel): 161.598
Time (total): 0.0142315
FPS (total): 70.2666
```

---

### **4. Naive GPU and CPU Pinned Memory Benchmark**

```bash
nvcc -arch=sm_75 -rdc=true -o ../output/04/gpu_pinned 04_gpu_conv2d_pinnedMem_benchmark.cu ../src/gpu_conv2d.cu ../src/utils.cpp -Iinclude -Ilib -O2 -w
```

**Result:**

```
GPU (Pinned) Benchmarking details
---------------------------------
Time (kernel): 0.000491125
FPS (kernel): 2036.14
GFLOPS (kernel): 153.724
Time (total): 0.0140036
FPS (total): 71.41
```

---

### **5. GPU Constant Memory and CPU Pinned Memory Benchmark**

```bash
nvcc -arch=sm_75 -rdc=true -o ../output/05/gpu_pinned_constMem ...
```

**Result:**

```
Time (kernel): 0.000411405
FPS (kernel): 2430.69
GFLOPS (kernel): 183.511
Time (total): 0.0133342
FPS (total): 74.995
```
