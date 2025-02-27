[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: Yibo Xu
### Student Id: 21071703
### Email: yxufb@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

### The measurement tool I used for this task is Phoronix Test Suite v10.8.4.  
#### Configuration of the Measurement Tool:  

1. **CPU Performance Test**:  
   - **Configuration**:  
     The CPU test uses the command `phoronix-test-suite run pts/compress-7zip` with default parameters.  
   - **Reason for Configuration**:  
     The default settings ensure consistency across EC2 instance comparisons. 7-Zip is chosen because it is a widely recognized benchmark for CPU performance, reflecting real-world workloads like data processing.  
   - **Result Interpretation**:  
     The output provides MIPS, MIPS stands for Million Instructions Per Second, which represents the number of compression operations completed per second. **Higher scores indicate better CPU performance**. For example, If a CPU achieves 4125 MIPS in a compression test, it means the CPU can execute 4.125 billion instructions per second for that specific workload.

2. **Memory Performance Test**:  
   - **Configuration**:  
     The memory test uses the command `phoronix-test-suite run pts/ramspeed`.
     with Type: 6: All memory operation types (Copy, Scale, Add, Triad, Average) will be tested and Benchmark: 3: Both integer and floating-point benchmarks will be run.
      ![image](https://github.com/user-attachments/assets/3527e4d7-8235-440b-b193-94903353f25f)
   - **Reason for Configuration**:  
     Type: 6: Running all memory operations provides a complete picture of the memory subsystem's performance.
     Benchmark: 3: Running both integer and floating-point benchmarks ensures the memory performance is tested for workloads that rely on different data types. Integer operations are common in general-purpose computing, while floating-point operations are critical in scientific and engineering applications.
 

   - **Result Interpretation**:  
     The output reports **memory speed in MB/s**, indicating how quickly the RAM can read/write data. **Higher values denote better memory performance**. For instance, a result of `15,000 MB/s` means the memory subsystem transferred 15 GB of data per second.
     
2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | Compression: 4125 MIPS Decompression: 3169 MIPS| Integer Average: 10,592.56 MB/s, Float Average: 10,468.35 MB/s            |
    | `t2.medium`| Compression: 9806 MIPS Decompression: 5914 MIPS| Integer Average: 19,284.87 MB/s, Float Average: 19,468.35 MB/s            |
    | `c5d.large`| Compression: 7783 MIPS Decompression: 5209 MIPS| Integer Average: 13,699.20 MB/s, Float Average: 13,555.18 MB/s            |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |  4170 Mbps     |  0.182   |
    | `m5.large` - `m5.large`   |  4930 Mbps     |  0.166   |
    | `c5n.large` - `c5n.large` |  9350 Mbps     |  0.101   |
    | `t3.medium` - `c5n.large` |  2510 Mbps     |  0.568   |
    | `m5.large` - `c5n.large`  |  4950 Mbps     |  0.161   |
    | `m5.large` - `t3.medium`  |  2230 Mbps     |  0.585   |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |   32.8 Mbps    |  62.5    |
    | N. Virginia - N. Virginia |   4860 Mbps    |  0.208   |
    | Oregon - Oregon           |                |          |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
