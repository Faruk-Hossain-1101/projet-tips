## Django Deployement

### Make new user and install all the packeges
```
sudo apt-get update
sudo apt-get install python3 python3-dev python3-venv -y
```

### Install mysql packeges
```
sudo apt-get install pkg-config
sudo apt-get install default-libmysqlclient-dev build-essential
```
### Clone the repo
```
git clone ___
```

### Make venv and activate
```
python3 -m venv <venv_name>
source <venv_name>/bin/activate
```

### Install requirements
```
pip install -r requirements.txt
```

### Migrate the app
```
python manage.py migrate
```

### Collect static files 
```
python manage.py collectstatic
```

### Run the server
```
python manage.py runserver 0.0.0.0:8000
```

### If not loading then allow the ufw
```
sudo ufw allow 8000
```
## Basic test complete

## Using NGINX and GUNICORN

### Install gunicorn
```
pip install gunicorn
```
### Run With gunicorn
```
gunicorn --workers 3 --bind 0.0.0.0:8000 test_django.wsgi:application
```

### Make socket file for gunicorn
```
sudo nano /etc/systemd/system/gunicorn.socket
```
```
# File content

[Unit]
Description=Gunicorn Socket

[Socket]
ListenStream=/run/gunicorn.sock

[Install]
WantedBy=sockets.target
```

### Make service file for gunicorn
```
sudo nano /etc/systemd/system/gunicorn.service
```
```
# File content 

[Unit]
Description=Gunicorn Daemon
Requires=gunicorn.socket
After=network.target

[Service]
User=your_username  # Change this to the user who owns the Django project
Group=www-data      # Change this to the group that Nginx runs as
WorkingDirectory=/path/to/your/django/project
ExecStart=/path/to/your/virtualenv/bin/gunicorn \
          --workers 3 \
          --bind unix:/run/gunicorn.sock \
          your_project_name.wsgi:application

[Install]
WantedBy=multi-user.target
```

###  Enable and start the socket and service
```
sudo systemctl enable gunicorn.socket
sudo systemctl enable gunicorn.service

sudo systemctl start gunicorn.socket
sudo systemctl start gunicorn.service
```

### Check the status
```
sudo systemctl status gunicorn.socket
sudo systemctl status gunicorn.service
```

### For test 
```
curl --unix-socket /run/gunicorn.sock http://localhost
```
### Disable ufw
```
sudo ufw disable
```

### Install nginx
```
sudo apt-get install nginx
```

### Ngnix 
```
sudo nano /etc/nginx/sites-available/your_project_name
```
```
server {
        listen 80;
        server_name _;


        location /static {
                autoindex on;
                alias /static/file/path;
        }

        location / {
                proxy_pass http://unix:/run/gunicorn.sock;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
}

```
### enable
```
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```

### Nginx restart
```
sudo systemctl restart nginx
```


