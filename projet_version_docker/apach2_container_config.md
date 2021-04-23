
```bash
apt update
apt install -y openssh-server nano
nano /etc/ssh/sshd_config # set : PermitRootLogin yes
service ssh restart
passwd root # set a password
```

OR 

```bash
apt update
apt install -y openssh-server
adduser admin # create a new user 
chmod o+w htdocs
```