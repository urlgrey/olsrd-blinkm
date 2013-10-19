olsrd-blinkm
============

I've been interested in have some visual indication of mesh connection status since I began work on the HSMM-Pi project (https://github.com/urlgrey/hsmm-pi).  The project turns a Raspberry Pi into a wireless mesh node, using the OLSRD software package to configure routing among the network nodes.  It's not feasible to have a computer display, or even a compact LCD, connected to the Raspberry Pi nodes in a large network.  I wanted an LED that could provide a quick visual cue of whether a mesh node is up and part of the network.

I decided to use the BlinkM module because I had one left over from a past project, and I like the simplicity with which you can change the color.  I used the ```overo-blinkm``` C program to communicate with the BlinkM module over the I2C bus.  The 'blinkm' binary that is produced by the 'overo-blinkm' project takes straightforward commandline arguments that trivialize the setting of the LED color and behavior.

I wrote the ```mesh_neighbors_led``` PHP script in this project to automate the task of querying OLSRD for mesh neighbors and updating the LED color to indicate the number of connected neighbors.  The script sets the LED color using the following rules:

1. Red = no reachable neighbors
1. Blue = 1 reachable neighbor
1. Green = 2 or more reachable neighbors

Installation
============

First you must compile and install hte overo-blinkm project (https://github.com/scottellis/overo-blinkm).  Place the resulting ```blinkm``` binary in /usr/local/bin:

```
sudo cp blinkm /usr/local/bin
```

Next, install the ```mesh_neighbors_led``` script in /usr/local/bin and set it to run each minute via CRON:

```
sudo cp mesh_neighbors_led /usr/local/bin
sudo chmod +x /usr/local/bin/mesh_neighbors_led
sudo echo "* *  * * *     root   /usr/local/bin/mesh_neighbors_led" > /etc/cron.d/blinkm_led
sudo chmod +x /etc/cron.d/blinkm_led
```

That's it!  You should begin to see the LED change color to match your network connect status.

