## New User Creation Ubuntu

#### Create User
```
sudo adduser <usrname>
```
#### Add user to access sudo 
```
sudo usermod -aG sudo <usrname>
```
### Setup the firwall

#### Check list of firewall
```
sudo ufw app list
```
#### Allow permission to access open ssh
```
sudo ufw allow OpenSSH
```
#### Enable SSH
```
sudo ufw enable
```
#### Check status
```
sudo ufw status
```

### Login to new user
```
su <usename>
```
#### Udate packages
```
sudo apt update
```
#### If getting error :
```
sudo nano /etc/environment
```
#### Add the line
```
export GNUTLS_CPUID_OVERRIDE=0x1
```








