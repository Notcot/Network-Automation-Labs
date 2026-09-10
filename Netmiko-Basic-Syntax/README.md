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
- "cisco1"'s dictionary can be think of as a boilerplate.
- "host" inside "cisco1" dictionary is the IP address or DNS name used to reach the device over the network. Not the hostname configured on device.
- "username" refers to the local username configured on the device
```
(config)# username __STRING__ privilege 15 secret __STRING__
```
- "password" is using getpass() in Netmiko's example. Its acting like input(), asking for device's password (secret). 
- the ".find_prompt()" method is to return the current CLI line on device.

```
variable = ConnectHandle(**{device_variable})                              #### Connection function
```

## "Enable mode" example from Netmiko:
```
from netmiko import ConnectHandler
from getpass import getpass

password = getpass()
secret = getpass("Enter secret: ")

cisco1 = {
    "device_type": "cisco_ios",
    "host": "cisco1.lasthop.io",
    "username": "pyclass",
    "password": password,
    "secret": secret,
}

net_connect = ConnectHandler(**cisco1)
# Call 'enable()' method to elevate privileges
net_connect.enable()
print(net_connect.find_prompt())
net_connect.disconnect()
```

- password and secret are assigned to variables which are used in the "cisco1" boilerplate.
- {Connection variable}.enable() = enter enable mode


## "Executing show command" example from Netmiko:
```
from netmiko import ConnectHandler
from getpass import getpass

cisco1 = { 
    "device_type": "cisco_ios",
    "host": "cisco1.lasthop.io",
    "username": "pyclass",
    "password": getpass(),
}

# Show command that we execute.
command = "show ip int brief"

with ConnectHandler(**cisco1) as net_connect:
    output = net_connect.send_command(command)

# Automatically cleans-up the output so that only the show output is returned
print()
print(output)
print()
```

- router commands need to be saved as a string and envelop it in .send_command({variable})
- this is not the command for configuration change, merely for printing show commands.


## "Configuration changes from a file" example from Netmiko:
```
#!/usr/bin/env python
from netmiko import ConnectHandler
from getpass import getpass

device1 = {
    "device_type": "cisco_ios",
    "host": "cisco1.lasthop.io",
    "username": "pyclass",
    "password": getpass(),
}

# File in same directory as script that contains
#
# $ cat config_changes.txt 
# --------------
# logging buffered 100000
# no logging console

cfg_file = "config_changes.txt"
with ConnectHandler(**device1) as net_connect:
    output = net_connect.send_config_from_file(cfg_file)
    # use the appropriate function for your Netmiko platform:
    # commit for Cisco-XR, Juniper-Junos, Palo Alto; save_config for others
    # output += net_connect.commit()
    output += net_connect.save_config()

print()
print(output)
print()
```

- "config_changes.txt" sits within the same directory as the py file
- different methods are used for different models.