





### Simulation Example 
```python
from SwarmSwIM import Simulator, CNNDetection
TIME_STEP = 1 / 24  # Simulation time step in seconds

# Initialize the simulator with the chosen time step
S = Simulator(TIME_STEP)

# Initialize a sensor model (e.g., CNN-based detection)
Detection = CNNDetection()

# Run the simulation until a specific condition is met
for i in range(1000):
    S.tick()         # Progress the simulation by one time step
    Detection(S)     # Apply detection on the current state of the simulator, return names of agents activated

```

This code runs the simulator in discrete steps until the specified condition is met. Note that the simulator does not run in real time; it iterates through each time step as quickly as possible. This setup is particularly useful for post-analysis and machine learning applications, where real-time processing is not required, and fast iteration are preferrable.

### Minimal animation
The animator2D provides a simple representation of the agents on the planar position and heading of each agent. Within comutational capabilites of the hosting machine and simulation complexity, the animator will run the simulation in real-time. A code example of the minimal animation can be found in the file `example2Danimation.py`.

With the Package installed, from the SwarmSwIM folder, launch the example:
```bash
python3 -m SwarmSwIM.example2Danimation
```