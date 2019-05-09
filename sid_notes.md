
Setup a robotarium from scratch!

## Hardware Stuffs
1. computer
2. camera
3. router

## Network Stuffs
1. Connect router via ethernet to a port. (auth the router to get internet via the GT network)
2. Set a static IP
   1. Change IPv4 method to manual
   2. Address: 192.168.1.8
   3. Netmask: 255.255.255.0
   4. Gateway: 192.168.1.1
   5. turn off automatic DNS
   6. Add DNS address: 8.8.8.8, 8.8.8.4
   7. Check if you have internet (may have to deactivate and activate connection)

## Software Stuffs
Starting from a base Ubuntu 18.04 install....
### Installing basic dependencies:
1. git
> sudo apt-get install git
2. vim
> sudo apt-get install vim
3. Docker:
   * Visit the [Docker CE for Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/) website. Use the instructions under "Install using the repository option"
> sudo groupadd docker

> sudo usermod -aG docker $USER

4. net-tools
> sudo apt-get install net-tools
5. arp-scan
> sudo apt-get install arp-scan
6. python3: should be already installed
7. pip
> sudo apt-get install python3-pip
8. conntrack
> sudo apt-get install conntrack
9. docker-compose
> sudo apt-get install docker-compose
### Clone the following repositories:
Create a directory ~/git. Clone the following repos here:

7. gritsbot_2 repository
> git clone https://github.com/robotarium/gritsbot_2
8. Vizier node
> git clone https://github.com/robotarium/vizier
   * Enter vizier folder and run: (-e option for developer mode)
   > python3 -m pip install -e .
9. Get robotarium nodes docker
> git clone https://github.gatech.edu/pglotfelter6/robotarium_nodes_docker

10. Get tag tracker repository
> git clone https://github.com/robotarium/tag_tracker

11. Robotarium matlab docker

> git clone https://gatech.github.edu/pglotfelter6/robotarium_matlab_docker

12. Robotarium matlab backend

> git clone https://github.gatech.edu/pglotfelter6/robotarium_matlab_backend

### Setting Up MQTT Broker

> sudo docker pull emqx/emqx:latest

> sudo docker images // just to check : emqx should show up


### Creating a docker container for MATLAB

1. Download latest MATLAB installer for Linux (zip file)
2. Put matlab in a nice folder
> mkdir ~/software && cd ~/software && mkdir matlab && cd matlab

   * Copy zip file and extract into this folder.

3. Back in git/robotarium_matlab_docker:
> cd ~/git/robotarium_matlab_docker
> sudo ./docker_build_base.sh

4. Open docker_run_base.sh (inside git/robotarium_matlab_docker) Ensure mac address never changes, next line is for X11 forwarding (purely explanatory)
5. Run docker_run_base:

> sudo xhost+   (every time you restart your system)

> sudo ./docker_run_base.sh "/home/#USER_NAME/software/matlab" // path to your matlab installation files

6. Container opens (you should see a folder called matlab). Entering this folder, you should see all the matlab install files
7. Run ./install in this folder:

> ./install

8. You should see a matlab splash pop up for matlab installation. Enter user account and follow through instructions. It will complain about the lack of a browser, and give you a link. Copy that to your browser. Once approved, go back to the docker and click next.
9. Only use MATLAB, Optimization toolbox, Statistics and ML Toolbox, Symbolic Math tool box, mapping toolbox. Click next, add symlink (tick the box). Once installed, activate matlab. Login name: root.
10. DO NOT EXIT THE CONTAINER
11. Check matlab works by typing "matlab" in command window.
12. Open preferences, keyboard, shortcut: change to windows default set.
13. Open another terminal (inside the container).

> sudo docker ps

14. You should see robotarium:matlab_base
15. Copy container ID. Then in the host machine, while that docker container is still open, run:

> sudo docker commit #CONTAINER_ID robotarium:matlab (WHATEVER YOU WANNA CALL IT)

16. Check its presence in sudo docker images (on host machine)
17. Now you can safely exit the docker container.
18. back to ~/git/robotarium_matlab_backend

> ./docker_run.sh  // before this see step 1 of Setting up MATLAB backend section i.e., create ~/user_code

19. The container should open up.


### Setting up MATLAB Backend (which talks to the robots)

> cd ~/ && mkdir user_code

1. Inside robotarium_matlab_backend (ensure its up-to-date with a git pull), enter robotarium_matlab_simulator and then:

> git submodule init

> git submodule update

> git pull origin master

> cd ~/git/robotarium_matlab_docker

> ./docker_run.sh

> matlab

2. This opens up matlab. Simulator files should be in it.

### Install Python Backend

### Set up the tracker

### Robot Firmware

### Basic testing with the robots

1. When robots turn on, arp-scan for the robot to see if it is on the wifi
> sudo arp-scan -I eno1 -l // here eno1 is the name of network we are on

Docker on the robots may take a while to come up. You can ssh into the raspberry pi on the robot, and check with "docker ps" if things are up and running

2. To listen in on data coming from the robots, run the following
> python3 -m vizier.vizier --host #HOST_IP --get #ROBOT_NUMBER/status //CHECK STATUS OF TEST ROBOT

> python3 -m vizier.vizier --host 192.168.1.8 --publish matlab_api/27 '{"v": 0.0, "w": 1.0}' //SEND TEST COMMANDS TO GRITSBOTX
