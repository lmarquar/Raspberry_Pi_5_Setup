# Raspberry_Pi_5_Setup
this is what i need to get my current config running. It consists of Fritzbox-Network, [RaspberryPi5+NVMe Base Projekt](https://www.etsy.com/de-en/listing/1732482582/), strato-domain, nextcloud-aio. I use it soley as cloud storage. Feel free to grab and interact!

## Os:
- PiOs Lite Download with SSH enabled. Continue once you can acess with SSH from your pc.

## Highsideswitch-fan:
or how to connect a PWM-Fan in a way that it shuts off at night and throttles automatically.
### Parts:
c557b, bc547b, 2 1kΩ-resistors, GeekPi LED-Fan
### circuit:
<img width="440" height="440" alt="image" src="https://github.com/user-attachments/assets/f01977fc-32c6-43df-839f-fcfcaeec8156" />

### define the pins:
to create the on-off switch append to end of `/boot/firmware/config.txt`
```
dtoverlay=gpio-fan  
dtoverlay=gpio-led,gpio=14,label=fan_state  
```

### configure on-off times:
we access cronjob with
`sudo crontab -e`
then paste
```
@reboot echo 1 > /sys/class/leds/fan_state/brightness  
0 6 * * * echo 1 > /sys/class/leds/fan_state/brightness  
0 22 * * * echo 0 > /sys/class/leds/fan_state/brightness  
```
now you can sleep peacefully, even if you have a Raspberry-Server in your room :-).

## Homecloud:
### inital Setup:
- make sure my server has a stable connection with a static local Ip-Adress
- find a Domain. I use Strato currently.
- create a subdomain with prefix: cloud.<domainname>. then find your homenetworks public-ip and set the A record.  
- follow the provided guide with no extra features: https://github.com/nextcloud/all-in-one
- make sure it's acessible from outside your LAN.
- If the public Ip changes regularly, get flexible and set a DynDNS instead of A record:
  - tutorial for my setup: https://www.strato.de/faq/hosting/so-einfach-richten-sie-dyndns-fuer-ihre-domains-ein/
  - otherwise what to do is set domain to dynDns instead of A record. then you need to configure your router to connect your server with the service.
  - Update-Url has this format with all 'x'-words replaced: `https://xDomain.de:xPassword@dyndns.strato.com/nic/update?hostname=xSubDomain.xDomain.de&myip=<ipaddr>,<ip6addr>`
- set automatic updates as found in the nextcloud-aio Docs
- Backup: set up a proper SSD with ext4-fileformat and automounting via /etc/fstab. Then configure borg-backup.
- Email-Server: configured it with my mail account wich was provided with the domain by strato. Good for resetting your password!
- Bonus: get rid of all warnings in your nc-settings.

### be careful with:
- optional NC containers which like to use ports, like to interfere and lead to random bugs.
- saving all your passwords well. I'm still with keepass on usb since 12 year old for all the rudimentary.

### Useful commands:
#### AIO interface/borg password
`sudo docker exec nextcloud-aio-mastercontainer grep password /mnt/docker-aio-config/data/configuration.json`

## additional improvements
- add cronjobs for automatic firmware-updates.

## Sources:
- https://github.com/raspberrypi/firmware/blob/master/boot/overlays/README
- https://github.com/lmarquar/Nextcloud_on_Raspi_commands
- 
