### Install mysql-server
```
sudo apt install mysql-server
```
### Login with sudo
```
sudo mysql -u root 
```
### Set Password
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_new_password';
```
### FLUSH PRIVILEGES
```
FLUSH PRIVILEGES;
```
