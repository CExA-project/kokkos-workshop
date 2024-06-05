# kokkos-workshop

## Useful links

- [Kokkos Core GitHub repository](https://github.com/kokkos/kokkos)
- [Kokkos Tutorials](https://github.com/kokkos/kokkos-tutorials)
- [Kokkos slides and videos](https://kokkos.org/kokkos-core-wiki/videolectures.html)
- [Ruche documentation](https://mesocentre.pages.centralesupelec.fr/user_doc/)
- [Cheat sheet](https://github.com/CExA-project/cheat-sheet-for-kokkos/tree/main?tab=readme-ov-file)

## Ruche cluster

Ruche is the mesocentre of Paris-Saclay. It provides a computing environment for research and teaching.
Ruche is equipped of both CPU (Intel Xeon Gold 6230 20C @ 2.1GHz) and GPU nodes (Nvidia Tesla V100 and A100).

### Connect to Ruche

```bash
ssh <login>@ruche.mesocentre.universite-paris-saclay.fr
```

SSH configuration:
```bash
Host ruche
   User <login>
   Hostname ruche.mesocentre.universite-paris-saclay.fr
```

Editors like `vim` and `nano` are available on Ruche. VSCode can be connected to Ruche using the Remote SSH extension.

### CPU environment (OpenMP backend)

#### GNU compiler

Preferred modules for GNU compilers:

```bash
module load gcc/11.2.0/gcc-4.8.5
module load cmake/3.21.4/gcc-11.2.0
```

We provide an already installed verison of Kokkos 4.3.1 in case of:

```bash
source /gpfs/workdir/cexaci/installation/env/kokkos4.3.1_gnu11.2.sh
# -> provides $KOKKOS_HOME
```


### GPU environment (CUDA backend)

Preferred modules for NVIDIA compilers:

```bash
module load cmake/3.21.4/gcc-11.2.0
module load gcc/11.2.0/gcc-4.8.5
module load cuda/12.2.1/gcc-11.2.0
```

- Installed Kokkos version for V100:

```bash
source /gpfs/workdir/cexaci/installation/env/kokkos4.3.1_cuda12_v100.sh
# -> provides $KOKKOS_HOME
```

using the following flags:
```bash
cmake -B build -DCMAKE_CXX_COMPILER=g++ -DCMAKE_INSTALL_PREFIX=${KOKKOS_HOME} -DKokkos_ENABLE_OPENMP=ON -DKokkos_ENABLE_CUDA=ON -DKokkos_ARCH_VOLTA70=ON -DKokkos_ENABLE_CUDA_LAMBDA=ON -DCMAKE_CXX_EXTENSIONS=ON -DKokkos_ENABLE_CUDA_CONSTEXPR=ON
```

- Installed Kokkos version for A100:

```bash
source /gpfs/workdir/cexaci/installation/env/kokkos4.3.1_cuda12_a100.sh
# -> provides $KOKKOS_HOME
```

### Tutorial

Slurm example to compile and execute a tutorial exercise with OpenMP backend on a CPU node:

```bash
#!/bin/bash
#SBATCH --job-name=kokkos
#SBATCH --output=output
#SBATCH --error=error
#SBATCH --time=00:10:00
#SBATCH --ntasks=4
#SBATCH --partition=cpu_short

# Load here your modules and prepare your env

module load ...

# to use an already installed version of Kokkos
export Kokkos_ROOT=...

set -x
cd ${SLURM_SUBMIT_DIR}

# OpenMP parameters
export OMP_PROC_BIND=spread
export OMP_PLACES=threads
export OMP_NUM_THREADS=4

# Execution
./build_cuda/Begin/$(basename $(pwd))

# Execution solution
./build_cuda/Solution/$(basename $(pwd))_Solution

```

Slurm example to compile and execute a tutorial exercise with OpenMP and CUDA backend on a GPU node:

```bash
#!/bin/bash
#SBATCH --job-name=kokkos
#SBATCH --output=output
#SBATCH --error=error
#SBATCH --time=00:10:00
#SBATCH --cpus-per-task=10
#SBATCH -p gpu_test
#SBATCH --gres=gpu:1

# Load here your modules and prepare your env

module load ...

# to use an already installed version of Kokkos
export Kokkos_ROOT=...


set -x
cd ${SLURM_SUBMIT_DIR}

export Kokkos_ROOT=$KOKKOS_HOME

cmake -B build_cuda -DCMAKE_CXX_COMPILER=g++ -DKokkos_ENABLE_CUDA=ON

make -C build_cuda -j

# OpenMP parameters
export OMP_PROC_BIND=spread
export OMP_PLACES=threads
export OMP_NUM_THREADS=1

# Execution
./build_cuda/Begin/$(basename $(pwd))

# Execution solution
./build_cuda/Solution/$(basename $(pwd))_Solution
```
