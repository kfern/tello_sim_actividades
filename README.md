# tello_sim

[**tello_sim**](https://github.com/Fireline-Science/tello_sim) is a simple Python simulator (sim) that can be used by students to test their [tello](https://www.ryzerobotics.com/tello-edu) flight plans before deploying them to a real drone. It was inspired by the [easyTello](https://github.com/Virodroid/easyTello) library and uses it for the drone interface.

After simulating their flight, they can then deploy the same code to a real drone to see how their model performs in the real world. In order to control the flight of the actual drones, the facilitator can take the output from each team or student (via the save command discussed below) and deploy it from a single computer. Students can then observe how different approaches work in the real world with the actual drone.

An exercise like this supports the United States' Next Generation Science Standards for K12 related to [distinguishing between a model and the actual object, process, and/or events the model represents](https://ngss.nsta.org/Practices.aspx?id=2).  

The sim was developed for use in a Jupyter notebook or QT console so that the plots are displayed inline with the code print outputs. The sim currently supports a subset of the full DJI command set including: takeoff, land, forward, back, left, right, up, down, flip, cw, and ccw.


## Cloud Notebook Option
If you don't want to install Jupyter on your local machine, you can also use the free [mybinder](https://mybinder.org/) cloud-based Jupyter notebook service. While this service will allow you to use the simulator, you will not be able to deploy your simulated flight to a real drone given the code will be running on a remote server.

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/kfern/tello_sim_actividades/apd01?filepath=como_volar.ipynb)


## Sim Examples

The sim is built to run interactively in a Jupyter notebook or QT Console. The sim class outputs both text prompts and plots with each
simulated command.

Creating a simulated drone object in Python:
```python
from tello_sim import Simulator

my_drone = Simulator()
```
![](/images/ready.png)

```python
my_drone.takeoff()
```
![](/images/takeoff.png)

```python
my_drone.forward(40)
```
![](/images/forward.png)

```python
my_drone.cw(45)
```
![](/images/cw.png)

```python
my_drone.forward(50)
```
![](/images/forward_2.png)

```python
my_drone.land()
```
![](/images/land.png)

## Saving and Loading Command scripts
In a classroom, it can be useful to allow students to share their command scripts or allow them to send them to a teacher which can be the gatekeeper for sending the scripts to the real drones. To facilitate that, we have two functions that allow you to save and load commands scripts. The scripts are a simple JSON document formatted as follows:

```json
[
    {
        "command": "command",
        "arguments": []
    },
    {
        "command": "takeoff",
        "arguments": []
    },
    {
        "command": "forward",
        "arguments": [
            100
        ]
    },
    {
    	"command": "cw",
	"arguments": [
	    90
	]
    },
    {
    	"command": "forward",
	"arguments": [
	    100
	]
    },
    {
        "command": "land",
        "arguments": []
    }
]
```

To save the commands built up in an interactive console, do the following:

```python
my_drone.save(file_path='save_file.json')
```

To load a command file, do the following. Note that the `load_commands` function resets your drone object, so any saved commands will be cleared.

```python
my_drone.load_commands(file_path='new_commands.json')
```

## Resetting Simulator States
To reset the state of your simulator for a given object, use the following:

```python
my_drone.reset()
```

## Deploying to a Real Drone
We are using the [easytello](https://github.com/Virodroid/easyTello) library to allow you to deploy your simulated flight to a real drone. Once you are connected to your drone via WiFi, you can deploy the commands you built up in an interactive session or loaded via a command file in Jupyter with the following command:

```python
my_drone.deploy()
```

## Running Multiple Command Scripts in the Same Session
Note: if you are running multiple scripts to the drone, you may have to kill the process that binds the python process to the Tello port if you receive a `OSError: [Errno 48] Address already in use` error. You can search for and kill the process as follows in a linux-like console:

```
lsof -i:8889
kill XXXX
```
