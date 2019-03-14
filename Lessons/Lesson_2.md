-- *Slide* --
### Part 0: Goals for Morning, Day 2
* Part 1: Architecture, and Parallel Limits
* Part 2: Distributed Memory Programming with OpenMPI
* Part 3: Profiling and Debugging
-- *Slide End* --

-- *Slide* --
### Part 1: Parallel Programming Strategy
#### "Taking control is the key strategy!"
-- Dr. Rolf Rabenseifner, High Performance Computing Center, Stuttgart
-- *Slide End* --

-- *Slide* --
### Part 1: Flynn's Taxonomy
* The type parallelisation can be determined by Flynn's taxonomy of computer Systems (1966), where each process is considered as the execution of a pool of instructions (instruction stream) on a pool of data (data stream), and with these streams are single or multiple. 
-- *Slide End* --

-- *Slide* --
### Part 1: Flynn's Taxonomy
* Four basic possibilities: Single Instruction Stream, Single Data Stream (SISD), Single Instruction Stream, Multiple Data Streams (SIMD), Multiple Instruction Streams, Single Data Stream (MISD), Multiple Instruction Streams, Multiple Data Streams (MIMD)
-- *Slide End* --

-- *Slide* --
### Part 1: Processors and Cores
* A processor is a physical device that accepts data as input and provides results as output. A uniprocessor system has one such general purpose device. 
* A unicore processor carries out the usual functions of a CPU, according to the instruction set. A multicore processor has independent central processing units ('cores') integrated a single integrated circuit die or a single chip package.
-- *Slide End* --

-- *Slide* --
### Part 1: Threads and Strands
* A process provides the resources to execute an instance of a program (such as address space, the code, handles to system objects, a process identifier etc).  An execution thread is the smallest processing unit in an operating system, contained inside a process. 
* There are also hardware threads (aka "strands"). Determine this from the output of `lscupu` and view the `NUMA node` information.
-- *Slide End* --

-- *Slide* --
### Part 1: Multicore Drivers
<img src="https://raw.githubusercontent.com/UoM-ResPlat-DevOps/SpartanParallel/master/Images/intelcpu.png" />
-- *Slide End* --

-- *Slide* --
### Part 1: Multicore Drivers
* Datasets are becoming larger than computers are improving.
* Effective clock speeds have flattened due to heat and power.
* Increasing gap between memory and processing speeds. 
-- *Slide End* --

-- *Slide* --
### Part 1: Multicore Drivers
* Symmetric multiprocessing (multi-processors, shared memory) is well established technology (IBM System/360, 1964)
* Tilera have developed 64 core (TILE64, 2007), and then a 100 core processor (2009). Founder Dr. Anant Agarwal leads the MIT Angstron Project to develop a 1,000 core processor (2012). NViDIA P100 3584 CUDA Cores (2016)
-- *Slide End* --

-- *Slide* --
### Part 1: Memory Distribution
* In a  multiprocessor computer system memory can either be distributed or shared. Memory coherence is an issue is shared memory environments.
* HPC Clusters use memory distributed between compute nodes and shared within compute nodes.
-- *Slide End* --

-- *Slide* --
### Part 1: Memory Distribution
* Operating systems like Plan 9 from Bell Labs creates a network function as a single collection of system resources.
* OpenMP uses shared memory parallelism; MPI uses distributed memory parallelism. The latter can cross compute nodes.
-- *Slide End* --

-- *Slide* --
### Part 1: Speedup and Locks
* The speedup of parallelism can be measured: Speedup (p) = Time (serial)/ Time (parallel). Ideal (linear) speedup is S(p) = p.
* Correctness requires requires synchronisation (locking). Synchronisation and atomic operations causes loss of performance, communication latency. 
-- *Slide End* --

-- *Slide* --
### Part 1: Deadlocks and Livelocks
* "When two trains approach each other at a crossing, both shall come to a full stop and neither shall start up again until the other has gone." (apocryphal Kansas railway statute)
* "When two people approach each other in a crowded corridor, both shall move out of the way of the other, and shall continue to move until they have an open path for progress" (Polite Persons in a Corridor Problem)
-- *Slide End* --

-- *Slide* --
### Part 1: Amdahl's Law 
* Because some of the task is in serial, there is a limit to the speedup based on the time that is required for the sequential task - no matter how many processors are thrown at the problem.
-- *Slide End* --

-- *Slide* --
<img src="https://raw.githubusercontent.com/UoM-ResPlat-DevOps/SpartanParallel/master/Images/amdhal.png" />
-- *Slide End* --

