# Agent Class Documentation

The Agent class models an underwater autonomous agent with configurable dynamics, control schemes, and noise effects.  
It supports heading, depth, and planar motion control, with options for direct force, velocity, and waypoint commands. 
Initialization of the various instances of `Agent` are autonmatically created and initializated from the `Simulator` class. It is however possible to create independent instances and add the to the simulation with the ```Simulator.add``` method.

The core use of the class, from the user perspective, are the Command Interfaces described below. see [Command Interfaces](#command-interfaces)

---

## Overview

- Each agent is uniquely identified by its name.
- The class loads physical and control parameters from an XML configuration file.
- It simulates the sensor feedback, directely invloved in the payload, with optional noise.
- The internally processed sensors feedback are, with respect of the control scheme adopted: depth, heave speed, heading, yawrate, position, velocity, thruster force output. 
- Multiple control schemes are available for heading, depth, and planar motion.
- Commands can be issued in terms of forces, velocities, positions, heading, depth, or yawrate.

---

## Initialization

```python
Agent(
    name: str,
    initialPosition: array-like = [0.0, 0.0, 0.0],
    initialHeading: float = 0.0,
    agent_xml: str = "default.xml",
    rng: int | None = None
)
```

Parameters:

* name (str): Unique identifier for the agent.
* initialPosition (list | tuple | np.ndarray, size 3): Initial \[x, y, z] position. Defaults to \[0, 0, 0].
* initialHeading (float): Initial heading in degrees (0 = North, positive clockwise). Defaults to 0.0.
* agent\_xml (str): XML file defining agent parameters. Defaults to "default.xml".
* rng (int | None): Random seed for measurement noise. If not given, an unseeded RNG is used.

Example:

```python
from SwarmSwIM import Agent
A1 = Agent(name="A1", initialPosition=[0, 0, -5], initialHeading=90.0, agent_xml = "myagent.xml")
```

---

## Key Attributes

* pos (np.ndarray): Current 3D position \[x, y, z].
* psi (float): Current heading (degrees).
* yawrate (float): Current yawrate (deg/s).
* measured\_depth, measured\_heading, measured\_pos: Noisy sensor measurements.
* cmd\_depth, cmd\_heading, cmd\_planar, cmd\_local\_vel, cmd\_forces: Current control commands.
* planar\_control, heading\_control, depth\_control: Control modes loaded from XML or set via commands.
* massMatrix, linear\_damping, quadratic\_damping: Dynamic parameters.

---

## Simulation Cycle

### tick()

Advances the agent state by one simulation step:

1. Updates sensor feedback with noise.
2. Updates heading.
3. Updates depth.
4. Updates planar motion.

Example:

---

## Control Modes

Depth Control:

* ideal: Instant correction.
* step: Incremental correction with fixed step size (constant speed, no overshoot). 
* proportional: Proportional controller with threshold limits.
* heave: Direct controlled vertical velocity.

Heading Control:

* ideal: Instant correction.
* step: Incremental correction with step size. (constant yaw rate, no overshoot)
* proportional: Pure Proportional controller with limit.
* yawrate: Direct yawrate control.

Planar Control:

* ideal: Instant correction.
* step: Incremental waypoint tracking. Assumes constant planar translation. Unaffected by agent heading.
* local\_velocity: Body-frame velocity commands, velocity compared to the surronding waterflow (The actual velocity is then affected by the additional translation of the currents, use this mode if the velocity contrlo is based on flow sensors or purerly model based).
* inertial\_velocity: Body-frame velocity commands with current compensation. Makes best effort to mantain the velocity indicated, with respect to an inertial reference frame. Whithin the limits indicated fully compensaates for currents. Use when emulating feedback based on DLV or INS.
* local\_forces: Force-based dynamics with damping and added mass. USe input as Body-frame force vector

---

## Command Interfaces

### set\_ForceHeadingDepth(forceNewton=None, headingDegrees=None, depthMeters=None)

Control planar force, heading, and depth simultaneously.

Example:

```python
A1.set_ForceHeadingDepth(forceNewton=[5., 0.], headingDegrees=45, depthMeters=10)
```

---

### set\_PositionHeading(positionMeters, headingDegrees=None)

Move to a target position with optional heading.
If positionMeters has 3 values, depth is updated as well.

Example:

```python
A1.set_PositionHeading([10., 5.], headingDegrees=90)
```

---

### set\_ForceCmd(forceNewton, enforce=False)

Direct force command in body frame.
If enforce=True, sets planar\_control = "local\_forces".

Example:

```python
A1.set_ForceCmd([2., 0.], enforce=True)
```

---

### set\_VelocityCmd(velocity, mode=None)

Velocity command in body frame.
mode can be "local\_velocity" or "inertial\_velocity".

Example:

```python
A1.set_VelocityCmd([1., 0.], mode="local_velocity")
```

---

### set\_WaypointCmd(waypoint, mode=None)

Waypoint command for planar position.
mode can be "ideal" or "step".

Example:

```python
A1.set_WaypointCmd([5., 5.], mode="step")
```

---

### set\_Heading(headingDegrees, mode=None)

Heading command. Set desired heading in Degrees (NED)
mode can be "ideal", "step", or "proportional".

Example:

```python
A1.set_Heading(180., mode="proportional")
```

---

### set\_Yawrate(yawrate, enforce=False)

Yawrate command. Set desired Yawrate in Degrees/second (NED)
If enforce=True, sets heading\_control = "yawrate".

Example:

```python
A1.set_Yawrate(10., enforce=True)
```

---

### set\_Depth(depthMeters, mode=None)

Depth command. Depth in meters, positive is down (NED)
mode can be "ideal", "step", or "proportional".

Example:

```python
A1.set_Depth(20., mode="step")
```

---

### set\_Heave(heave, enforce=False)

Heave (vertical velocity) command. Heave in meters/seconds, positive is down (NED)
If enforce=True, sets depth\_control = "heave".

Example:

```python
A1.set_Heave(0.5, enforce=True)
```

---

## Noise Simulation

* Each measurement can be affected by Gaussian noise.
* Noise parameters (e\_depth, e\_heading, e\_position, etc.) are loaded from the XML.
* Errors are applied via emulate\_error(value, error).

---

## Notes

* The XML configuration file defines dynamics, damping, control parameters, and noise models.
* Depth is positive downwards (NED convention).
* Heading follows NED convention (0° = North, increasing clockwise).

