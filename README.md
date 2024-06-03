# kokkos-workshop

## Useful links

- [Kokkos GitHub repository](https://github.com/kokkos/kokkos)
- [Kokkos Tutorials](https://github.com/kokkos/kokkos-tutorials)
- [Kokkos slides and videos](https://kokkos.org/kokkos-core-wiki/videolectures.html)
- [Ruche documentation](https://mesocentre.pages.centralesupelec.fr/user_doc/)

## Ruche

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

Preferred modules for GNU compilers:

```bash
module load gcc/11.2.0/gcc-4.8.5
module load cmake/3.21.4/gcc-11.2.0
```

### GPU environment (CUDA backend)

```bash
module load cmake/3.21.4/gcc-11.2.0
module load gcc/11.2.0/gcc-4.8.5
module load cuda/12.2.1/gcc-11.2.0
```
