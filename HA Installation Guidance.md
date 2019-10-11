#Home Assistant Installation Guidance

Installation Procedure

- Prepare build environment
```
apt-get install build-essential libssl-dev libffi-dev python-dev python3-pip
```


- Install dependent pip package
```
pip3 install aiohttp_cors sqlalchemy PyNaCl
```

- Install homeassistant
```
python3 -m pip install homeassistant
```
When you see the prompt:

"Successfully installed MarkupSafe-1.1.1 PyJWT-1.7.1 
aiohttp-3.6.1 astral-1.10.1 async-timeout-3.0.1 attrs-19.2.0 bcrypt-3.1.7 
certifi-2019.9.11 cffi-1.12.3 contextvars-2.4 cryptography-2.7 homeassistant-0.100.0 
idna-ssl-1.1.0 immutables-0.10 importlib-metadata-0.23 jinja2-2.10.3 more-itertools-7.2.0 
multidict-4.5.2 pycparser-2.19 python-slugify-3.0.4 pytz-2019.3 pyyaml-5.1.2 requests-2.22.0 
ruamel.yaml-0.15.100 text-unidecode-1.3 typing-extensions-3.7.4 voluptuous-0.11.7
voluptuous-serialize-2.3.0 yarl-1.3.0 zipp-0.6.0"

It represents a successful initial installation.

- Start hass
```
hass --open-ui
```

Trouble shooting for prompt like:
"Unable to import ssdp: No module named 'netdisco'"

```
pip3 install netdisco
```

When you see the prompt "Starting Home Assistant", Home Assistant Installation is successful.
After performing the above steps, you can open a browser and enter the url: 

http://192.168.8.1:8123/onboarding.html

You can see web of Home Assistant.
