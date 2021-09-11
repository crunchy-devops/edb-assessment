# Second step

## Install pip3 packages for using Docker
```shell
cd ~/edb-assessment/
# you should be in venv
source venv/bin/activate 
cd ~/edb-assessment/django-polls-postgresql
pip3 install psycopg2-binary  # install postgresql interface for python
pip3 install docker  # install docker packages for Python
pip3 install docker-compose # install docker-compose
docker-compose version  

## install postgresql 12 docker container
```shell script
docker run -d --name db -e POSTGRES_PASSWORD=password  -v /opt/postgres:/var/lib/postgresql/data \
  -p 5432:5432  systemdevformations/docker-postgres12
```
## Run django app
```shell
python3 manage.py makemigrations  # detect changes
python3 manage.py migrate # create database schema
python3 manage.py createsuperuser # set superuser username and password
python3 manage.py runserver 0:8000 # sanity test and enter your first poll 
# Check in your browser 
http://<ip_address_vm/8000/admin
#user password as keyed createsuperuser 
# add a poll question
```
## Check the database using pgadmin 
Launch PgAdmin 4 container connected to the Postgresql database
```shell
docker run -d --name pgadmin -p 20100:80 --link db:postgres -e PGADMIN_DEFAULT_EMAIL=edb@test.com \
-e PGADMIN_DEFAULT_PASSWORD=p4ssw0rd dpage/pgadmin4
```
Connect as edb@test.com/p4ssw0rd to pgadmin4 
Add a server named poll, the host name is the postgresql container name db, 5432 , postgres , password  
Check the table polls_choice

## Create the dockerfile 
your prompt set as venv environment   
Type ```pip3 freeze```  
Copy paste the version of django and psycopg2-binary in requirements.txt file    
see Dockerfile 

## the big question , should you use a docker-compose or ansible yaml file ? 
start db container using ansible  
```shell
cd ~/edb-assessment/  # set to the porject diretory
source venv/bin/activate # prompt to virtualenv
ansible-playbook -i inventory django-polls-postgresql/install-postgresql.yml
docker ps # Check if the db container is up and running
```
start web container using ansible
```shell
ansible-playbook -i inventory django-polls-postgresql/install-django-app.yml
docker ps # Check if the  web container is up and running
```