### docker version of [VirusPilot ogn-pi34 standard install script](https://github.com/VirusPilot/ogn-pi34?tab=readme-ov-file#automatic-setup-standard-script)

### NEW: native support of RTL-SDR Blog v4 SDR since Debian 13 Trixie

### supported operating systems
Debian-based Linux Operating Systems (64bit Debian 13 Trixie or newer)
- Debian
- Ubuntu
- DietPi
- RaspiOS

### supported hardware architectures
- arm64 (64-bit ARM CPUs with hardware floating point processor)
- x64 (64-bit AMD/Intel CPUs)

### prepare system
- `sudo apt update && sudo apt install git`
- you may be asked `Y/n` a couple of times, it is safe to answer all of them with `Y`

### prepare docker
- `wget -q https://raw.githubusercontent.com/sdr-enthusiasts/docker-install/main/docker-install.sh && chmod +x docker-install.sh`
- `nano docker-install.sh` and remove/disable lines #33 and #41 (software-properties-common, netcat)
- `./docker-install.sh`
- you may be asked `Y/n` a couple of times, it is safe to answer all of them with `Y`
- `sudo reboot`

### prepare SDR
- identify or set SDR serial (e.g. 868), it is are required for the `config.vars` below
- to change the SDR serial, leave only the SDR to be used for 868 MHz reception plugged in, then issue the following command:
  - `docker run --rm -it --device /dev/bus/usb --entrypoint rtl_eeprom ghcr.io/sdr-enthusiasts/docker-adsb-ultrafeeder -s 868` 

### prepare ogn
- `git clone --branch trixie https://github.com/VirusPilot/docker-ogn`

### configuration
- `cd ./docker-ogn`
- `nano config.vars`
  - save changes with `CTRL O`
  - exit nano with `CTRL X`

| variable | example | description |
| :--- | :--- | :--- |
| STATION_LAT | 50.0 | your station latitude |
| STATION_LON | 10.0 | your station longitude |
| STATION_ALT_MSL_M | 300 | your sattion altitude AMSL [m] |
| STATION_NAME | OGNTEST | your max. 9 letter station name, please comply with [naming convention](http://wiki.glidernet.org/receiver-naming-convention) |
| FREQ_PLAN | 1 | 1=Europe/Africa (default), 2=USA/Canada, 3=South America/Australia, 4=New Zeeland |
| GSM_CENTER_FREQ | 935.8 | default = 0, change only if you know your closest GSM900 station frequency [MHz] |
| SDR_868_SERIAL | 868 | enter your OGN SDR serial |
| SDR_868_PPM | 0 | change only if you know your SDR's ppm |
| SDR_868_BIAS_T_ENABLE | 0 | set to 1 to enable Bias Tee on your SDR, e.g. to power a LNA |

### build
- `cd ./docker-ogn`
- `docker compose up --detach --build`
- you may be asked `Y/n` a couple of times, it is safe to answer all of them with `Y`
- `sudo reboot`

### apply configuration changes
- `cd ./docker-ogn`
- `nano config.vars`
- `docker compose up --detach --build`

### monitor OGN details
- `http://yourReceiverIP:8080`
- `http://yourReceiverIP:8081`

### monitor docker container
- `docker logs -f rtlsdr-ogn`

### useful docker commands
- `docker ps -a` list all docker containers, including stopped ones
- stop and deactivate containers
  - `docker stop <container_name_or_id>` stop a running container
  - `docker rm <container_name_or_id>` deactivate a stopped container
  - `docker container prune` remove all stopped containers
  - `docker compose down` stop and remove containers, networks
  - `docker compose up --detach` create and start containers
  - `docker compose up --detach --build` build, create and start containers
- list and delete docker images
  - `docker image ls` list docker images
  - `docker rmi <image_id_or_name>` delete docker image
  - `docker image prune` delete all docker images
- clean your entire docker environment e.g. for a fresh `docker compose`
  - `docker rm -f $(docker ps -aq)` force remove ALL containers
  - `docker system prune -af --volumes` clean your docker environment
- open a shell inside your container
  - `docker exec -it <yourDockerContainer> bash`
