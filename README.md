How to setup the CDS development environment using Vagrant and Docker (any Host OS).
===

Vagrant will create a VM and install docker on it. The actual applications run in individual docker containers on this VM.
Example: You develop on Windows (host OS), the VM (guest OS) will run Ubuntu 14.04, and all the docker containers run inside this VM (sandboxed).

1. Install Virtual Box https://www.virtualbox.org/wiki/Downloads
2. Install Vagrant https://www.vagrantup.com/downloads.html
3. Checkout the git repository to your system.
3. Use Vagrant to spin up the apacheds(LDAP) and postgres server (you can also do this using a Vagrant IntelliJ or Eclipse plugin):

```
vagrant up
(optional) vagrant provision # Provision docker containers if for some reason vagrant up did not already do that.
```

The Postgres port 5432 as well as the LDAP port 10389 have been made available on the host OS (Windows).
You can now run Tomcat locally and configure the application in such a way that it connects to the ports on the localhost.

In order to access the guest OS (and inspect the docker containers) SSH to localhost:2222 using the key present in .vagrant/machines/default/virtualbox/private_key.
You might need to convert this key to a Putty key when using putty (use puttygen.exe for this).

