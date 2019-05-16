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
	sudo apt-get install git vim net-tools arp-scan python3-pip conntrack docker-compose maven python3-tk
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
	git clone https://github.com/robotarium/mqtt_broker
	git clone https://github.com/robotarium/tag_tracker  
	git clone https://github.com/robotarium/python_tag_tracker
	git clone https://github.com/robotarium/matlab-java-client
	git clone https://gatech.github.edu/pglotfelter6/robotarium_matlab_docker  
	git clone https://gatech.github.edu/pglotfelter6/robotarium_matlab_backend
	git clone https://github.gatech.edu/pglotfelter6/matlab_node
	git clone https://gatech.github.edu/pglotfelter6/robotarium_python_backend
	```

4. In mqtt_broker (this should run all the time)

	1. Setup MQTT broker  
		```
		cd ~/git/mqtt_broker/docker
		./docker_build.sh
		```

	2. Run broker on start up:
		```
		Automagical procedure goes here.
		```		

5. Install vizier
	```
	cd ~/git/vizier
	python3 -m pip install -e .
	```
	
	Test: 

	1. Turn a robot and wait; if the MQTT broker is running then

		```
		python3 -m vizier.vizier --host 192.168.1.8   
		python3 -m vizier.vizier --host 192.168.1.8 --publish matlab_api/27 '{"v": 0.0, "w": 1.0}'  
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

8. Vizier implementation in java for MATLAB (default included in `robotarium_matlab_backend`, use this for updates):

	```
	cd ~/git/matlab-java-client
	./build.sh
	cd ./target
 	cp *.jar ~/git/matlab_node/vizier_java.jar	
	cd ~/git/matlab_node/
	```

	Build the jar:

	```
	./make_repo.sh vizier_java.jar
	./build.sh
	cd target
	mv *.jar ~/git/robotarium_matlab_backend/jars/matlab_node.jar
	```

9. Setup the Tracker:
	1. Camera-based tracking (Dependencies are included in the container):

		Install:

		```
		cd ~/git/python_tag_tracker/docker
		./docker_build.sh
		```

		Test it with:

		```		
		./docker_run.sh  
		```  

10. Python Backend Setup  

	1. Prior to Install    
		
		```
		cd ~/git/robotarium_python_backedn_
		cd robotarium-python-simulator  
		git submodule init  
		git submodule update  	
		git pull origin master  
		./run_before_install.sh
		```
		
	2. Install
		
		```
		cd ./robotarium_python_simulator
		python3 -m pip install -e .
		```	

# Startup Procedure


# TODO 

## Git

1. Move robotarium_nodes_docker to robotarium org
2. Clean up tag_tracker repo
3. On mqtt maybe a emqx directory
4. Check submodule clean up on robotarium_matlab_backend
5. restructure python backend

## Workstation

1. Launch broker on startup. 


## Instructions

1. Vicon instructions
2. Add tests to each section
3. Organize sections into primary components 

## QOL

1. Make it easy to change host ip in everything
