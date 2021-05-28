# Scavenger Hunt
This repository contains the API tools to enable new robot platforms to interact with the Scavenger Hunt website, which hosts challenges to be carried out by _your_ robot! Complete tasks and upload proof to get points. 

The goal? Get your organization to the top of the leaderboard while expanding your robot's capabilities, and challenging others to do the same!

## Installing

In the `src/` folder of a catkin workspace:

* `$ git clone https://github.com/utexas-bwi/scavenger_hunt_api.git --recursive` 

build using catkin tools; `$ catkin build` in the root of your catkin workspace.

# Instructions for New Users

## Getting Started 

A short description of steps to help orient yourself around the Scavenger Hunt interface follows:

- [Create an account](https://scavenger-hunt.cs.utexas.edu/public_html/register.php) and login
- Download this repo and modify config files
- Use the website to create a hunt and associate some tasks
- Write your code to load the hunts' tasks, execute them and send the proofs after each task is done  


### Using the Scavenger Website

In order to participate in the Scavenger Hunt challenge, you will need an account. Please create on at the [Scavenger Hunt main page](https://scavenger-hunt.cs.utexas.edu/). 
Once you have an account, you can:

+ create new tasks
+ create scavenger Hunts
+ verify your own, and other participants uploaded proofs
+ view the leaderboards


### Using the API

If you are a new user that wants to participate, you can use this repository to interact with the Scavenger API.
The `scavenger_hunt_api` folder of this repository of the same name contains ROS wrappers for interacting with 
the Scavenger Hunt website so you can submit proofs and participate in Hunts. 

Please see the [API](https://scavenger-hunt.cs.utexas.edu/public_html/api.php) webpage for details on the API, which includes code samples to query existing Hunts and submit proofs. 

Once you've implemented the API code samples to enable your robot to interact with the website, the only part left is write the code that allows your robot to perform the task! After your robot is able to perform a task, your program should use the Scavenger Hunt API to submit an image or video proof (depending on the task). 

After submitting a proof, you should be able to see it on the Proofs page on the Scavenger Hunt website. This will be visable to other users of the website as well, who can confirm or deny whether your robot's attempt was successful.

Good luck!


# Instructions for BWI Platforms

This repository also contains the code used to perform Scavenger Hunts on BWIbots. This can be run on a BWIbot, or used as a reference implmentation for your own robot platform.

## BWI Scavenger Startup

The following sections outlines what is required to run the full Scavenger Hunt codebase on the BWIBot platforms. 
The BWI Scavenger code may serve as an example for how to implement a scavenger hunt, fetching and starting Hunts, and submitting proofs.

There are two options for image recognition: offloading the image processing to an offboard computer if the robot does 
not have a GPU, or running YOLO directly on the robot computer if a GPU is available. 

In the case that a GPU is not available, `darksocket` is used to facilitate data flow.

### Repo Overview
Complete codebase for BWI's Scavenger Hunt project.

`website/` contains everything relating to the website. All other directories
are ROS packages for Scavenger Hunt robots. Some important ones:

* `scavenger_hunt_api/` - The Scavenger Hunt API. Contains utilities for connecting
to the website, uploading proofs, and querying proof feedback. Non-ROSified,
though there is an optional ROS wrapper.
* `bwi_scavenger/` - Drivers for our Scavenger Hunt participant robots.
* `darksocket/` - Tool for offboard darknet computing.

### Initial Configuration

* Verify launch parameters in `bwi_scavenger/config/bwi_scavenger.yaml`
  * `scav_hunt_login` - Valid website credentials
  * `ws_path` - Fully-qualified path to the workspace containing these packages
  * `world` - Either `sim` if running in Gazebo or `irl` if running in the lab
  * `topics/perception/xxx` - Camera topics should be prefixed with `/nav_kinect` for IRL and `/camera` for simulation
* Verify launch parameters in `scavenger_hunt_api/config/scavenger_hunt.yaml`
  * `website/domain` - Domain of Scavenger Hunt website (until the web server is
    up this is `localhost` for lab machines and `localhost:8080` for robots)
  * `scratch_path` - Valid path that ROS has permission to create files in
    (we usually use the workspace path)
* Verify darksocket config in `darksocket/scripts/darksocket.conf`
  * Robot and offboard machine
    * `host` - IP of robot (find with `hostname -I`; should be the second IP printed)
    * `port` - we usually use 51820
  * Offboard machine only
    * `darknet_path` - Location of darknet binary (built with `GPU=1`, otherwise you're undermining the purpose of darksocket)
    * `cfg_path` - Fully-qualified path of cfg file
    * `weights_path` - Fully-qualified path of weights file
    * `meta_path` - Fully-qualified path of meta file (which in turn contains the fully-qualified path of the names file)
    * `recv_img_path` - Fully-qualified path to store a received image in (including image file name)
* Verify darksocket launch parameters in `darksocket/launch/darksocket_ros.launch`
  * `topics/camera` - Camera topics should be prefixed with `/nav_kinect` for IRL and `/camera` for simulation
  * `conf_path` - Fully-qualified path to `darksocket.conf`


### Robot Startup

1. Boot up the scavenger robot. Before any ROS
commands, open an SSH tunnel on the robot so it can see the website. This is required only for instances where `darksocket` is used to offload image processing to another machine.

```
ssh -D 8080 http://utw10989.utweb.utexas.edu/
```

2. Navigate to the workspace and run the following launch files.

```
roslaunch bwi_launch segbot_v2.launch
roslaunch bwi_scavenger scavenger.launch
rosrun bwi_scavenger absim_ros.py
python3 src/bwi_scavenger/scripts/absim_socket.py
```

3. Run darksocket on the offboard machine. If everything was set up correctly, it should load the network and then print "connection established." Again, only required if using `darksocket`.

```
python darksocket.py client
```

4. Localize the robot in rviz and start a hunt via the `do_hunt` node.

```
rosrun bwi_scavenger do_hunt "BWI Lab Hunt"
```

`BWI Lab Hunt` is the name of our test hunt, though any hunt registered on the
website will work.