-- *Slide* --
### Part 1: Gustafson-Barsis Law
* Gene Amdahl proposed his law in 1967; it wasn't until over twenty years later in 1988 that an alternative by John L. Gustafson and Edwin H. Barsis was offered. 
* Amadahl's Law assumed a computation problem of fixed data set size. Programmers tend to set the size of their computational problems according to the available equipment; therefore as faster and more parallel equipment becomes available, larger problems can be solved.
-- *Slide End* --

-- *Slide* --
### Part 2: Message Passing
* The core principle is that many processors should be able cooperate to solve a problem by passing messages to each through a common communications network. 
* It does require additional programmer effort. Additional routines act as wrappers to existing compilers.
-- *Slide End* --

-- *Slide* --
### Part 2: The Communications World
* The basic principle behind MPI is to intiate a communications world, assign ranks to each member of that communications world, carry out the necessary tasks and close the communications world. 
* The examples `mpi-helloworld.c` and `mpi-helloworld.f90`
-- *Slide End* --

-- *Slide* --
### Part 2: Send and Receiving
* There are several routines for sending and receiving information in MPI. In each of these a number of parameters must be included, including the buffer, the rank of the sender, the rank of the receiver, a communication notice, a tag.
* If the MPI communications world is analoguous to a postal service, this information is the equivalent of addressing an envelope.
* The basic example is `mpi-sendrecv.c` and `mpi-sendrecv.f90`.
-- *Slide End* --

-- *Slide* --
### Part 2: Send and Receive Options
* `MPI_Status()` MPI_Status is not a routine, but  rather a data structure and is typically attached to an MPI_Recv() routine.
* `MPI_Ssend()` MPI_Ssend performs a synchronous-mode, blocking send. Whereas MPI_Send will not return until the program can use the send buffer,  MPI_Ssend will no return until a matching receive is posted.
-- *Slide End* --

-- *Slide* --
### Part 2: Send and Receive Options
|Send Mode | Explanation | Benefits  |Problems      |
|:---------|:------------|:----------|:-------------:|
|MPI_Send() | Standard send. May be synchronous or buffering | Flexible tradeoff; automatically uses buffer if available, but goes for synchronous if not. | Can hide deadlocks, uncertainty of type makes debugging harder. |
-- *Slide End* --

-- *Slide* --
|Send Mode | Explanation | Benefits  |Problems      |
|:---------|:------------|:----------|:-------------:|
| MPI_Ssend() | Synchronous send. Doesn't return until receive has also completed. | Safest mode, confident that message has been received. | Lower performance, especially without non-blocking. |
-- *Slide End* --

-- *Slide* --
|Send Mode | Explanation | Benefits  |Problems      |
|:---------|:------------|:----------|:-------------:|
| MPI_Bsend() | Buffered send. Copies data to buffer, program free to continue whilst message delivered later. | Good performance. Need to be aware of buffer space. | Buffer management issues. |
-- *Slide End* --

-- *Slide* --
|Send Mode | Explanation | Benefits  |Problems      |
|:---------|:------------|:----------|:-------------:|
| MPI_Rsend() | Receive send. Message must be already posted. | Slight performance increase since there's no handshake. | Risky and difficult to design. |
-- *Slide End* --

-- *Slide* --
### Part 2: Collective Communications
* MPI can also conduct collective communications.  These include MPI_Broadcast, MPI_Scatter, MPI_Gather, MPI_Reduce, and MPI_Allreduce. 
* MPI_Bcast Broadcasts a message from the process with rank "root" to all other processes of the communicator, including itself. It is significantly more prefereable than using a loop.
-- *Slide End* --

-- *Slide* --
### Part 2: Collective Communications
* MPI_Scatter sends data from one task to all tasks in a group; the inverse operation of MPI_Gather. The outcome is as if the root executed n send operations and each process executed a receive. MPI_Scatterv scatters a buffer in parts to all tasks in a group.
-- *Slide End* --

-- *Slide* --
<img src="https://raw.githubusercontent.com/UoM-ResPlat-DevOps/SpartanParallel/master/Images/scatter.png" />
<img src="https://raw.githubusercontent.com/UoM-ResPlat-DevOps/SpartanParallel/master/Images/gather.png" />
-- *Slide End* --

-- *Slide* --
### Part 2: Reductions
* MPI_Reduce performs a reduce operation (such as sum, max, logical AND, etc.) across all the members of a communication group.
* MPI_Allreduce conducts the same operation but returns the reduced result to all processors.
-- *Slide End* --

