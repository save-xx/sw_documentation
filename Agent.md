# Agent
The Agent class contains all the information relative to a single agent in the simulation. The various instances of the agent classes 

**SwarmSwIM.Agent** *(name, Dt= 0.1, initialPosition= np.array([0.0,0.0,0.0]), initialHeading = 0.0, agent_xml= "default.xml", rng= None)*

### Parameters
- **name**: *(string)* 
    * Unique name of the agent added. Mandatory field
- **Dt** *(float)* 
    * Simulation interval in seconds. Overwritten by simulator. Default 0.0.
- **initialPosition** *(array-like[x,y,z])* 
    * initial position of the agent in the world, NED coordinated. Default [0,0,0].
- **initialHeading** *(float)* 
    * initial heading of the agent. NED coordinates. Default 0.0 (North).
- **agent_xml** *(string)* 
    * Local path to the xml file describing the agent settings. Default is "default.xml"
- **rng** *(int or None)*
    * Seed for the generation of the random generation, normal use is to hereditate the seed from the simulation. If None (default) a the seed is randomized.

### Attributes

- **measured_depth**: *(float)* 
    * Measured estimated depth used in depth control
- **measured_heading**: *(float)* 
    * Measured estimated heading used in heading control
- **measured_pos**: *(list[x,y])* 
    * Measured estimated position used in position control 
- **cmd_depth**: *(float)* 
    * get/set desired depth in meters. Only when depth_control is ideal, step, proportional.
- **cmd_heave**: *(float)* 
    * get/set desired heave velocity. Only when depth_control is heave.
- **cmd_heading**: *(float)* 
    * get/set desired heading in degrees. Only when heading_control is ideal, step, proportional.
- **cmd_yawrate**: *(float)* 
    * get/set desired rotational yaw rate velocity. Only when depth_control is yawrate.
- **cmd_planar**: *(numpy.array[x,y])* 
    * get/set planar desired coordinates. Only when planar_control is ideal or step.
- **cmd_local_vel**: *(array like or float)* 
    * get/set desired surge and sway velocity [x,y] in body frame. Only when planar_control is local_velocity. If set float, assumes sway=0.
- **cmd_forces**: *(array like or float)* 
    * get/set desired surge and sway forces in body frame. Only when planar_control is local_forces. If set float, assumes sway=0.
- **internal_clock**: *(float)* 
    * internally measured time, affected by clock drift.

TODO 'e_depth','e_heave','e_heading','e_yawrate','e_position','e_local_vel','e_inertial_vel', 'e_local_force'

  
### Additional Attributes 
Based on addon sensors, the class can collected additional attributes used by the relative plugins

- **NNDetector**: *(dictionary)* 
    * results obtained since last detection event. Each key is the name of a detected agent, and the item associated is a list of [distance, relative horizontal angle, relative vertical angle]. A special key named time_lapsed contains the value of the time interval since last detection. See `NNDetector`.

### Methods

- **cmd_fhd(force,heading,depth)**->None: 
    * Set desired command as force (float or array-like[x,y]), heading (float), depth (float). 

- **cmd_xyz_phi(position,heading)**->None: 
    * Set desired waypoint (np.array[3]) and heading (float)

## Examples

```python
# Import The Simulator from SwarmSwIM
from SwarmSwIM import Simulator, Agent

# Generate a simulation object, set the time step to 1/30 sec (30 Hz)
TIME_STEP = 1.0 / 30
sim = Simulator(TIME_STEP)

# Create an additional agent, type default, at initial position x0 and heading psi0
x0 = [2.0, 3.0, 12.0]
psi0 = 30
new_agent = Agent('nemo', TIME_STEP, initialPosition= x0 , initialHeading= psi0)

# set an initial command to the new agent, as surge force (1N), heading (90 degree), and depth (10m)
new_agent.cmd_fhd(1.0, 90.0, 10.0)

# Add new agent to simulation
sim.add(new_agent)

# Run 100 seconds of simulation
for ist in range(3000):
    # Print new agent name and position of simulation evey 300 ist (10 sec)
    if ist%300==0: print(f'{sim.agents[-1].name}: pos {sim.agents[-1].measured_pos}')
    # advance the simulation foward of one TIME_STEP seconds
    sim.tick()
```