```bash
#!/bin/bash
#SBATCH --job-name=kokkos
#SBATCH --output=output
#SBATCH --error=error
#SBATCH --time=00:05:00
#SBATCH --ntasks=40
#SBATCH --partition=cpu_short

source /gpfs/workdir/cexaci/installation/env/kokkos4.3.1_gnu11.2.sh

set -x
cd ${SLURM_SUBMIT_DIR}

cmake -B build -DCMAKE_CXX_COMPILER=g++ -DCMAKE_INSTALL_PREFIX=${KOKKOS_HOME} -DKokkos_ENABLE_OPENMP=ON -DCMAKE_CXX_EXTENSIONS=ON

make -C build -j
make -C build  install
```

```bash
#!/bin/bash
#SBATCH --job-name=kokkos
#SBATCH --output=output
#SBATCH --error=error
#SBATCH --time=00:05:00
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=10
#SBATCH -p gpu_test
#SBATCH --exclude=ruche-gpu08  # This node has an older CUDA version
#SBATCH --gres=gpu:1

source /gpfs/workdir/cexaci/installation/env/kokkos4.3.1_cuda12_v100.sh

set -x
cd ${SLURM_SUBMIT_DIR}

cmake -B build -DCMAKE_CXX_COMPILER=g++ -DCMAKE_INSTALL_PREFIX=${KOKKOS_HOME} -DKokkos_ENABLE_OPENMP=ON -DKokkos_ENABLE_CUDA=ON -DKokkos_ARCH_VOLTA70=ON -DKokkos_ENABLE_CUDA_LAMBDA=ON -DCMAKE_CXX_EXTENSIONS=ON -DKokkos_ENABLE_CUDA_CONSTEXPR=ON

make -C build -j
make -C build  install
```

```bash
#!/bin/bash
#SBATCH --job-name=kokkos
#SBATCH --output=output
#SBATCH --error=error
#SBATCH --time=00:05:00
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8
#SBATCH -p gpua100
#SBATCH --gres=gpu:1

source /gpfs/workdir/cexaci/installation/env/kokkos4.3.1_cuda12_a100.sh

set -x
cd ${SLURM_SUBMIT_DIR}

cmake -B build -DCMAKE_CXX_COMPILER=g++ -DCMAKE_INSTALL_PREFIX=${KOKKOS_HOME} -DKokkos_ENABLE_OPENMP=ON -DKokkos_ENABLE_CUDA=ON -DKokkos_ARCH_AMPERE80=ON -DKokkos_ENABLE_CUDA_LAMBDA=ON -DCMAKE_CXX_EXTENSIONS=ON -DKokkos_ENABLE_CUDA_CONSTEXPR=ON

make -C build -j
make -C build  install
```
