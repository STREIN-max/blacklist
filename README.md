# Czech blacklist info page

This is simple Czech Web BLACKLIST info page with simple API to retrieve blacklist items written in Python Flask

Original blacklist is in [PDF](http://www.mfcr.cz/assets/cs/media/Zverejnovane-udaje-ze-Seznamu-nepovolenych-internetovych-her_v1.pdf) (Yes in PDF, i'm not joking) on [MFCR](http://www.mfcr.cz/cs/soukromy-sektor/hazardni-hry/seznam-nepovolenych-internetovych-her/2017/zverejnovane-udaje-ze-seznamu-nepovoleny-29270) web page

# Features

* JSON API
* List of blacklist items with blocking info, URL, page thumbnail and paging
* Administration of blacklist
* Your provider blocking info
* OSS :)

# TODO

* List of blocking providers
* Blocking vs not blocking requests stats

# Installation

```bash
git clone https://github.com/Salamek/blacklist.git
cd blacklist
pip install -r requirements.txt
export FLASK_APP=run.py
flask initdb
flask default_data
bower install
flask run
```

default username and password are admin:admin

application uses Celery for background tasks, so you should run run_worker.sh & run_beat.sh if you want them working :)

```
./run_worker.sh
./run_beat.sh
```

# UWSGI

```ini
[uwsgi]
uid = www-data
master = true
chdir = /path/to/blacklist_dir
socket = server.sock
module = run
callable = app
plugins = python
buffer-size = 32768
```

# Nginx

```
server {
        listen 80;
        listen [::]:80;
        server_name blacklist.example.com;

        root /var/www/blacklist.example.com;
        access_log /var/www/blacklist.example.com/logs/access.log;
        error_log /var/www/blacklist.example.com/logs/error.log;

        location / {
                uwsgi_pass unix:///var/www/blacklist.example.com/server.sock;
                include uwsgi_params;
        }
}

```
