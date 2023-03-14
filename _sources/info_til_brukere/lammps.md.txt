# LAMMPS
A (recent) version of [LAMMPS](https://www.lammps.org) should always be available on all nodes. The main executable is `lmp`, and is located at `/usr/local/bin`. This executable is built with a selection of LAMMPS packages, including

- Granular
- KOKKOS (on GPU nodes)
- KSpace
- Manybody
- Molecule
- Python
- Rigid

The LAMMPS executable is unique on every GPU as it has to match the architecture, and may differ from node to node. If you need other LAMMPS packages than the ones listed, we encourage you to build LAMMPS on your own. See guide below.

## Examples on running LAMMPS
Two CPU nodes à 16 CPU cores:
```bash
mpirun -n 32 lmp -in in.lammps
```
A GPU with KOKKOS:
```bash
mpirun -n 1 lmp -in in.lammps -sf kk -pk newton on neigh half -g on 1
```

## Building LAMMPS on your own
Building a customized LAMMPS setup on your own can be done in several ways, but we recommend to use CMake which will be presented here. First, clone the GitHub repository containing the source code:
```bash
git clone https://github.com/lammps/lammps.git
```
Then, create a new build directory and build using `ccmake`:
```bash
mkdir lammps/cmake/build
cd lammps/cmake/build
ccmake ..
```
`ccmake` provides a terminal user interface for CMake, where you first need to pick the desired packages (for instance the ones listed above). When the packages are ticked, configure with `c`. Second, you need to choose the architecture. For the GPU nodes, pick the correct architecture from the table below. Also, you need to tick off the option `KOKKOS_ENABLE_CUDA` if LAMMPS was built with the KOKKOS package (recommended). Then configure again using `c`, and finally generate makefile using `g`.

GPU | Architecture | Computers
--- | --- | ---
P100 | PASCAL60 | bigfacet
A100 | AMPERE80 | hugefacet, greatfacet
RTX 3090 | AMPERE86 | egil-analyze, neuroai
RTX 2080 | TURING75 | bioai, rahman

Compile with
```bash
make -j32
```
and then add the path to the executable to `$PATH`:
```bash
echo "export PATH=~/lammps/cmake/build:$PATH" >> ~/.bashrc
```
Feel free to ask for help when building LAMMPS, everything is not necessarily obvious.

## Running LAMMPS from Python
LAMMPS scripts can be launched from Python using [lammps-simulator](https://github.com/evenmn/lammps-simulator). Install from source using
```bash
pip install lammps-simulator
```

### Example 1: Simple CPU job
The following script will run the LAMMPS script `in.lammps` on 4 CPUs using the `lmp` executable:
```bash
from lammps_simulator import Simulator

sim = Simulator(directory="simulation")
sim.set_input_script("in.lammps")
sim.run(num_procs=4, lmp_exec="lmp")
```

### Example 2: GPU job
The script below will run the LAMMPS script `in.lammps` on 1 GPU using the `lmp` executable. It will by default use the KOKKOS package.
```bash
from lammps_simulator import Simulator
from lammps_simulator.computer import GPU

computer = GPU(lmp_exec="lmp", gpus_per_node=1)

sim = Simulator(directory="simulation")
sim.set_input_script("in.lammps")
sim.run(computer=computer)
```

### Example 3: GPU job submitted to Slurm
```bash
from lammps_simulator import Simulator
from lammps_simulator.computer import SlurmGPU

slurm_args = {"job-name": "GPU-job",
              "partition": "normal",
              "ntasks": 1,
              "cpus-per-task": "2",
              "gres": "gpu:1",
              "output": "slurm.out",
              }

computer = SlurmGPU(lmp_exec="lmp", slurm_args=slurm_args)

sim = Simulator(directory="simulation")
sim.set_input_script("in.lammps")
sim.run(computer=computer)
```
For more examples and functionality, see the [GitHub repository](https://github.com/evenmn/lammps-simulator).
