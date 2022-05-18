Cluster Management System LAD/SLURM
SIMPLE USER MANUAL
Last update: 21/03/2022

1.  ladcomp

        Compiles of a parallel application to be executed in LAD cluster.

        Usage: ladcomp -env <environment> <sourcefile> -o <executable>

        Parameters:
        -env <environment>      compilation environment (
                                                mpicc for MPI over using C,
                                                mpiompcc for MPI over using C with openmp,
                                                mpiCC for MPI using C++,
                                                mpiompCC for MPI using C++ with openmp,
                                                intelMPI for MPI using Intel C/C++ or
                                                intelMPIomp for MPI using Intel C/C++ with openmp)

        Example: ladcomp -env mpiCC hello.c -o hello

2.  ladalloc

        Allocates a set of nodes in a cluster for a specific amount of time in exclusive or shared mode.
        Exclusive mode means that all cores per node allocated are dedicated to user job.
        Shared mode means that just 1 core per node allocated is dedicated (and available) to user job. Remaining
        cores are not used and stay free for other allocations.
        After completion, the user shell is redirected to the first node of the set of allocated nodes.
        If the user leaves the shell (using exit), the application under execution, if any,  will be aborted,
        and the allocated nodes are released.

        Usage: ladalloc -n <nodes> -t <time> [-s|-e]

        Parameters:
        -n <nodes>              number of nodes
        -t <time>               amount of time for allocation
        [-s|-e]                 shared (1 core/node allocation)
                                or exclusive mode (all cores/node allocation)

        Example: ladalloc -n 3 -t 15 -e

3.  ladrun

        Executes an application in a LAD cluster using allocated nodes.

        Usage: ladrun [-np <number_of_process>] <executable>

        Parameters:
        -np <number_of_process>         number of processes used to execute the application - if not
                                                specified, it use the maximum number of cores available

        Example: ladrun -np 2 ./hello

4.  ladinfo

        Shows information about the LAD cluster.

        Usage: ladinfo

        Example: ladinfo

5.  ladrls

        Delete job(s) from current user

        Usage: ladrls -j <job_id> | -u

        Parameters:
        -j <job_id>             Cancel specific job
        -u                      Candel all jobs from current user

        Example: ladrls -j 105

6.  ladnodes

        Shows detailed information about your running job(s): JOBID, USER, NAME, NODE, CPUS, MIN_MEMORY, START_TIME, END_TIME

        Usage: ladnodes

7.  ladqueue

        Shows detailed information about queue of requests jobs: list all running and waiting jobs on LAD cluster

        Usage: ladqueue
