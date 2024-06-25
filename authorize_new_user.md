## Authorizations create for new user in ubuntu

### Login to server with username and password

### Create a ssh key
```
ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_test 
```
### Get the ssh pub key and copy the content
```
sudo cat  .ssh/id_test.pub
```
### Create a new authorization file 
```
sudo nano .ssh/authorized_keys
```
### Get the private key and copy for next step
```
sudo cat  .ssh/id_test
```

### Past the private keys and exit
