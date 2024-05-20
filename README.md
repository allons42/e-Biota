# eBiota: a platform for *ab initio* designing microbial communities with desired functions


[![Documentation Status](https://readthedocs.org/projects/e-biota/badge/?version=latest)](https://e-biota.readthedocs.io/en/latest/index.html)
DOI

we developed eBiota, a platform for the *ab initio* design of artificial microbial communities with desired functions from a large bacterial seed pool, including maximal production or degradation efficiency of specified compounds. To achieve this, three novel algorithms are embedded in eBiota: CoreBFS, ProdFBA, and DeepCooc. CoreBFS is a graph-based, breadth-first search (BFS) algorithm for rapidly determining whether a bacterium has metabolic pathways from substrates to intermediates and/or from intermediates to products. Noting that *ab initio* designing artificial microbial communities from such a large seed pool is computationally expensive, CoreBFS allows efficient searches at acceptable computational cost through pre-stored molecules and metabolic pathways. ProdFBA is a multi-step FBA algorithm that infers metabolic flux distributions for microbial communities with potential optimum production (or degradation) efficiency while ensuring biomass. Compared with classical FBA, ProdFBA provides more artificial microbial communities with the potential to generate target products. DeepCooc is a deep learning algorithm for predicting the co-occurrence of microbial communities, which provides insight into microbial co-existence



## Installation

Conda is recommended to install eBiota in an virtual environment.

```bash
# Download latest version of eBiota
git clone https://github.com/allons42/e-Biota.git
cd e-Biota/src/

# Create and activate a conda environment "ebiota_env"
conda env create -n ebiota_env --file ebiota_env.yml
conda activate ebiota_env

# install Carveme for GEM rebuild, according to https://carveme.readthedocs.io/
pip install carveme
conda install -c bioconda diamond

# check if the installation is successful
python eBiota.py --test
```

## Quickstart

eBiota supports various functions for microbial community design. For a quickstart, we provide an example to design communities that utilize glucose and produce hydrogen.

```bash
python eBiota.py --Function design --substrate glc__D_e --product h2_e
```

There are plenty of configurations to custom your communities, detailed in `config.json`. For more usage and tutorials, see the [documentation](https://e-biota.readthedocs.io/en/latest/index.html).


## Citation

