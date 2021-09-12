# Second step

## Install pip3 packages for using Docker in Ansible
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
(Note: You may hit some issue when using django polls directly, db ip address not found
 hit docker network inspect bridge for getting the IP address of db container)
python3 manage.py createsuperuser # set superuser username and password
python3 manage.py runserver 0:8000 # sanity test and enter your first poll 
# Check in your browser 
http://<ip_address_vm/8000/admin
#user password as keyed createsuperuser 
# add a poll question
```

## Load some data 
In goland, use the database tab on left side.   
Set a postgresql database connection  
Right click on the table polls_choice and select Import Data from  File  
Select the file to load in the directory data postgres_public_polls_choice.csv
Do the same for the table polls_question 

## Check the database using pgadmin 
Launch PgAdmin 4 container connected to the Postgresql database
```shell
docker run -d --name pgadmin -p 20100:80 --link db:postgres -e PGADMIN_DEFAULT_EMAIL=edb@test.com \
-e PGADMIN_DEFAULT_PASSWORD=p4ssw0rd dpage/pgadmin4
```
Connect as edb@test.com/p4ssw0rd to pgadmin4 
Add a server named poll, the host name is the postgresql container name db, 5432 , postgres , password  
Check the table polls_choice  

## Create the dockerfile for django app
your prompt set as venv environment   
Type ```pip3 freeze```  
Copy and paste django and psycopg2-binary version in requirements.txt file    
see Dockerfile file

## Build and start Django polls
```shell
cd ~/edb-assessment/django-polls-postgresql
docker build -t poll . 
docker run -d --name web --link db:ex -p 8000:8000 poll python3 manage.py runserver 0:8000
# Check
```

## The big question: Should you use a docker-compose or ansible yaml file ? 
Ummh!!!, start db container using ansible  
```shell
cd ~/edb-assessment/  # set to the porject diretory
source venv/bin/activate # prompt to virtualenv
ansible-playbook -i inventory django-polls-postgresql/install-postgresql.yml
(Note: On my desktop linux box, not on a VM , I got an issue with docker lib not found  
that's is a mess with the virtualenv and python interperter with ansible, that should be investigated')
docker ps # Check if the db container is up and running
docker logs db
```
start web container using ansible
```shell
ansible-playbook -i inventory django-polls-postgresql/install-django-app.yml
docker ps # Check if the  web container is up and running
docker logs web 
```