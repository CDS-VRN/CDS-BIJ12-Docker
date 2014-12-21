Bouw een CDS ontwikkel omgeving (apacheds+postgres) met Docker, gebaseerd op Windows 7 en Ubuntu 14.04.1 LTS
===

Installeer Virtual Box
Maak een Ubuntu server VM
Stel port forwarding in voor 22, 5432 en 10389
Gebruik cygwin voor ssh sessie ```ssh  169.254.122.86```
Installeer Guest Editions (zie ook http://askubuntu.com/questions/456400/why-cant-i-access-a-shared-folder-from-within-my-virtualbox-machine)

Installeer Docker (zie https://docs.docker.com/installation/ubuntulinux/ -> Docker maintained package installation)
Installeer Fig
Fix probleem met Docker host, zie https://github.com/docker/fig/issues/88

--> Zie snapshot
Check CDS-BIJ12-Docker git project uit in windows
Mount de root folder van het git project in VirtualBox
In ubuntu mount de folder naar /CDS-BIJ12-Docker
``` sh
sudo mkdir /CDS-BIJ12-Docker
sudo mount -t vboxsf -o uid=1000,gid=50 CDS-BIJ12-Docker /CDS-BIJ12-Docker
```

Zorg dat je op het Geodan netwerk zit (VPN of op lokatie)
Start de containers met fig
```
cd /CDS-BIJ12-Docker
fig -p vrn up
```

