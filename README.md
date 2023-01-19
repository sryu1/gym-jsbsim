# JSBGym

[![Python: 3.7+](https://img.shields.io/badge/python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

JSBGym provides reinforcement learning environments for the control of fixed-wing aircraft using the JSBSim flight dynamics model. JSBGym requires at least Python 3.7. The package's environments implement the OpenAI Gym interface allowing environments to be created and interacted with in the usual way.

## Setup

Firstly, install [JSBSim](https://github.com/JSBSim-Team/jsbsim). Make sure that it is installed in `C:/JSBSim`

If you would like to render the environment with FlightGear, install it from [here](https://sourceforge.net/projects/flightgear/). Make sure the FlightGear bin directory is in PATH (Usually `C:\Program Files\FlighGear 2020.3\bin`)and there is a system variable called `FG_ROOT` with the FG data folder as it's value (Usually `C:\Program Files\FlightGear 2020.3\data`).
3D visualisation requires installation of the FlightGear simulator. Confirm it is runnable from terminal with:

```console
fgfs --version
```

Open the console and install the required dependencies for python:

```console
pip install gym git+https://github.com/sryu1/gym-jsbsim jsbsim
```

## Getting Started

```python
import jsbsim
import gym_jsbsim
import gym

env = gym.make(ENV_ID)
env.reset()
observation, reward, done, info = env.step(action)
```

## Environments

JSBGym implements two tasks for controlling the altitude and heading of aircraft:

* **HeadingControlTask**: aircraft must fly in a straight line, maintaining its initial altitude and direction of travel (heading)
* **TurnHeadingControlTask**: aircraft must turn to face a random target heading while maintaining their initial altitude

The environment can be configured to use one of three aircraft:

* **Cessna172P** light aircraft
* **F15** fighter jet
* **A320** airliner

Environment ID strings are constructed as follows:

```python
f'JSBSim-{task}-{aircraft}-SHAPING_STANDARD-NoFG-v0'
```

For example, to fly a Cessna on the TurnHeadingControl task,

```python
env = gym.make('JSBSim-TurnHeadingControlTask-Cessna172P-Shaping.STANDARD-NoFG-v0')
```

## Visualisation

### 2D

A basic plot of agent actions and current state information can be using `human` render mode by calling `env.render(mode="human")`.

```python
env = gym.make("JSBSim-TurnHeadingControlTask-Cessna172P-Shaping.STANDARD-NoFG-v0")
env.reset()
env.render(mode="human")
```

### 3D

Visualising with FlightGear requires the Gym environment to be created with a FlightGear-enabled environment ID by changing 'NoFG' -> 'FG' and specifying the render mode as "flightgear"

```python
env = gym.make("JSBSim-TurnHeadingControlTask-Cessna172P-Shaping.STANDARD-FG-v0")
env.reset()
env.render(mode="flightgear")
```

## State and Action Space

JSBGym's environments have a continuous state and action space. The state is a 17-tuple:

```python
(name='position/h-sl-ft', description='altitude above mean sea level [ft]', min=-1400, max=85000)
(name='attitude/pitch-rad', description='pitch [rad]', min=-1.5707963267948966, max=1.5707963267948966)
(name='attitude/roll-rad', description='roll [rad]', min=-3.141592653589793, max=3.141592653589793)
(name='velocities/u-fps', description='body frame x-axis velocity [ft/s]', min=-2200, max=2200)
(name='velocities/v-fps', description='body frame y-axis velocity [ft/s]', min=-2200, max=2200)
(name='velocities/w-fps', description='body frame z-axis velocity [ft/s]', min=-2200, max=2200)
(name='velocities/p-rad_sec', description='roll rate [rad/s]', min=-6.283185307179586, max=6.283185307179586)
(name='velocities/q-rad_sec', description='pitch rate [rad/s]', min=-6.283185307179586, max=6.283185307179586)
(name='velocities/r-rad_sec', description='yaw rate [rad/s]', min=-6.283185307179586, max=6.283185307179586)
(name='fcs/left-aileron-pos-norm', description='left aileron position, normalised', min=-1, max=1)
(name='fcs/right-aileron-pos-norm', description='right aileron position, normalised', min=-1, max=1)
(name='fcs/elevator-pos-norm', description='elevator position, normalised', min=-1, max=1)
(name='fcs/rudder-pos-norm', description='rudder position, normalised', min=-1, max=1)
(name='error/altitude-error-ft', description='error to desired altitude [ft]', min=-1400, max=85000)
(name='aero/beta-deg', description='sideslip [deg]', min=-180, max=180)
(name='error/track-error-deg', description='error to desired track [deg]', min=-180, max=180)
(name='info/steps_left', description='steps remaining in episode', min=0, max=300)
 ```

 Actions are 3-tuples of floats in the range [-1,+1] describing commands to move the aircraft's control surfaces (ailerons, elevator, rudder):

 ```python
 (name='fcs/aileron-cmd-norm', description='aileron commanded position, normalised', min=-1.0, max=1.0)
 (name='fcs/elevator-cmd-norm', description='elevator commanded position, normalised', min=-1.0, max=1.0)
 (name='fcs/rudder-cmd-norm', description='rudder commanded position, normalised', min=-1.0, max=1.0)
 ```
