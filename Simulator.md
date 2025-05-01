# Simulator class
The `Simulator` class holds the primary tool to run a simulation. A single instance of the class is required for a simulation run. If the simulation is run with support of the UE5 Api of ROS bridging, The API or ROS bridge will handle the creation and setting of the `Simulation` class instance.

## sim_class.Simulator
**sim_class.Simulator** ( **Dt**, **sim_xml**= "simulation.xml" )
Generate a simulation instance

### Parameters:
- Dt: *float*  
    period of time subdivision, measured in seconds.

- sim_xml: *string, optional*  
    Absolute or relative path of the simulation xml file. If an absolute path is specified, if will be used, otherwise it will be considered as a relative path. The relative path refers to the directory where the file is stored (uw_simulator). Default is "simulation.xml", which refers to the file in simulation. 
