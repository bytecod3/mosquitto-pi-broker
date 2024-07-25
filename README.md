## MOSQUITTO broker setup on Raspberry Pi

This procedure outlines setting up mosquitto broker on a Raspberry pi

### Tested on
1. Pi Model		: Raspberry Pi Model 4 B
2. OS			: Raspbian GNU/Linux 11 (bullseye)
3. Architecture	: arm64

### Installing mosquitto 
``` sudo apt update & sudo apt upgrade```
```sudo apt install -y mosquitto mosquitto-clients```

This command will install the Mosquitto broker and the client applications, like the subscriber and publisher.
Once the installation is complete, it will be possible to proceed with the configuration.

### Test the installation
```mosquitto -v```

On some Raspberry Pi's, mosquitto broker is ran as a background service by default do you 
might get an error saying ```Address already in use```. 
This is okay. 

confirm mosquitto and check all running ports 
``` sudo ss -ltn ```  

mosquitto should be running on 127.0.0.1:1883

### Testing setup and connection
1. open 2 separate consoles 
2. on console 1, subscribe to a test topic
```mosquitto_sub -h localhost -t /test/topic```
3. on console 2, publish a message to a test topic
```mosquitto_pub -h localhost -t /test/topic "hello" ```

A message "hello" should appear on the subscribing console.

### Enabling remote broker access
By default, our Mosquitto broker only accepts connection requests from the localhost and not from other devices on the network.
To enable the remote access function, we must add the following configurations to the Mosquitto configuration file:

```
listener 1883
allow_anonymous true
```

open ```mosquitto.conf``` with nano file using this command:
```$sudo nano /etc/mosquitto/mosquitto.conf```

Add the above 2 lines at the end of the file 

Then restart the mosquitto service 
```sudo service mosquitto restart```

Now that remote access is active, the broker will also be accessible from other remote devices. 

### getting the broker address  
1. run ```ifconfig```
2. get the ip address of the raspberry pi
3. use this when subscribing to a topic

