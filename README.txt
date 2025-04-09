Nicholas Llaurado

REPORT

Experimental Data:

From the data we can see that runtime is directly related to threshold. As the threshold decreases, runtime shortens until a certian point where the runtime hits a minimum and no longer decreases. 
This is expected as parallelism inherently has diminishing returns. A closer look shows that a threshold of 2097152 is nearly sequential and gives the longest runtime. Parallelism begins giving an advantage around 1048576 and continues until it reaches its maximum benefit at 262144–65536 with realtime values from 0.17–0.20s. Any thresholds below that produce stagnated runtimes. 
The improvement in runtime diminishes at lower thresholds because of forking for very small partitions. Each fork requires CPU time and memory opertaions which results in more time spent managing these processes than actually sorting the data. Each fork requires the CPU to switch between processes which creates overhead. Overhead is the extra work the cpu has to do to such as context switching between each task and synchronization from waiting for each child process. 
This data shows just how much parallel processing can help before overhead causes detriment. We can see the threshold range which provides the best balance. 
For this setup, the quicksort function forks to create a child to handle the left side of the array while the parent continues with the right. This means that for an array of length n, the first CPU core will fork the computation process of [0, n/2] to a second CPU core, which then forks the process of [0,n/4] to a third core which also handles the parent sorting of [n/2, n]. This process continues recursively, mapping the remainging partitions of the array to other cpu cores. 
If we were to use fork to create a chile for the left and another for the right, this would create more overhead and stress the CPU. Forking only one side creates a more balanced approach. 

Test run with threshold 2097152

real    0m0.930s
user    0m0.887s
sys     0m0.031s
Test run with threshold 1048576

real    0m0.396s
user    0m0.654s
sys     0m0.050s
Test run with threshold 524288

real    0m0.286s
user    0m0.698s
sys     0m0.070s
Test run with threshold 262144

real    0m0.203s
user    0m0.675s
sys     0m0.064s
Test run with threshold 131072

real    0m0.187s
user    0m0.671s
sys     0m0.085s
Test run with threshold 65536

real    0m0.175s
user    0m0.693s
sys     0m0.119s
Test run with threshold 32768

real    0m0.188s
user    0m0.767s
sys     0m0.167s
Test run with threshold 16384

real    0m0.171s
user    0m0.743s
sys     0m0.227s
