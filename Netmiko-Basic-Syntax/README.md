# Netmiko Basic Syntax

## "Connect using a dictionary" example from Netmiko:

```
from netmiko import ConnectHandler
from getpass import getpass

cisco1 = {
    "device_type": "cisco_ios",
    "host": "cisco1.lasthop.io",
    "username": "pyclass",
    "password": getpass(),
}

net_connect = ConnectHandler(**cisco1)
print(net_connect.find_prompt())
net_connect.disconnect()
```

- "host" inside "cisco1" dictionary is the IP address or DNS name used to reach the device over the network. Not the hostname configured on device.
- "username" refers to the local username configured on the device
```
(config)# username __STRING__ privilege 15 secret __STRING__
```
- "password" is using getpass() in Netmiko's example. Its acting like input(), asking for device's password (secret). 
- the ".find_prompt()" method is to return the current CLI line on device.



## "Enable mode" example from Netmiko:
