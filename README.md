Bouw een CDS ontwikkel omgeving met Docker
===

Installeer Boot2Docker
Check CDS-BIJ12-Docker git project uit
Mount de root folder van het git project in VirtualBox
In boot2docker mount de folder
``` sh
sudo mkdir /CDS-BIJ12-Docker
sudo mount -t vboxsf -o uid=1000,gid=50 CDS-BIJ12-Docker /CDS-BIJ12-Docker
```

De resources zijn nu beschikbaar onder 

Upgrade docker naar alpha versie zodat docker up commando ondesteund wordt:
zie https://github.com/docker/docker/issues/9459

Tip: run boot2docker vanuit console2/cygwin shell
