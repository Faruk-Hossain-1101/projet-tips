## SETUP AWS WITH GUNICORN-NGINX


### LOGIN THROUGH SSH

```
chmod 400 your-private-key.pem
ssh -i your-key-pair.pem ec2-user@your-ec2-instance-ip
```
### UPDATE AND INSTALL PACKEGES
```
sudo apt-get update
sudo apt-get install -y python3 python3-pip python3-venv
```
### SETUP GITHUB IF A PRIVATE REPO
```
ssh-keygen
cat ~/.ssh/id_rsa.pub
```
### GIVE USER PERMISSION IF REQUIRED TO WORKING FOLDER
```
sudo chown user:group file_or_directory
```
### CLONE THE REPO
```
git clone ---your-repo---
cd /to/repo/
```
### CREATE VIRTUAL ENV AND ACTIVATE
```
python3 -m venv venv
source venv/bin/activate
```
### INSTALL REQUIREMENTS
```
pip install -r requirements.txt
```
### IF YOU HAVE MYSQL IN USE THEN INSTALL BELOW PACKEGES
```
sudo apt-get install pkg-config
sudo apt-get install libmysqlclient-dev
```
### INSTALL NGINX AND SETUP
```
sudo apt-get install nginx
sudo nano /etc/nginx/sites-available/flask_app <!-- To create a ngnix config file -->
sudo ln -s /etc/nginx/sites-available/flask_app /etc/nginx/sites-enabled/  <!-- To create a symbolic link -->
```
### FILE CONTENT
```
server {
    listen 80;
    server_name your-domain-or-ip;

    location / {
        proxy_pass http://127.0.0.1:5000;
        include /etc/nginx/proxy_params;
        proxy_redirect off;
    }
}
```
### INSTALL AND SETUP GUNICORN
```
pip install gunicorn
gunicorn -w 3 -b 127.0.0.1:5000 your_app:app <!-- To check app running or not -->
sudo nano /etc/systemd/system/your-flask-app.service <!-- Service file for running app -->
```
### FILE CONTENT
```
[Unit]
Description=Gunicorn instance to serve your Flask app
After=network.target

[Service]
User=ec2-user
Group=ec2-user
WorkingDirectory=/path/to/your/flask/app
ExecStart=/path/to/venv/bin/gunicorn -w 3 -b 127.0.0.1:5000 your_app:app

[Install]
WantedBy=multi-user.target
```
### START AND ENABLE SERVICE
```
sudo systemctl start your-flask-app
sudo systemctl enable your-flask-app
```
### TO RESTART NGNIX AND GUNICORN
```
sudo systemctl restart your-flask-app
sudo systemctl restart nginx
```
