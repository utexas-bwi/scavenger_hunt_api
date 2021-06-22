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

Please see the [API](https://scavenger-hunt.cs.utexas.edu/public_html/api.php) webpage for details on the API.
