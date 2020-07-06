# Home Assistant Installation Guide

**1.** Install dependencies:
```sh
apt update && apt upgrade -y
apt install -y build-essential libssl-dev libffi-dev python-dev python3-pip
```

**2.** Install Home Assistant and dependencies
```sh
pip3 install homeassistant aiohttp_cors netdisco sqlalchemy
```

**3.** Once the install is complete, you will see output similar to the one bellow, indicating the install was successful:
```sh
Successfully installed MarkupSafe-1.1.1 PyJWT-1.7.1 aiohttp-3.6.1 aiohttp-cors-0.7.0 astral-1.10.1 async-timeout-3.0.1 attrs-19.3.0 bcrypt-3.1.7 certifi-2020.6.20 cffi-1.14.0 contextvars-2.4 cryptography-2.8 homeassistant-0.103.6 idna-ssl-1.1.0 ifaddr-0.1.7 immutables-0.14 importlib-metadata-0.23 jinja2-2.11.2 multidict-4.7.6 netdisco-2.8.0 pycparser-2.20 python-slugify-4.0.0 pytz-2020.1 pyyaml-5.1.2 requests-2.22.0 ruamel.yaml-0.15.100 sqlalchemy-1.3.18 text-unidecode-1.3 typing-extensions-3.7.4.2 voluptuous-0.11.7 voluptuous-serialize-2.3.0 yarl-1.4.2 zeroconf-0.27.1 zipp-3.1.0
```
**4.** Start Home Assistant
```
hass
```
**4.** Once the terminal output has settled, access the Home Assistant Web UI from the following address:

http://192.168.8.1:8123/
