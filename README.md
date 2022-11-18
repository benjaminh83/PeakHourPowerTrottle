# PeakHourPowerTrottle
Will limit the power usage in specified timeframes when grid power is scarce and expensive

Using systemd to control GPUs with nvidia-smi and CPU with cpupower

The following files are provided and needs tweaking. This was tested on Ubuntu 20.04 with a Geforce RTX 3090. Files are to be placed in: 
/etc/systemd/system/

### PWslowdown.service
- Defines the powersetting for the GPU (defined in watts - minimum for 3090 is 100W)
- Sets CPU power goverer to powersave mode (lower clock speed)
- Edit: ´sudo nano /etc/systemd/system/PWslowdown.service` 

### PWslowdown.timer
- Defines the time when the slowdown is triggered. Currently set to trigger at 6AM and 5PM  
- Edit: ´sudo nano /etc/systemd/system/PWslowdown.timer`


### PWspeedup.service
- Defines the powersetting for the GPU (defined in watts - 350W is normal for 3090, but could be 288W for crypto)
- Sets CPU power goverer to performance mode (highest clock speed)
- Edit: ´sudo nano /etc/systemd/system/PWspeedup.service` 

### PWspeedup.timer
- Defines the time when the slowdown is triggered. Currently set to trigger at 10AM and 22PM  
- Edit: ´sudo nano /etc/systemd/system/PWspeedup.timer`


#### Remember to enable all services and timers:
- `sudo systemctl enable PWslowdown.service´
- `sudo systemctl enable PWslowdown.timer´
- `sudo systemctl enable PWspeedup.service´
- `sudo systemctl enable PWspeedup.timer´

#### Remember to always update systemd when changing files:
`sudo systemctl daemon-reload´
