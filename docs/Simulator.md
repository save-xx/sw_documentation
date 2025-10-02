# Simulator 
The `Simulator` class holds the primary tool to run a simulation. A single instance of the class is required for a simulation run. If the simulation is run with support of the UE5 Api of ROS bridging, The API or ROS bridge will handle the creation and setting of the `Simulation` class instance.

**SwarmSwIM.Simulator** *(timeSubdivision, sim_xml= "simulation.xml")*  
Generate a simulation instance  

### Parameters:  
  - **timeSubdivision**:  *(float)*  
    * Simulation interval in seconds (mandatory)  
  - **sim_xml**: *(string)* 
    * Local or Global path to the xml file describing the simulation settings. Default is "simulation.xml"
    The setting provvides the addition of all Agents and all current effects. 

### Attributes:  
- **time**: *(float)* 
    * Current enlapsed time in seconds
- **rnd**: *(random.generator)* 
    * Random generator object associated to this specific simulation. 
- **agents**: *(list of Agents)* 
    * List of the actively simulated agents objects
- **environment**: *(dictionary)* 
    * Collection of all current parameters. See Description avaiable option. This attribute is precompiled by the sim_xml file.
- **states**: *(dictionary name: np.array [x,y,z])* 
    - Position of each agent in the simulation, collected by agent name as key 


### Methods:
- **add ( * args)** -> *None*: add one or more agents to the simulation. 
    - args: one or more Agent object
- **remove ( * args)** -> *None*: remove, if present, one or more agents to the simulation. 
    - args: one or more Agent object in agents.  

- **tick( )** -> None: move the simulation foward of the interval indicated in Dt
- **rel_pos (Agent1, Agent2)** -> *numpy.array*: return the istantaneous position vector of Agent2 respect to Agent1.
    - Agent1: Agent object
    - Agent2: Agent object
      
- **acoustic_range (Agent1, Agent2)** -> *float*: return the distance vector of Agent2 respect to Agent1, considering acoustic delay, including measurament errors.
    - Agent1: Agent object (reciver)
    - Agent2: Agent object (sender)
      
- **OWTT_acoustic_range (Agent1, Agent2)** -> *float*: return the distance vector of Agent2 respect to Agent1, considering acoustic delay, , including measurament errors and One way clock drift error.
    - Agent1, Agent object (reciver)
    - Agent2: Agent object (sender)
      
- **doppler (Agent1, Agent2, msg_dt= 1.0)** -> *float*: return the value of doppler velocity of Agent2 measured by agent1 on an incoming communication, including measurament errors.
    - Agent1, Agent object (reciver)
    - Agent2: Agent object (sender)
      

## Examples
Following a minimalist example of the use of the `Simulator`

```python
# Import The Simulator from SwarmSwIM
from SwarmSwIM import Simulator

# Generate a simulation object, set the time step to 1/30 sec (30 Hz)
TIME_STEP = 1.0 / 30
sim = Simulator(TIME_STEP)

# Run 100 seconds of simulation
for ist in range(3000):
    # Print current time of simulation evey 300 ist (10 sec)
    if ist%300==0: print(f'{sim.time:.2f}')
    # advance the simulation foward of one TIME_STEP seconds
    sim.tick()
```


## `environment` attrubute description
The **Simulator.environment** attribute is a *dictionary* containing all the parameters that globally characterize the simulation. This attribute is fully populated during initialization and reflects the content of the simulation XML file.
The preferred usage is to rely on the XML file for all parameter definitions. However, in certain scenarios, it may be useful to modify specific parameters during the mission.
Below is a description of the individual elements:

`seed`  
Integer value used as seed for the random generator, if not defined an new random seed is used at each launch. Changes ater launch have no effect to the simulation. 

`is_uniform_current`  
Boolean, enables uniform currents

`uniform_current`  
Value of uniform current. (see XML sim for usage)

`is_noise_currents`  
Boolean, enables noise currents

`noise_currents_freq`, `noise_currents_intensity`  
Value of noise current. (see XML sim for usage)

`is_vortex_currents`  
Boolean, enables large vortex currents

`vortex_currents_density`, `vortex_currents_intensity`   
Value of vortex current. (see XML sim for usage)

`is_global_waves`  
Boolean, enables global waves (only time dependant) currents

`global_waves`  
Contains a list of *dictionaries*, each defining a global wave. Each of such dictionay is structured as follows:
- `amplitude`
- `frequency`
- `direction`
- `shift`
Details on the Vaules contained under each key are explained in XML sim.

`is_local_waves`  
Boolean, enables local waves (time and space dependant) currents

`local_waves`  
Contains a list of *dictionaries*, each defining a local wave. Each of such dictionay is structured as follows:
- `amplitude`
- `wavelength`
- `wavespeed`
- `direction`
- `shift`

  
Details on the Vaules contained under each key are explained in XML sim.


