# Target Display Mode script

## How it works (currently)

### iMac
I made a shell script `~/bin/tdm.ssh`:

	osascript -e 'tell application "System Events" to key code 144 using command down'
	
It invokes an AppleScript which presses **CMD+F2**.
I also added laptop's RSA to the authorized_keys list.

 
### Laptop
I connect to the iMac (hardwired address) and run that shell script:

	ssh -t user@imac.local "sh ~/bin/tdm.sh"
	
## Goals

- To recognize and locate the iMac dynamically ("Mac connected via Thunderbolt")
- To execute the command directly, without the shell script on the iMac
- To wake the iMac remotely


## Finding the IP
With this command, you can find the IP of whatever's connected as bridge0:

	arp -a | grep "bridge0" | grep -oE "(\d{1,3}\.){3}\d{1,3}" | uniq
	
However, there are two issues with this approach:

- It doesn't uniquely identify the device connected to the laptop, as `bridge0` repeats. You should instead somehow locate the device with the laptop's Thunderbolt's MAC address, and then locate whatever device is connected to it (assuming you only have one device connected)
- Most importantly, the laptop doesn't immediately connect to the iMac when you plug in the Thunderbolt.  
If you connect to it even once, now it shows up on `arp -a` but not before that. So either there's a way to make the Mac automatically connect, or there has to be another way to locate the machine.