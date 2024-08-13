## PHP & APACHE INSTALLATION ON UBUNTU

1. Install Apache
```
sudo apt-get update
sudo apt-get install apache2
```

2. Start & enable server
```
sudo systemctl start apache2
sudo systemctl enable apache2
```
3. Install PHP
```
sudo apt-get install php libapache2-mod-php php-mysql php-cli php-curl php-mbstring php-xml php-zip
```

Status & version check
```
sudo systemctl status apache2
php -v
```


