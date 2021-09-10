# Second step

## Install pip3 packages for using Docker
```shell
cd ~/edb-assessment/
# you should be in venv
source venv/bin/activate 
cd ~/edb-assessment/django-polls-postgresql
pip3 install docker  # install docker packages for Python
pip3 install docker-compose # install docker-compose
docker-compose version  

## install postgresql 12 docker container
```shell script
docker run -d --name db -e POSTGRES_PASSWORD=password  -v /opt/postgres:/var/lib/postgresql/data \
  -p 5432:5432  systemdevformations/docker-postgres12
```

python3 manage.py makemigrations  # detect changes
python3 manage.py migrations  # create database schema
python3 manage.py createsuperuser # set superuser username and password
python3 manage.py runserver 0:8000 # sanity test and enter your first poll 
# Check in your browser 
http://<ip_address_vm/8000/admin
# user password as keyed createsuperuser 
# add a poll questions
```