# Scavenger Hunt

Complete codebase for BWI's Scavenger Hunt project.

`website/` contains everything relating to the website. All other directories
are ROS packages for Scavenger Hunt robots. Some important ones:

* `scavenger_hunt_api/` - The Scavenger Hunt API. Contains utilities for connecting
to the website, uploading proofs, and querying proof feedback. Non-ROSified,
though there is an optional ROS wrapper.
* `bwi_scavenger/` - Drivers for our Scavenger Hunt participant robots.
* `darksocket/` - Tool for offboard darknet computing.

## Installing

In the `src/` folder of a catkin workspace:

* `$ git clone https://github.com/utexas-bwi/scavenger_hunt_api.git --recursive` 

build using catkin tools; `$ catkin build` in the root of your catkin workspace.

## Using the Scavenger Hunt API 
The `scavenger_hunt_api` folder of this repository of the same name contains ROS wrappers for interacting with 
the Scavenger Hunt website so you can submit proofs and participate in Hunts. 

This leaves the implementation of how to carry out the hunts up to the individual.

Please see the [API](https://scavenger-hunt.cs.utexas.edu/public_html/api.php) webpage for details on the API.
