# Hardware Setup



# Software Setup

Main components:
  * MQTT broker  
  * MATLAB Container and Backend  
  * Python Container and Backend  
  * Tracker (Vicon or Aruco)  
  * Robot Firmware  
  * Autonomy Stuff


## OS
1. Create live USB Ubuntu 18.04
	```
	sudo dd BS=4M if=/path/to/image.iso of=/dev/disk status=progress conv=fdatasync
	```
	
2. Install Ubuntu; minimal installation with 3rd party stuffs


## Network Setup

1. Authorize the router  
	1.   

2. Router setup as usual 


## Robotarium Network

1. Set workstation to static IP address, turn off automatic DNS
	```
	IP 		= 	192.168.1.8 
	Netmask 	= 	255.255.255.0
	Gateway 	= 	192.168.1.1
	DNS 		=  	8.8.8.8, 208.67.222.222
	```

## Dependencies

1. Install the following:
	```
	sudo apt-get install git vim net-tools arp-scan python3-pip conntrack docker-compose
	```

11. Setup directory structure

	```
	mkdir ~/git
	mkdir ~/software
	mkdir ~/user_code 
	```

2. Install Docker
	* Visit [the Docker website](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
	* Follow instructions under "Install using the Repository"
	* Manager Docker as non-root user (might not work, if so just `sudo`)
		```
		sudo usermod -aG docker $USER
		```

3. Clone the following
	```
	cd git   
	git clone https://github.com/robotarium/gritsbot_2   
	git clone https://github.com/robotarium/vizier  
	git clone https://github.gatech.edu/pglotfelter6/robotarium_nodes_docker  
	git clone https://github.com/robotarium/tag_tracker  
	git clone https://gatech.github.edu/pglotfelter6/robotarium_matlab_docker  
	git clone https://gatech.github.edu/pglotfelter6/robotarium_matlab_backend
	```

4. Install vizier
	```
	cd ~/git/vizier
	python3 -m pip install -e .
	```

5. In robotarium_nodes_docker (this runs all the time)

	1. Setup MQTT broker  
		```
		sudo docker pull emqx/emqx:latest		
		```
6. Create MATLAB Docker Container
	1. Download MATLAB Linux installer using student credentials  
	2. Create location for install  
		```
		mkdir ~/software/matlab  
		cd ~/software/matlab  
		```

		Unzip the installer here  
	3. Open a terminal in the following location
		```
		cd ~/git/robotarium_matlab_docker
		```
	4. Run the following in robotarium_matlab_docker to build the base
		```
		sudo ./docker_build_base.sh
		```
	5. Run container using the following
		```
		sudo xhost +
		sudo docker_run_base.sh '/home/robotarium/software/matlab
		./install	
		```
	6. Go through the install; skip image processing, dps
	7. Enter name as root
	8. Run matlab in container
		```
		matlab
		```		

		Change preferences>keyboard>shortcuts to windows from emacs
	9. Open a new terminal to commit the container
		```
		sudo docker ps  
		```

		Copy container ID  

		```
		sudo docker commit DockerIDgoesHere robotarium:matlab	
		``` 
	10. To run: 
		```
		cd ~/git/robotarium_matlab_docker
		./docker_run.sh
		```

7. Setting up the MATLAB Backend to control the robots
	```
	cd ~/git/robotarium_matlab_backend  
	cd robotarium-matlab-simulator  
	git submodule init  
	git submodule update  	
	git pull origin master  
	```

	To run:
	```
	cd ~/git/robotarium_matlab_docker  
	./docker_run.sh  
	```
	
	Launch matlab
	```
	run('server_init.m')
	```


# Interaction with Robots

1. Turn a robot and wait; if the MQTT broker is running then
	```
	python3 -m vizier.vizier --host 192.168.1.8   
	python3 -m vizier.vizier --host 192.168.1.8 --publish matlab_api/27 '{"v": 0.0, "w": 1.0}'  
	```
	
		

# TODO 

## Git

1. Move robotarium_nodes_docker to robotarium org
2. Clean up tag_tracker repo
3. On mqtt maybe a emqx directory
4. Check submodule clean up on robotarium_matlab_backend
