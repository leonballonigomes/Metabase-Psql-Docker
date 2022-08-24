# Metabase + PSQL and pgadmin

The idea is to use [Docker](https://www.docker.com/) to facilitate sharing this project. 

1. Build a metabase data viz
2. Build a PSQL db to persist local data
3. Integrade PSQL db with pgadmin to have a friendly interface

# How to use it

    Requirements
    1. Docker
    2. environment variables
    3. RDBMs
    4. (opt) Some knowledge of docker is helpful
    5. (opt) Some knowledge of data viz

## Install docker engine and docker compose
If you don`t have it already, this is a must have for this to work.

[Use this link to install and test it](https://docs.docker.com/engine/install/ubuntu/#installation-methods) -> **Read it carefully before begining**

## set up you environment variables

First thing to do is to make you own .env file. 
This will be used to build the docker compose with the right configuration. 

Therefore, in your root folder or ./ path create a .env file as shown below. 

Create it using terminal nano for Linux kernel
    
![Option 1](images/op1_env.png "Option 1") 

Create it using a desired Graphical user interface (GUI)
    
![Option 2](images/op2_env.png "Option 2") 

Insert the .env following informations

    MB_PORT=3000
    PSQL_SERVER_PORT=5432
    PGADM_SERVER_PORT_IN=5050
    PGADM_SERVER_PORT_OUT=80
    PSQL_USER=postgres
    PSQL_PSWD=postgres
    PGADM_EMAIL=youremail@provider.com
    PGADM_PSWD=postgres

## Using docker-compose 

After everything above, you can setup you own metabase, psql and pgadmin using the following command 

    docker-compose up --build -d 

**Start**

![Start and build dockers](images/build_start_docker.png "Start and build dockers")

**Build**

![Building](images/build_docker.png "Building")

After building and runnin, check it using 

    docker ps 

![Check Dockers](images/running_docker.png "Check it")


## Accessing

### Pgadmin 

enter a browser and insert the following

    0.0.0.0:5050 
    
    (localhost:port given by PGADM_SERVER_PORT_IN)

![entering Pgadmin](images/pgadmin.png "entering Pgadmin")

Use the user and password defined by
PGADM_EMAIL and PGADM_PSWD env variables

![Pg Panel](images/pgadmin_enter.png "Pg Panel")

The entrance panel 

![Pg server](images/pgadmin_server0.png "Pg server")

Register the psql server you want to directly access the data

![Pg server](images/pgadmin_server1.png "Pg server")

Enter the server name 

![Pg server](images/pgadmin_server2.png "Pg server")

define the host or ip and enter the db user. 
Use **psql_db** to define the host (this is the internal psql container **DSN**)

### Metabase

    0.0.0.0:3000 
    
    (localhost:port given by MB_PORT)

![entering metabase](images/metabase.png "entering metabase")

The admin user is created by the first access

### Psql db

This one is a little more complicated. This is because you will need to enter the docker psql_db and run psql command to enter it. You must use a terminal for this.

Get the container ID of the psql_db container:

    docker exec -it [Container_ID] bash

    exec -> execute a direct command in a container
    -i -> have interactivity 
    -t -> with pseudo terminal 
    bash -> enter the bash or container terminal

![entering the docker](images/psql1.png "entering the docker")

After entering the container, execute the command to access the psql application giving the user you defined in PSQL_USER 

    psql -d postgres -U postgres -W 
    
    -d -> specifc database to enter (by standard, every psql starts with a postgres db)
    -U -> User 
    -W -> ask for password

![Accessing psql](images/psql_2.png "Accessing psql")

# General information

## Metabase 

Free dataviz to build and share analytical dashboards

[Metabase](https://www.metabase.com/)

[Metabase on Docker](https://www.metabase.com/docs/latest/installation-and-operation/running-metabase-on-docker.html)

## PSQL

Relational DB useful for storing data as well as serve as building block for analytics or prototyping with SQL

[PostgreSQL](https://www.postgresql.org/)

[PostgreSQL on Docker](https://hub.docker.com/_/postgres/)

## pgadmin

Visual interface integrated with PSQL DB 

[Pgadmin](https://www.pgadmin.org/)

[Pgadmin on Docker](https://hub.docker.com/r/dpage/pgadmin4/)


## References
Useful links for you own project

[1] https://www.automacaodedados.com.br/stories/configurando-metabase-e-postgresql-no-docker/

[2] https://dev.to/stefanopassador/docker-compose-with-python-and-posgresql-33kk

[3] https://www.dabbleofdevops.com/blog/setup-a-postgres-python-docker-dev-stack

[4] https://towardsdatascience.com/a-data-scientist-approach-running-postgres-sql-using-docker-1b978122e5e6

[5] https://dev.to/shree_j/how-to-install-and-run-psql-using-docker-41j2#:~:text=Connecting%20to%20the%20PSQL%20server%20via%20CLI%20%3A&text=Run%20the%20below%20command%20to,p%205432%20%2DU%20postgres%20%2DW

[6] https://towardsdatascience.com/how-to-build-slim-docker-images-fast-ecc246d7f4a7

[7] https://www.postgresqltutorial.com/postgresql-tutorial/import-csv-file-into-posgresql-table/#:~:text=First%2C%20right%2Dclick%20the%20persons,the%20delimiter%20as%20comma%20(%20%2C%20)%3A