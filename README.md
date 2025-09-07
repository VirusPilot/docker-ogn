### docker version of https://github.com/VirusPilot/ogn-pi34?tab=readme-ov-file#automatic-setup-standard-script

### supported operating systems
Debian-based Linux Operating Systems (64bit Bookworm or newer)
- Ubuntu
- DietPi
- RaspiOS

### supported hardware architectures
- arm64 (64-bit ARM CPUs with hardware floating point processor)
- x64 (64-bit AMD/Intel CPUs)

### prepare
- identify SDR serial (e.g. 868), required for the `config.vars` below
- `sudo apt update && sudo apt install git`
- `git clone https://github.com/VirusPilot/docker-ogn`
- `cd docker-ogn`
- `bash <(wget -q -O - https://raw.githubusercontent.com/sdr-enthusiasts/docker-install/main/docker-install.sh)`
- you may be asked `Y/n` a couple of times, it is safe to answer all of them with `Y`
- `sudo usermod -aG docker $USER && newgrp docker`

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

### build
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
- `docker stop <container_name_or_id>` stop a running container
- `docker rm <container_name_or_id>` deactivate a stopped container
- `docker container prune` deactivate all stopped containers
- `docker image ls` list docker images
- `docker rmi <image_id_or_name>` delete docker image
- `docker image prune` delete all docker images
- `docker system prune -a --volumes` clean your docker environment
