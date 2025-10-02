# How To install SwarmSwIM

## Default installation

SwarmSwIM is a standard python package. For users that are not interested in adding or modifuing the code, it is suggested to install it as:

```bash
git clone https://github.com/save-xx/SwarmSwIM.git
```

This will install SwarmSwIM in your standard packages folder, which should be in your pythonpath. 
You can quickly test it by opening a python IDE and typing

```python
import SwarmSwIM
```

## Developer installation (Install from Source)

In order to have direct access to the codebase, set modifications, add or develop additional plugins it is necessary to setup a developer installation. 
To install from source, create a folder where to save the source files, then clone the repository.

```bash
mkdir swsw_dev && cd swsw_dev
git clone https://github.com/save-xx/SwarmSwIM.git
```

Enter the SwarmSwIM folder and use pip to install it in editable mode
```bash
cd SwarmSwIM/
pip install -e .
```

To run an isolated python vistual envrioment (using venv):

```bash
# 1. Create your project folder and enter it
mkdir swsw_dev && cd swsw_dev

# 2. Clone the repo
git clone https://github.com/save-xx/SwarmSwIM.git

# 3. Create a virtual environment named "swsw"
python3 -m venv swsw
```

Activate the virtual envrioment
+ On Linux / macOS:
```bash
source swsw/bin/activate
```
+ On Windows (PowerShell):
```bash
swsw\Scripts\Activate.ps1
```

Enter the cloned folder and Install SwarmSwIM in editable mode inside the swsw venv
```bash
cd SwarmSwIM/
pip install -e .
```

## Quick test installation
To test all the system installed and works correctly, run the example script
From terminal

```bash
python3 -m SwarmSwIM.example
```

A windows should appear showcasing a 2D represntation

---

You can now [create a new simulation](create_new.md) 