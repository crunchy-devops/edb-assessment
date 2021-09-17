# First step

## Django set up on virtualenv on Ubuntu 20.04 using sqlite3
```shell
cd ~/edb-assessment/django-polls
# you should be in venv
source ../venv/bin/activate 
pip3 install django  # install django 
python3 -m django --version  # check version number
python3 manage.py migrate  # create database schema
python3 manage.py createsuperuser # set superuser username and password
python3 manage.py runserver 0:8000 # sanity test and enter your first poll 
# Check in your browser 
http://<ip_address_vm/8000/admin
# user password as keyed createsuperuser 
# add a poll questions
```