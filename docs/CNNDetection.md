# CNNDetection
The `CNNDetection` class is an optional sensor plugin designed to emulate the output of a Convolutional Neural Network (CNN) detector. It simulates the detection and localization of neighboring agents by estimating their positions and headings. This plugin operates as a global sensor, meaning it has access to the full environment state to generate its output.

**SwarmSwIM.sensors.CNNDetection** *(rnd = None, visibility_model = None)*

### Parameters
- rnd: *(numpy.random.generator or None)* Default None
    * Initializes the random number generator. If set to `None`, a new generator will be created. Otherwise, the provided generator will be used. The intended usage is to inherit the generator from the `Simulator`, ensuring consistent and reproducible behavior across components.

**(deprectated remove)**
- visibility_model: *(string)* Default None 
    * Define model used to determine the distance-based probability of detection. `None`, and `linear`. In  `None` all detection are succesfull if contained in the defined Field of View. In `linear` the probability function is defined as a splitwise linear fuction of the distance. 

### Methods
- `__call__`**(Simulation)**  ->  *list [string]* 
    * Calculates the detections of all agents involved in the simulations, outputs the lists of the agents names that recived an updated in the call. The input is the `Simulation` instance. Note that the method already optimizes on the udate rate, so that the relative pose clculations are only executed as needed. 
    This method alters and stores the relative informations in each `Agent` instance withing the `NNDetector` atturibute (see Example).


### `Agent.NNDetector` 
The `Agent.NNDetector` is an optional `Agent` attrubute which is manipulated by the `CNNDetection` in order to store the detection informations. Specifically it is a *dictionary* with the following proprieties: 
- a special key `time_lapsed` stores the lapsed time since the last detection activation. 
- a key with the name of each agent sucessfully detected in the last detection structures as a list of three flloats:
    - Radial relative distance [m]
    - Planar angle Alpha [degree]. Angle on the horizontal plane, with **x** axis (0 degree) the heading of the observing Agent.  
    - Elevation angle Beta [degree]. Angle measured from the horizontal plane

The values presented already implement the measuraments noises as defined in the Agent XML description.

## Examples

```python
# Import The Simulator from SwarmSwIM
from SwarmSwIM import Simulator, Agent
from SwarmSwIM import CNNDetection

# Generate a simulation object, set the time step to 1/30 sec (30 Hz)
TIME_STEP = 1.0 / 30
sim = Simulator(TIME_STEP)
# Initiate detection
Detection = CNNDetection()

first_agent_name = sim.agents[0].name

# Run 100 seconds of simulation
for ist in range(3000):
    # advance the simulation foward of one TIME_STEP seconds
    sim.tick()
    detection_upd_names = Detection(sim)
    # print resulting detection for the first agent, if it has recived an update
    if first_agent_name in detection_upd_names:
        new_detection = sim.agents[0].NNDetector
        for key in new_detection:
            if key=='time_lapsed': continue
            print(f' time: {sim.time:.2f}', end="")
            print(f'    {key} : ',end="") 
            print(f'[ dist: {new_detection[key][0]:.2f} ',end="") 
            print(f'alpha: {new_detection[key][1]:.2f} ',end="") 
            print(f'beta: {new_detection[key][2]:.2f} ]')
```