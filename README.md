# Raspberry_Pi_5_Setup
this is what i need to get my current config running. Fell free to grab and interact!

# Highsideswitch-fan
## circuit:
<img width="440" height="440" alt="image" src="https://github.com/user-attachments/assets/f01977fc-32c6-43df-839f-fcfcaeec8156" />

## append to end of /boot/firmware/config.txt
dtoverlay=gpio-fan  
dtoverlay=gpio-led,gpio=14,label=fan_state  
## cronjobs
@reboot echo 1 > /sys/class/leds/fan_state/brightness  
0 6 * * * echo 1 > /sys/class/leds/fan_state/brightness  
0 22 * * * echo 0 > /sys/class/leds/fan_state/brightness  

# AIO interface/borg password
sudo docker exec nextcloud-aio-mastercontainer grep password /mnt/docker-aio-config/data/configuration.json

