# Simulator 
The `Simulator` class holds the primary tool to run a simulation. A single instance of the class is required for a simulation run. If the simulation is run with support of the UE5 Api of ROS bridging, The API or ROS bridge will handle the creation and setting of the `Simulation` class instance.

**SwarmSwIM.Simulator** (timeSubdivision, sim_xml= "simulation.xml")  
Generate a simulation instance  

### Parameters:  
  - **timeSubdivision**:  *(float)*  
    * Simulation interval in seconds (mandatory)  
  - **sim_xml**: *(string)* 
    * Local or Global path to the xml file describing the simulation settings. Default is "simulation.xml"
    The setting provvides the addition of all Agents all current effects. 

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
- **rel_pos(Agent1, Agent2)** -> *numpy.array*: return the istantaneous position vector of Agent2 respect to Agent1.
    - Agent1: Agent object
    - Agent2: Agent object
      
- **acoustic_range(Agent1, Agent2)** -> *float*: return the distance vector of Agent2 respect to Agent1, considering acoustic delay, including measurament errors.
    - Agent1: Agent object (reciver)
    - Agent2: Agent object (sender)
      
- **OWTT_acoustic_range(Agent1, Agent2)** -> *float*:return the distance vector of Agent2 respect to Agent1, considering acoustic delay, , including measurament errors and One way clock drift error.
    - Agent1, Agent object (reciver)
    - Agent2: Agent object (sender)
      
- **doppler(Agent1, Agent2, msg_dt= 1.0)** -> *float*: return the value of doppler velocity of Agent2 measured by agent1 on an incoming communication, including measurament errors.
    - Agent1, Agent object (reciver)
    - Agent2: Agent object (sender)
      

## Examples
TODO


## `environment` attrubute description
TODO