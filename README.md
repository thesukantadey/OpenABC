# OpenABC-D: A Large-Scale Dataset For Machine Learning Guided Integrated Circuit Synthesis 
[![Version](https://img.shields.io/badge/Version-1.0.0-brightgreen)](https://github.com/NYU-MLDA/OpenABC) 
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)

## Overview

**OpenABC-D** is a large-scale labeled dataset generated by synthesizing open source hardware IPs using state-of-art logic synthesis tool **yosys-abc**. We consider 29 open-source hardware IP designs collected from various sources (MIT-CEP, IWLS, OpenROAD, OpenPiton etc) and synthesized them with 1500 random synthesis flows (we call them *synthesis recipes*).


Each synthesis flow has a predefined length **L** (L=20, in our case). We preserved all AIGs: starting, intermediate and final AIGs with labels like number of nodes, longest path, sequence of atomic synthesis transofrmations (*rewrite*, *refactor*, *balance* etc.) along with graph statistics, area and delay of final AIG. 

We converted the AIGs in **pytorch** data format that can be directly used by a machine learning engineer lessening the effort of costly labeled data generation and pre-processing. **OpenABC-D** can be used for a variety of learning tasks on logic synthesis such as

1. Predicting quality of result (QoR) performance of a *synthesis recipe* on a hardware IP. 
2. Area and delay prediction post techonolgy mapping.
3. Learn functional and structural features of AIG using self-supervised labels (useful for tasks like RL-based logic synthesis)

Our dataset can easily be used for graph-based machine learning framework like [Pytorch-Geometric](https://github.com/rusty1s/pytorch_geometric). The data generation pipeline of **OpenABC-D** is shown as follows:
![](https://github.com/NYU-MLDA/OpenABC/blob/master/figures/DatagenerationPipeline.png)


## Installing dependencies

We recommend using [venv](https://docs.python.org/3/library/venv.html) or [Anaconda](https://www.anaconda.com/) environment to install pre-requisites packages for running our framework and models.
We list down the packages which we used on our side for experimentations. We recommend installing the packages using *requirements.txt* file provided in our repository.

- cudatoolkit = 10.1
- numpy >= 1.20.1
- pandas >= 1.2.2
- pickleshare >= 0.7.5
- python >=3.9
- pytorch = 1.8.1
- scikit-learn = 0.24.1
- torch-geometric=1.7.0
- tqdm >= 4.56
- seaborn >= 0.11.1
- networkx >= 2.5
- joblib >= 1.1.0

Here are few resources to install the packages (if not using *requirements.txt*)

- [Pytorch](https://pytorch.org/get-started/locally/)
- [Torch-geometric](https://pytorch-geometric.readthedocs.io/en/latest/notes/installation.html)
- [Networkx] (https://networkx.org/documentation/stable/install.html)

Make sure that that the cudatoolkit version in the gpu matches with the pytorch-geometric's (and dependencies) CUDA version.

## Organisation

### Dataset directory sturcture

	├── OPENABC_DATASET
	│   ├── bench			# Original and synthesized bench files. Log reports post technology mapping
	│   ├── graphml                # Graphml files
	│   ├── lib			# Nangate 15nm technology library
	│   ├── ptdata			# pytorch-geometric compatible data
	│   ├── statistics		# Area, delay, number of nodes, depth of final AIGs for all designs
	│   └── synScripts		# 1500 synthesis scripts customized for each design


### Data generation

	├── datagen
	│   ├── automation 				# Scripts for automation (Bulk/parallel runs for synthesis, AIG2Graph conversions etc.)
	│   │   ├── automate_bulkSynthesis.py         # Shell script for each design to perform 1500 synthesis runs
	│   │   ├── automate_finalDataCollection.py   # Script file to collect graph statistics, area and delay of final AIG
	│   │   ├── automate_synbench2Graphml.py      # Shell script file generation to involking andAIG2Graphml.py
	│   │   └── automate_synthesisScriptGen.py    # Script to generate 1500 synthesis script customized for each design
	│   └── utilities
	│       ├── andAIG2Graphml.py			# Python utility to convert AIG BENCH file to graphml format
	│       ├── collectAreaAndDelay.py            # Python utility to parse log and collect area and delay numbers
	│       ├── collectGraphStatistics.py         # Python utility to for computing final AIG statistics
	│       ├── pickleStatsForML.py               # Pickled file containing labels of all designs (to be used to assign labels in ML pipeline)
	│       ├── PyGDataAIG.py			# Python utility to convert synthesized graphml files to pytorch data format
	│       └── synthID2SeqMapping.py		# Python utility to annotate synthesis recipe using numerical encoding and dump in pickle form
	├── synScripts
	├── library	









