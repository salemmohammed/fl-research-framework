# SAFER-FedEdge: Federated Learning for IIoT

Implementation of "Federated and Edge AI for 6G-Enabled IIoT" using Flower (flwr),
built from source.

## Requirements

- macOS (Apple Silicon or Intel)
- Miniconda or Anaconda
- Git
- Python 3.11 (NOT 3.13 — see Troubleshooting)


## Setup

### 1. Clone the repository
git clone https://github.com/salemmohammed/fl-research-framework.git
cd fl-research-framework

### 2. Create a Python 3.11 environment
conda create -n flower-env python=3.11  
conda activate flower-env

### 3. Install Poetry inside the environment
pip install poetry==1.7.1  

### 4. Tell Poetry to use this environment's Python
python -m poetry env use $(which python)  

### 5. Install Flower and all dependencies from source
python -m poetry install --all-extras  

### 6. Fix numpy conflict
python -m poetry run pip install "numpy<2.0.0"  

### 7. Install example dependencies
python -m poetry run pip install "flwr-datasets[vision]" torch torchvision tqdm

### 8. Verify the installation
python -m poetry run python -c "import flwr; print(flwr.__file__)"  
python -m poetry run python -c "import flwr; print(flwr.__version__)"  
python -m poetry run python -c "import numpy; print(numpy.__version__)"

Expected output:
- Path ending in: fl-research-framework/src/py/flwr/__init__.py
- Version: 1.10.0
- Numpy: 1.26.4


## Run the quickstart example (deployment mode)

Open 3 terminals. In each terminal activate the environment first:
    conda activate flower-env
    cd fl-research-framework

Terminal 1 - start the server:
    python -m poetry run python examples/quickstart-pytorch/server.py

Terminal 2 - start client 0:
    python -m poetry run python examples/quickstart-pytorch/client.py --partition-id 0

Terminal 3 - start client 1:
    python -m poetry run python examples/quickstart-pytorch/client.py --partition-id 1

Expected output in Terminal 1 after 3 rounds:
    Run finished 3 round(s)
    accuracy: round 1: 0.1253, round 2: 0.2174, round 3: 0.2896

## Run simulation mode (recommended for paper)

One command, no separate terminals, simulates N clients on one machine.  
This is the mode used for SAFER-FedEdge experiments:  
    python -m poetry run python examples/simulation-pytorch/sim.py

## Project structure
```
fl-research-framework/
├── SAFER-FedEdge/          ← our implementation (paper)
│   └── README.md
├── src/py/flwr/            ← Flower source code (do not modify)
├── examples/               ← Flower examples (reference only)
└── pyproject.toml          ← root build file
```


## Troubleshooting

### Python 3.13 not supported
Flower 1.10.0 requires Python 3.8-3.12.  
ruamel-yaml-clib fails to compile on Python 3.13.  
Fix: use Python 3.11 via conda (see Setup step 2).

### numpy conflict
flwr-datasets installs numpy>=2.0 but Flower 1.10.0 requires numpy<2.0.  
Fix: python -m poetry run pip install "numpy<2.0.0"

### zsh bracket error
zsh interprets square brackets as glob patterns.  
Wrong:  pip install flwr-datasets[vision]  
Right:  pip install "flwr-datasets[vision]"

### flwr_datasets not found
This package is separate from flwr and must be installed manually.  
Fix: python -m poetry run pip install "flwr-datasets[vision]"

### poetry command not found after creating conda env
Poetry is not automatically available in new conda environments.  
Fix: pip install poetry==1.7.1 inside the new environment first.

