# Robotarium Flight Rules

- Port 1884
- IP: 192.168.1.8

### Startup:
1. Vicon
    - Turn Vicon On (switch | should be pressed)
2. Charging Stations
    - Pull Red Knobs to activate charging stations
    - Charging Stations (take a while to work ~10 mins)
    - There are 6 raspberry-pis running all the charging stations
    - Their Mac addresses can be obtained on: git/power_distribution/config/mac_list.json
    - To obtain their IPs check out the code from the ``shutdown_robots.sh`` script (see shutdown section)
3. Robots
    - Switch Them On
    - Their Mac addresses can be obtained on: git/gritsbot_2/config/mac_list.json
    - To obtain their IPs check out the code from the ``shutdown_robots.sh`` script (see shutdown section)
4. Center of Mass Calibration
    - The script for com calibration is in utilities/static_data_creation/learn_com
    - First delete the old com errors from the static_data folder
    - Place new file in static data
5. Calibrate Projector
    - under utilities/projector allows you to control axis of projector
    - Overwrite the project_axis_limits.mat (put one in static_data, and leave one in the utilities/projector folder)
6. Recording the Charging Stations
    - We record the id’s of the robots when they’re sitting on the charging stations. These are the points they drive to. The file is under: utilities/, same deal.

### Matlab BackEnd:
- Run “xhost +” before running the container  to be able to bring up GUIs
- Make sure you are connected
    - Wifi: Wired Connection 1, Wired Connection 2, robotarium api.
    - Check if config to check the correct wireless IP address is up (look for wlp5s0) by running ``ifconfig``. IP should be 192.168.1.8
- Make sure that the correct docker containers are on
    - To see a list of the docket containers that are on run ``sudo docker ps -a``
    - To restart a docker container run ``sudo docker restart <insert id>``
    - The dockers that should be on are the vicon_tracker, emqtt, robotarium_base_node.

### Running User Experiments:
- Bring Up Job Supervisor (Runs Everything)
    - From the git/robotarium_job_supervisor repo, command: ``sudo python3 automatic_run.py``
- If you want to run only a single specific one:
    - Uncomment last bit of last line in ``automatic_run.py (-exp_id $1) and use Command is: ./docker_run.sh``

### Shutdown:
- From desktop in git/gritsbot_2/interfacing run: ``./shutdown_robots.sh``
- ``sudo python3 get_ip_by_mac.py ../../power_distribution/config/mac_list.json wlp5s0 -c “sudo shutdown now”``
- Turn off the robots
- Then then red knobs under the windows
- Turn off projector