-- *Slide* --
### Part 2: Reductions
* The general principle in Reduce and All Reduce is the idea of reducing a set of numbers to a small set via a function. If you have a set of numbers (e.g., [1,2,3,4,5]) a reduce function (e.g., sum) can convert that set to a reduced set (e.g., 15). MPI_Reduce takes in an array of values as that set and outputs the result to the root process. MPI_AllReduce outputs the result to all processes.
* Examples; `mpi-particle.f90` `mpi-particle.f90`
-- *Slide End* --

-- *Slide* --
### Part 2: Reductions
<img src="https://raw.githubusercontent.com/UoM-ResPlat-DevOps/SpartanParallel/master/Images/reduce.png" /> 
-- *Slide End* --

-- *Slide* --
### Part 2: Reductions
<img src="https://raw.githubusercontent.com/UoM-ResPlat-DevOps/SpartanParallel/master/Images/allreduce.png" />
-- *Slide End* --

-- *Slide* --
### Part 2: Reduction Operators
| MPI_Name  | Function |
|:----------|:---------|
| MPI_MAX   | Maximum  |
| MPI_MIN   | Minimum  |
| MPI_SUM   | Sum      |
| MPI_PROD  | Product  |
| MPI_LAND  | Logical AND |
-- *Slide End* --

-- *Slide* --
### Part 2: Reduction Operators
| MPI_Name  | Function |
|:----------|:---------|
| MPI_BAND  | Bitwise AND |
| MPI_LOR   | Logical OR  |
| MPI_BOR   | Bitwise OR |
| MPI_LXOR  | Logical exclusive OR |
| MPI_BXOR  | Bitwise exclusive OR |
| MPI_MAXLOC | Maximum and location |
| MPI_MINLOC | Miniumun and location|
-- *Slide End* --

-- *Slide* --
### Part 3: Common Mistakes
* Syntax errors (e.g., missing or wrong terminators, wrong qutotes)
* Compiler can catch these.
* Semantic errors (e.g., crossing array bounds, wrong scope, memory leaks)
* Compiler won't catch these - means unexpected behaviour.
-- *Slide End* --

-- *Slide* --
### Part 3: Parallel Code
* Race conditions. 
* Deadlocks and livelocks.
* Start with working serial code - then incrementally make parallel.
* Basic debugging examples at `/usr/local/common/Debug`.
-- *Slide End* --

-- *Slide* --
### Part 3: Valgrind
* Valgrind is a debugging suite that automatically detects many memory management and threading bugs. 
* Typically built for serial applications, it can also be built with mpicc wrappers for GCC and Intel.
* Use the same compiler used in both the build and the Valgrind test. Turn on debgugging information at compilation.
* `mpicc -g mpi-debug.c -o mpi-debug` or `mpif90 -g mpi.debug.f90 -o mpi-debug` then `mpiexec -np 2 valgrind ./mpi-sendrec 2> valgrind.out`
-- *Slide End* --

-- *Slide* --
### Part 3: GDB
* GDB, the GNU Project debugger, allows you to see what happens to a program while it executes.
* Stepwise operation on code.
* `module load GDB`, `mpiexec -np 2 gdb --exec=mpi-debug --command=gdb.cmd`
* Attach to individual processes and stepwise. 
* Add `printf("PID %d on %s ready for attach\n", getpid(), hostname);`, then `mpiexec -np 2 mpi-debug` and `gdb -p $PID` 
-- *Slide End* --

### Part 4: Gprof
* Gprof, the GNU profiler, provides a flat profile (total execution time in each function) and and the call graph, which shows who called a function (parent) and who it called (child subroutines).
* Instrumention code is inserted with the `-pg` option at compilation. The `gprof` tool can be run against the executatble and output. e.g.,
* `gcc -Wall -pg test_gprof.c test_gprof_new.c -o test_gprof`, `./test_grof`, then `gprof test_gprof gmon.out > analysis.txt`


-- *Slide* --
### Part 4: PDT and TAU
* TAU (Tuning and Analysis Utilities) is a portable profiling and tracing toolkit.
* Parallel Performance Issues include the following: Coverage - % of the code that is parallel., Granularity - Amount of work in each section
Load Balancing., Locality - Communication structure., Synchronization - Locking latencies
* Load with `pprof` and `paraprof`
-- *Slide End* --

-- *Slide* --
-- *Slide End* --

-- *Slide* --
<img src="https://raw.githubusercontent.com/UoM-ResPlat-DevOps/SpartanIntro/master/Images/hypnotoad.png" width="150%" height="150%" />
-- *Slide End* --

