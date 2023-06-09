# RaspyHA

Short personal reminder of commands for spinning up a Zigbee2MQTT + HA on a Raspberry

## Official doc
+ https://www.zigbee2mqtt.io/guide/installation/01_linux.html#optional-running-as-a-daemon-with-systemctl
+ https://mosquitto.org/download/
+ https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script
+ https://www.home-assistant.io/installation/raspberrypi#install-home-assistant-container

## Commands

### Basics
```
sudo raspi-config
sudo apt update
sudo apt upgrade -y
sudo apt auto-remove
```

### Mosquitto (MQTT Broker)
```
sudo apt install -y mosquitto
```

### Zigbee2MQTT
#### Install
```
sudo curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs git make g++ gcc
sudo mkdir /opt/zigbee2mqtt
sudo chown -R ${USER}: /opt/zigbee2mqtt
git clone --depth 1 https://github.com/Koenkk/zigbee2mqtt.git /opt/zigbee2mqtt
cd /opt/zigbee2mqtt
npm ci
```

#### Configuration and start
```
nano /opt/zigbee2mqtt/data/configuration.yaml
cd /opt/zigbee2mqtt
npm start
```

### Docker
```
cd
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo usermod -aG docker ${USER}
sudo reboot
```

### Home Assistant
```
cd
mkdir HA
sudo tee <<EOF >/dev/null ~/HA/docker-compose.yml
version: '3'
services:
  homeassistant:
    environment:
      - TZ=Europe/Paris
    container_name: homeassistant
    image: "ghcr.io/home-assistant/home-assistant:stable"
    volumes:
      - ~/docker/homeAssistant/data:/config
      - /etc/localtime:/etc/localtime:ro
    restart: unless-stopped
    privileged: true
    network_mode: host
EOF
cd HA
docker compose up
```