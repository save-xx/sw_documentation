# Create a new Plugin 
SwarmSwim is set to simplify and streamline the process of adding a new plugin to the simulator.
In order to do so one should follow the template below:

```python
"""SwarmSwIM Plugin template."""
# A baseline template to add new plugins to the SwarmSwIM simulator

import xml.etree.ElementTree as ET
import logging

logger = logging.getLogger(__name__)

def activate_Plugin(simulation, sensor_name="default_name", *args, **kwargs):
    """
    User function to add the plugin to the simulator.
    Takes an instance of the simulator and any number of arguments (as preferred)
    """
    # Create an instance of the Plugin class
    plugin_instance = Plugin(simulation, sensor_name, *args, **kwargs)

    # Register the sensor in the simulation post-step call dictionary
    simulation.plugins_calls_poststep[sensor_name] = plugin_instance

    # or the pre-step call dictionary
    # simulation.plugins_calls_prestep[sensor_name] = plugin_instance

    # Optionally, return an handle to the user
    return plugin_instance


class Plugin:
    def __init__(self, simulation, plugin_name, *args, **kwargs):
        """Initialize the Plugin."""

        # Some common operation
        self.sim = simulation
        self.plugin_name = plugin_name

        # create random seed
        self.rnd = simulation.rnd.spawn(1)[0]

        # path to the active simulation.xml file for parsing
        sim_xml_path = simulation._simulation_filepath

        # Extract parameters from XML
        self._unpack_from_xml(plugin_name,  sim_xml_path)


    def _unpack_from_xml(self, plugin_name,  sim_xml_path):
        """Store and extract data from an xml file, under sensor name."""
        tree = ET.parse(sim_xml_path)
        root = tree.getroot()
        root = root.find(plugin_name)
        if root is None: 
            raise ValueError(f"The XML does not contain a <{plugin_name}> element.")
        # Extract data
        self.some_parameter = root.find("some_parameter").text

    def __call__(self):
        """Resolve plugin event at each simulation step"""
        
        # =====================
        # Plugin Logic
        # Add here all the plugin logic and action
        # remeber that you have access to the simulation state
        # by referring self.sim, and each agent by self.sim.agents
        # =====================

        # store the latest output 
        self.result = "Done"
        # Return the output of the event for the plugin
        return self.result
    
    def _bag(self)->dict:
        """Collect SQLite friendly output."""

        # =====================
        # Return a dictionary or list of dictionaries that can be saved in a sql format.
        # A dedicated page will be created for the plugin using the plugin name.
        # Each list element adds a line in the log, multiple elements will add multiple
        #  lines at each call.
        # if [] is passed no line is added.
        # =====================
        
        sql_friendly_out = [{"output": self.result}]
        return sql_friendly_out
```

## Explanation
---
### Imports
```python
import xml.etree.ElementTree as ET
import logging

logger = logging.getLogger(__name__)
```
Those are the minimum recommended inputs to be added. `xml` allows to parse the simulations and agents xml files, allowing to add and use new tags dedicated to the new plugin.  
The logging is the suggested way to add terminal output instead of `print()`.

---

### Activate the plugin
It is always recomended to create an activation function that starts the plugin and adds it to the simulation

```python
def activate_Plugin(simulation, sensor_name="default_name", *args, **kwargs):
```

As convetion, the function should be named activate_<plugin name>. Two required inputs should be set:
`simulation`, as a reference the the simulation object, and `sensor_name` which should provide an unique key to identify the specific plugin. Beyond those an arbitrary number of parameters can be added as additional inputs as required.

```python
    # Create an instance of the Plugin class
    plugin_instance = Plugin(simulation, sensor_name, *args, **kwargs)
```
In this line the `__init__` method of the Plugin class is called to create a new instance
Succesively the instance must be passed to either `plugins_calls_poststep`, 

```python
    # Register the sensor in the simulation post-step call dictionary
    simulation.plugins_calls_poststep[sensor_name] = plugin_instance
```

or `plugins_calls_prestep`
```python
    simulation.plugins_calls_prestep[sensor_name] = plugin_instance
```

The Prestep plugins are called before the update of the position of each agent due to control, while the poststep are called right after


In some cases there are some methods of the plugin that should be accessible to the user: for example in the Acoustic plugin, the `AcousticChannel` plugin has a method `send` that is supposed to be used by the user/main script, to initiate acoustic communication.

In those cases it is needed to expose a reference to the instance of the created plugin by returning it:

```python
    # Optionally, return an handle to the user
    return plugin_instance
```
---

### `__init__`
Some common practices during a plugin initialization
```python
        # Some common operation
        self.sim = simulation
        self.plugin_name = plugin_name
```
Save the simulation instance and key name of the instance

```python
        # create random seed
        self.rnd = simulation.rnd.spawn(1)[0]
```

Avoid using `random` directely and refer to `self.rnd` as a spawn of a general random generator. This will preserve the full simulation repetability, when setting a global `seed`, while allowing to introduce pseudo-randomness to the plugin functions, if needed.


```python
        # path to the active simulation.xml file for parsing
        sim_xml_path = simulation._simulation_filepath

        # Extract parameters from XML
        self._unpack_from_xml(plugin_name,  sim_xml_path)
```
In order to extract data from the xml, one can directly refer to the full path saved internally in simulation. Below explained `_unpack_from_xml` to extract data from the simulation xml.

### Unpack from XML
Instead of passing every parameter at initialization, it is convinient to 