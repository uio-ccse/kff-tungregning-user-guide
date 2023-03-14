# Submitting jobs
We use the queueing system [Slurm](https://slurm.schedmd.com/documentation.html) on most of the cluster nodes. The job is queueing until the requested resources are available. Long simulations should always be executed via Slurm on bigfacet, hugefacet and greatfacet, but short test simulations may be run without Slurm. 

There are three ways to submit jobs to the queue, with different applications: `srun`, `sbatch` and `salloc`.

## [srun](https://slurm.schedmd.com/srun.html)
The `srun` command is used when a job has to be run in an interactive way. This means that the job outputs directly to the terminal, like it would if it was executed without Slurm. A consequence is that the terminal cannot be used while the job is running, which is inconvinient if the queue is long or the job takes long time.

### Syntax
``` bash
srun <srun-arguments> <executable>
```

### An example
An example where an executable `lmp` runs on two nodes á 16 CPUs looks like this:
``` bash
srun --partition=normal --job-name=CPU-job --ntasks=32 --nodes=2 mpirun lmp
```

## [sbatch](https://slurm.schedmd.com/sbatch.html)
The `sbatch` command is used when running jobs in the background. This means that what would usually output to the terminal is stored in a text file with default name `slurm-<job-id>.out`. Moreover, `sbatch` requires a job script where SBATCH arguments are used to describe how to execute some code. Most of the arguments are the same for both `srun` and `sbatch`, but job arrays are supported by `sbatch` only.

### Syntax
``` bash
sbatch <job-script>
```
### An example
A simple job script where one runs the executable `lmp` on two nodes á 16 CPUs could look like this:
``` bash
#!/bin/bash
#SBATCH --partition=normal
#SBATCH --job-name=test
#SBATCH --ntasks=32
#SBATCH --nodes=2

mpirun lmp
```
and can be submitted with

``` bash
sbatch job.sh
```
where job.sh is the name of the job script.


## [salloc](https://slurm.schedmd.com/salloc.html)
The `salloc` command can be used to allocate a part of a node or an entire node. This can be useful when running interactive programs, for instance notebooks. The command sets up an interactive shell with access to the requested resources. A program executed from this shell will only have access to the requested resourses.

### Syntax
``` bash
salloc <salloc-arguments>
```

### Example 1: CPU
Below is a simple `salloc` command where 2 nodes with 32 CPUs are allocated for an hour:
``` bash
salloc --job-name=salloc --ntasks=32 --nodes=2 --time=01:00:00 --exclusive
```
The `exclusive` argument indicates that other users are not allowed to run on the requested resources, even if there is available capacity.

### Example 2: GPU
The command `salloc` can also be used to allocate one or multiple GPUs. To allocate two CPU cores and a GPU, run the following:
``` bash
salloc -n 1 -c 2 --gres=gpu:1 --job-name=salloc --time=01:00:00
```
Do not use the `exclusive` argument for GPUs, unless you allocate all GPUs on the node.

## Other useful Slurm commands
### [squeue](https://slurm.schedmd.com/squeue.html)
The command `squeue` presents the queue and priorities.

### [sinfo](https://slurm.schedmd.com/sinfo.html)
The command `sinfo` provides an overview of the different resources (available, occupied, down).

### [scancel](https://slurm.schedmd.com/scancel.html)
The command `scancel` is used to cancel jobs.

_Example_: Cancel job with job-ID 1234:
``` bash
scancel 1234
```

_Example_: Cancel all jobs associated with the user `egil`:
``` bash
scancel --user egil
```
