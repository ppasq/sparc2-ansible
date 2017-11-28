# sparc2-ansible
Ansible Project for Sparc 2.x

# Installation for Development

On the host machine, you'll need Ansible installed.

## Get Ansible

To get ansible follow the relevant section below.  Also see http://docs.ansible.com/ansible/intro_installation.html#getting-ansible for more information.

### Mac OS X

```
sudo easy_install pip
sudo pip install ansible==2.0.0.1
```

## Create your workspace

Create a workspace directory (e.g. wfp) and clone all the required repos.

```
# Create a folder
mkdir wfp
cd wfp

# Clone the repos
git clone https://github.com/wfp-ose/geodash-base.git geodash-base.git
git clone https://github.com/wfp-ose/geodash-framework-django.git geodash-framework-django.git
git clone https://github.com/wfp-ose/sparc2-core.js.git sparc2-core.js.git
git clone https://github.com/wfp-ose/sparc2-pipeline.git sparc2-pipeline.git
git clone https://github.com/wfp-ose/sparc2-plugin-calendar.git sparc2-plugin-calendar.git
git clone https://github.com/wfp-ose/sparc2-plugin-sidebar.git sparc2-plugin-sidebar.git
git clone https://github.com/wfp-ose/sparc2.git sparc2.git
git clone https://github.com/karakostis/sparc2-ansible.git sparc2-ansible.git
```

## Vagrant

Change directory in sparc2-ansible.git and run vagrant.

```
cd sparc2-ansible.git
vagrant up
```

## Fix installed packages

If you have an error related to GDAL, access the virtual machine and install GDAL in sparc2 virtualenv.

```
vagrant ssh
workon sparc2

/home/vagrant/.venvs/sparc2/bin/pip install --no-install GDAL==2.0.0
cd /home/vagrant/.venvs/sparc2/build/GDAL
/home/vagrant/.venvs/sparc2/bin/python setup.py build_ext --include-dirs=/usr/include/gdal
/home/vagrant/.venvs/sparc2/bin/pip install --no-download GDAL==2.0.0
```

Install in your virtualenv django-leaflet and the required Django version.

```
pip install django-leaflet
pip install django==1.9.6
pip install six==1.10.0
```

Change the PostgreSQL configuration.

```
sudo nano /etc/postgresql/9.3/main/pg_hba.conf

# Fix pg_hba.conf
# --------------------------------------------------------------------------------
...
# Database administrative login by Unix domain socket
local   all             postgres                                md5

# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     md5
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
# IPv6 local connections:
host    all             all             ::1/128                 md5
# Allow replication connections from localhost, by a user with the
# replication privilege.
#local   replication     postgres                                peer
#host    replication     postgres        127.0.0.1/32            md5
#host    replication     postgres        ::1/128                 md5
local   sparc2          sparc2                                  md5
host    sparc2          sparc2           127.0.0.1/32           md5
host    sparc2          sparc2           10.0.2.2/32            md5
host    all             all              all                    password
# --------------------------------------------------------------------------------
```

Then restart PostgreSQL.

```
sudo service postgresql restart
```

Restore the database

```
sudo su postgres
psql -d sparc2 -f path/to/sparc2_backup.sql
```

Modify the gunicorn path inside the supervisord.conf

```
nano /home/vagrant/sparc2.git/supervisord.conf
```
Verify that all supervisor processes are stopped

```
supervisorctl status
```

In case stop or kill all processes

```
supervisorctl stop sparc2:*
ps -ef | grep supervisord
# Change 1234 with the pid
kill -s SIGTERM 1234
```

Install GeoDash framework

```
/home/vagrant/.venvs/sparc2/bin/pip install -e git+https://github.com/wfp-ose/geodash-framework-django.git@sparc2#egg=geodash-framework-django
```

Run collectstatic to move Django static files

```
sudo /home/vagrant/.venvs/sparc2/bin/python manage.py collectstatic --noinput -i gulpfile.js -i package.json -i temp -i node_modules
```

Start supervisor and then sparc2

```
supervisord -c /home/vagrant/sparc2.git/supervisord.conf
python manage.py runserver 0.0.0.0:8000
```


# Usage

## Vagrant

```
cd sparc2-ansible.git
vagrant up
vagrant ssh
workon sparc2
supervisord -c /home/vagrant/sparc2.git/supervisord.conf
cd /home/vagrant/sparc2.git/
python manage.py runserver 0.0.0.0:8000
```
