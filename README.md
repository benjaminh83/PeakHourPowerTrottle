# PeakHourPowerTrottle
Will limit the power usage in specified timeframes when grid power is scarce and expensive.


This is highly relevant for areas with advanced grid metering and hourly energy price models. Also this will lead to greener energy consumption by limiting consumption in peak hours which usually require more black energy mix. 


Example of ideal outcome from throttling power usage (show with hashrate from ETC mining, but relevant for all workloads, as they are not being stopped from processing, but just slowed down due to restrictions on power usage in the GPU and CPU):

![image](https://user-images.githubusercontent.com/14029124/202670955-c94a163d-0917-4495-9dee-03eae767f6d6.png)


## This approach is using systemd to control GPUs with nvidia-smi and CPU with cpupower

The following files are provided and needs tweaking. This was tested on Ubuntu 20.04 with a Geforce RTX 3090. Files are to be placed in: 
`/etc/systemd/system/`

### PWslowdown.service
- Defines the powersetting for the GPU (defined in watts - minimum for 3090 is 100W)
- Sets CPU power goverer to powersave mode (lower clock speed)
- Edit: `sudo nano /etc/systemd/system/PWslowdown.service` 

### PWslowdown.timer
- Defines the time when the slowdown is triggered. Currently set to trigger at 6AM and 5PM  
- Edit: `sudo nano /etc/systemd/system/PWslowdown.timer`


### PWspeedup.service
- Defines the powersetting for the GPU (defined in watts - 350W is normal for 3090, but could be 288W for crypto)
- Sets CPU power goverer to performance mode (highest clock speed)
- Edit: `sudo nano /etc/systemd/system/PWspeedup.service` 

### PWspeedup.timer
- Defines the time when the slowdown is triggered. Currently set to trigger at 10AM and 22PM  
- Edit: `sudo nano /etc/systemd/system/PWspeedup.timer`


#### Remember to enable all services and timers:
`sudo systemctl enable PWslowdown.service`


`sudo systemctl enable PWslowdown.timer`


`sudo systemctl enable PWspeedup.service`


`sudo systemctl enable PWspeedup.timer`

#### Remember to always update systemd when changing files:
`sudo systemctl daemon-reload`

#### Prerequisites / Troubleshooting
Its probably a good idea to manually check that you have the correct packages installed, so that you are able to execute the commands like: 


`sudo /usr/bin/nvidia-smi -pl 200`


`sudo cpupower frequency-set --governor performance`
