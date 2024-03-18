# Installation



```sh
# Installation of dependencies

# Download latest eBiota version from github
git clone https://github.com/... && cd eBiota

# Download latest database

```

Test your installation with:
```sh
./test
```



## conda
There is no conda eBiota package available yet but all dependencies can be installed from conda channels without super user rights using the following steps:

**1. Install Mini-/Anaconda**

Follow the instructions provided by conda to Install [Anaconda/Miniconda](https://conda.io/projects/conda/en/latest/user-guide/install/index.html).

**2. Create conda environment for eBiota and adding package sources**

```sh
# Cloning the development version of eBiota
git clone https://github.com/...
cd gapseq

# Create and activate a conda environment "ebiota-dev"
conda env create -n ebiota-dev --file ebiota_env.yml
conda activate ebiota-dev
```

**3. Test the installation**

```sh
./test
```



## Gurobi solver support

We recommend using *Gurobi* as LP-solver as it is usually faster than *glpk*. The *Gurobi* solver is free for academic use ([see here](https://community.ibm.com/community/user/ai-datascience/blogs/xavier-nodet1/2020/07/09/cplex-free-for-students)). Please follow the installation instructions for *Gurobi* provided by ....



## Troubleshooting
