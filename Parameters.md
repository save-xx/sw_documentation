## Attributes and Methods


[Simulator](Object_description/Simulator.md): Generate the core simulation


### Agent
**Parameters**
- **name**: (string) Unique name of the agent added. Mandatory field
- **Dt** (float) Simulation interval in seconds. Overwritten by simulator. Default 0.0.
- **pos** (array-like[x,y,z]) initial position of the agent in the world, NED coordinated. Default [0,0,0].
- **psi0** (float) initial heading of the agent. NED coordinates. Default 0.0 (North).
- **agent_xml** (string) Local path to the xml file describing the agent settings. Default is "default.xml"

**Attributes**

- **measured_depth**: (float) Measured estimated depth used in depth control
- **measured_heading**: (float) Measured estimated heading used in heading control
- **measured_pos**: (list[x,y]) Measured estimated position used in position control 

- **cmd_depth**: (float) get/set desired depth in meters. Only when depth_control is ideal, step, proportional.
- **cmd_heave**: (float) get/set desired heave velocity. Only when depth_control is heave.
- **cmd_heading**: (float) (float) get/set desired heading in degrees. Only when heading_control is ideal, step, proportional.
- **cmd_yawrate**: (float) get/set desired rotational yaw rate velocity. Only when depth_control is yawrate.
- **cmd_planar**: (numpy.array[x,y]) get/set planar desired coordinates. Only when planar_control is ideal or step.
- **cmd_local_vel**: (array like or float) get/set desired surge and sway velocity in body frame. Only when planar_control is local_velocity. If set float, assumes sway=0.
- **cmd_forces**: (float) (array like or float) get/set desired surge and sway forces in body frame. Only when planar_control is local_forces. If set float, assumes sway=0.

- **internal_clock**: (float) internally measured time, affected by clock drift.

- **NNDetector**: (dictionary) results obtained since last detection event. Each key is the name of a detected agent, and the item associated is a list of [distance, relative horizontal angle, relative vertical angle]. A special key named time_lapsed contains the value of the time interval since last detection.

**Methods**
- **cmd_fhd(force,heading,depth)**->None: Set desired command as force (float or array-like[x,y]), heading (float), depth (float). 

- **cmd_xyz_phi(position,heading)**->None: Set desired waypoint (np.array[3]) and heading (float)