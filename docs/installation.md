# How To install SwarmSwIM

## Default installation

SwarmSwIM is a standard python package. For users that are not interested in adding or modifuing the code, it is suggested to install it as:

```bash
pip install git+https://github.com/save-xx/SwarmSwIM.git@dev-0.4.0
```

This will install SwarmSwIM in your standard packages folder, which should be in your pythonpath. 
You can quickly test it by opening a python IDE and typing

```python
import SwarmSwIM
```

## Developer installation (Install from Source)

To modify the codebase or develop additional plugins, you should install SwarmSwIM in editable mode.
To install from source, create a folder where to save the source files, then clone the repository.

```bash
mkdir swsw_dev && cd swsw_dev
git clone https://github.com/save-xx/SwarmSwIM.git
```

Enter the SwarmSwIM folder and switch to the development branch:
```bash
cd SwarmSwIM/
git fetch origin
git switch dev-0.4.0
```

Finally, install the package in editable mode:
```bash
pip install -e .
```

### Developer installation with VENV
It is good practice to run the package in an isolated environment using venv. 
In this setup, the virtual environment can be created in any location (not necessarily inside the project folder).

```bash
# enter your development forder where SwarmSwIM is located
cd swsw_dev/SwarmSwIM

# 3. Create a virtual environment named "swsw"
python3 -m venv ~/<your_path>/swsw
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

Install SwarmSwIM in editable mode
Make sure the virtual environment is activated, then install the package:

From the SwarmSwIM folder
```bash
pip install -e .
```

Check the correct installation:

Exit the SwarmSwim folder and run the following for another folder:

```bash
cd ~
python -c "import SwarmSwIM"
```

## Quick test installation
To test all the system installed and works correctly, run the example script
From terminal

```bash
python -m SwarmSwIM.example
```

A windows should appear showcasing a 2D represntation

---

You can now [create a new simulation](create_new.md) 