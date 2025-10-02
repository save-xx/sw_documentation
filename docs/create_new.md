# Create a new simulation

Previous: [To install](installation.md)

## Setup

In order to create a new simulation is recommended to create a dedicated folder (separate from the installation folder, is installed from Source).

For this example we will call it mysim

```bash 
mkdir mysim && cd mysim
```

Within the folder run the following shortcut:
```bash
SwarmSwIM create_new
```

Should output:
```
$ Initiated SwarmSwIM env templates in .
```

In windows or some python Env it ma be possible that the shortcut does not work, you can still refer to the script by running:

```bash
python3 -m SwarmSwIM.utility.cli create_new
```

Test by running the automatically created example:
```bash
python3 example.py
```

---

## Create a new script
We will ignore `example.py` and create a brand new script to run our simulation
Create a new file `my_first_sim.py`

```python
# Import the Simulator
from SwarmSwIM import Simulator

PERIOD = 0.05 

# Start a Simulator instance with a 0.05 s period
S = Simulator(PERIOD)

# define function describing all actions in a simulation step
def cycle():
    # run a simulation step
    events = S.tick()
    # print step events
    print(events)

# Execute 100 simulation steps (5 seconds)
for i in range(100):
    cycle()
```

This is the most basic simulation. 

```python 
S = Simulator(PERIOD)
```
Starts a new simulation object with a certain timestep (mandatory field). By default the `Simulator` will search for `simualtion.xml` in the script folder. the user can specify a specific `sim_xml` as a path (either relative or absolute) to specify a different xml file to be used. Details of the `Simulator` class are in [Simulator](Simulator.md).

We created a function called `cycle` that contains all the actions happening in a single simulation step.

```python 
events = S.tick()
print(events)
```

Advances the simulation forward of one period. events is a dictionary collecting all plugins events. Currenty no plugin has been added, as such the event dictionary is empty.

Finally we run the simulation for 100 steps:

```python 
for i in range(100):
    cycle()
```

The result is that we are printing an empty dictionary at each step, as the is no plugin event.

---

Let's print somenthing more useful: in this simulation the agents are named `A01` and `A02`, the naming can be controlled in the [simulator xml](XML_files.md), later explained.

To output the position of `A01` lets change the print to:
```python
print(S['A01'].pos)
```

We will now see a series of 3-elements arrays:
```
[0. 0. 1.]
```

Those are the x, y, z position of `A01` at each step.

---

In the next page, how to [Set a 2d Visualized](setVisualizer.md)