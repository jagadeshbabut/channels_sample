# Setup Django 2.x, Channels 2.x, Nginx and uWSGI on a Ubuntu 16.04 server for Python 3.5 applications

#### REQUIRED PACKAGES/DEPENDENCIES
```
sudo apt-get update && sudo apt-get -y upgrade
sudo apt-get -y install python3-pip python3-setuptools libpq-dev build-essential vim uwsgi uwsgi-plugin-python3
sudo add-apt-repository ppa:nginx/stable
sudo apt-get update
sudo apt-get install nginx
sudo apt-get install redis-server
```

### SETUP

##### Django Setup

###### Virtualenvwrapper Setup
```
sudo pip3 install virtualenvwrapper
echo "export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3" >> ~/.bashrc
echo "export WORKON_HOME=$HOME/.virtualenvs" >> ~/.bashrc
echo "export PROJECT_HOME=$HOME/Devel" >> ~/.bashrc
echo "source /usr/local/bin/virtualenvwrapper.sh" >> ~/.bashrc
source ~/.bashrc
```

###### Application Setup
```
mkvirtualenv --python=/usr/bin/python3 channels_sample
git clone git@github.com:jagadeshbabut/channels_sample.git / git clone https://github.com/jagadeshbabut/channels_sample.git
cd channels_sample/
pip install -r requirements.txt
python manage.py migrate
python manage.py collectstatic --no-input
python manage.py createsuperuser
```


#### NGNIX
```
sudo rm /etc/nginx/sites-enabled/default
sudo vim /etc/nginx/sites-available/nginx.conf
```
Copy the [nginx.conf](./configs/nginx.conf) file
```
sudo ln -s /etc/nginx/sites-available/nginx.conf /etc/nginx/sites-enabled/nginx.conf
sudo service nginx configtest
sudo service nginx restart
sudo service nginx status
```

#### UWSGI
```
sudo vim /etc/uwsgi/apps-available/uwsgi.ini
```
Copy the [uwsgi.ini](./configs/uwsgi.ini) file

```
sudo ln -s /etc/uwsgi/apps-available/uwsgi.ini /etc/uwsgi/apps-enabled/uwsgi.ini
sudo service uwsgi restart
sudo service uwsgi status
```

#### DAPHNE
```
sudo vim /lib/systemd/system/daphne.service
```
Copy the [daphne.service](./configs/daphne.service) file

```
sudo systemctl enable daphne.service
sudo systemctl start daphne
sudo systemctl status daphne
```


##### Restart Services
```
sudo systemctl restart daphne
sudo service uwsgi restart
sudo service nginx restart
```
exit