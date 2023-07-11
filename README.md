# Installation of Home Assistant Core on M1 Mac mini

## Home Assistant in Python Virtual Environment

### Installation of brew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
(echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
brew install autoconf
```

### Installation of Home Assistant
```
python3 --version
pip3 install --upgrade pip
pip3 install virtualenv
python3 -m venv homeassistant
source homeassistant/bin/activate
pip3 install homeassistant
cd homeassistant/bin/
./hass
```

#### Running Home Assistant in background
```
hass --script macos install
```

#### Update Home Assistant
```
source homeassistant/bin/activate
cd homeassistant/bin/
hass --script macos uninstall
pip3 install --upgrade homeassistant
hass --script macos install
```

## Installation of Mosquito Broker
```
brew install mosquitto
nano /opt/homebrew/etc/mosquitto/mosquitto.conf
cd /etc
mkdir  mosquitto
sudo mosquitto_passwd -c /etc/mosquitto/passwd homeassistant
mosquitto -v
```

#### Running Mosquito in background
```
brew services start mosquitto
```

## Installation of Zigbee2MQTT
```
sudo mosquitto_passwd /etc/mosquitto/passwd zigbee2mqtt
brew services restart mosquitto

git clone --depth 1 https://github.com/Koenkk/zigbee2mqtt.git ~/zigbee2mqtt
cd ~/zigbee2mqtt
npm ci
npm start
```

#### Location of Zigbee Adapter
```
ls /dev
ls /dev/tty.usbserial-220
```

#### Running Zigbe2MQTT in background
```
sudo npm install pm2 -g
pm2 --name zigbee2mqtt start npm -- start
pm2 startup
pm2 save
````

## Installation of HACS
```
brew install wget
wget -O - https://get.hacs.xyz | bash -
```

## Installation of PS5-MQTT

```
sudo mosquitto_passwd /etc/mosquitto/passwd ps5-mqtt
brew services restart mosquitto

git clone https://github.com/FunkeyFlo/ps5-mqtt.git ~/ps5-mqtt
cd ps5-mqtt/ps5-mqtt

sudo npm ci
chmod a+x run.sh
sudo npm run build
sudo npm start
```

#### run.sh
```
#!/bin/sh

export CONFIG_PATH="/data/options.json"
export CREDENTIAL_STORAGE_PATH="config/credentials.json"
export MQTT_HOST='localhost'
export MQTT_PORT='1883'
export MQTT_USERNAME='ps5-mqtt'
export MQTT_PASSWORD=‘REDACTED’
export FRONTEND_PORT=8645

# configure logger
export DEBUG="*,-mqttjs*,-mqtt-packet*,-playactor:*,-@ha:state*,-@ha:ps5:poll*,-@ha:ps5:check*"

echo Starting PS5-MQTT...
node server/dist/index.js
```

#### package.json
```
{
    "name": "ps5-mqtt",
    "workspaces": [
        "server",
        "client"
    ],
    "private": true,
    "scripts": {
        "start": "sh run.sh",
        "build": "npm run build --workspaces --if-present",
        "test": "jest"
    },
    "devDependencies": {
        "@jest/globals": "29.3.1",
        "@types/jest": "^29.2.2",
        "@typescript-eslint/eslint-plugin": "^5.41.0",
        "@typescript-eslint/parser": "^5.40.1",
        "eslint": "^8.24.0",
        "eslint-plugin-react": "^7.31.8",
        "jest": "^29.3.0",
        "redux-saga-testing": "^2.0.2",
        "ts-jest": "^29.0.3",
        "ts-node": "^10.9.1",
        "typescript": "^4.8.3"
    }
}
```

#### Running PS5-MQTT in background
_Authorize each console before running script in background._
```
pm2 --name ps5-mqtt start npm -- start
pm2 save
```

## Installation of ESPHome

```
/usr/sbin/softwareupdate --install-rosetta --agree-to-license
pip3 install wheel esphome
esphome dashboard --open-ui ESPHome
```

# Dashboards
|Mobile|Tablet|
|---|---|
|![](https://user-images.githubusercontent.com/9057322/232841689-82f75744-165b-4e1f-8d83-de346ec01126.mov)|![](https://user-images.githubusercontent.com/9057322/232843773-9b5d5a87-424a-4e83-be59-5b67638338f9.jpeg)|




