# Description

DustLink is a web-based SmartMesh IP management application with the following features:
- Connects to an arbitrary number of SmartMesh IP managers.
- Gathers and displays information about motes, managers and networks.
- Displays the topology of the low-power wireless mesh networks.
- Stores and displays sensor data from arbitrary applications.
- Monitors the health of all connected networks.
- Bridges to the Xively cloud service as well as Google Spreadsheet to publish data.
- Uses advanced privileges for fine-grained user access management.

The application is written entirely in Python and is intended for use as a demo vehicle and a quick prototyping tool. DustLink can be installed on any computer that supports Python.

## Docker file build notes:

### Build docker image:
`docker build -t dustlink .`

On MacOS the device map to the container only works if the vitualisation engine supports USB serial ports. Docker on Mac does not support it with native Hypervisor.

### Installation:
- Install newest version Docker for Mac. It may contain a newer version of docker and docker-machine.
- Install newest Kitematic from [Github releases](https://github.com/docker/kitematic/releases)
- Install newest VirtualBox from Oracle
- Install [Docker Toolbox for Mac](https://download.docker.com/mac/stable/DockerToolbox.pkg). It my contain an older Kitematic version. Remove it from *Docker* folder in the *Applications* directory. Also remove `docker, docker-machine, docker-compose` from the `/usr/local/bin` folder.

### Run container in VirtualBox
- Hook up the DUST Network dongle DC2274A-A to a USB port of the host computer. Open a terminal and verify that it has been recognized by the host. The command `ls /dev/tty*` lists lines similar to /dev/tty.usbserial[ID]
- Start the docker machine from the "Docker Quickstart Terminal" which comes with the Docker Toolbox for Mac. It creates an active docker-machine named *default* in VirtualBox. Verfify with `docker-machine ls` and see the asterik:
```
    NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
    default   *        virtualbox   Running   tcp://192.168.99.100:2376           v18.06.1-ce
```
- Start VirtualBox, select the container named *default* and goto *Settings|Ports|USB*. Tick the *Enable USB Controller* box and select the *USB 2.0* radio button option.
- Click *Use Device Filer* add + and select *LTC DC2274 WITH MEMORY [0800]*
- Start the docker-machine container named *default* within VirtualBox
- Start Kitematic - eventually restart it with *Use Virtualbox* option
- In Kitematic goto the tab *My Images*, there select the *dustlink* tile and create a container
- When the container shows up on the left side, select it and goto the *Settings* tab at the top right of the window
- There on the *Advanced* tab enable the tickbox *Privileged Mode*. This will map the USB serial devices to the docker container. To verify click the *EXEC* button on the menu bar. Enter `ls /dev/tty*` into the upcoming terminal. It will list `/dev/ttyUSB0` to `/dev/ttyUSB3`. Use the `/dev/ttyUSB3` within the DUSTlink application.
