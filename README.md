# Create Instances of Omeka S with Docker

**NOTE**
This is a work in progress that is at the moment very sparcely documented. More to come soon.

## Prerequisites

You need a server environment with Docker and Docker Compose installed. For running Omeka S, there must be a (sub)domain where the instance can be reached at root level (not in a subdirectory).

## Workflow

### 1. Configuration

In **.env** set the following values

```shell
### Docker
COMPOSE_PROJECT_NAME=a-name-for-your-project
### DB
DB_NAME=omekas
DB_USERNAME=omekas
### Omeka
OMEKA_S_VERSION=4.1.0
### PORTS
PMA_PORT=819X
OMEKA_S_PORT=811X
```
PMA_PORT and OMEKA_S_PORT **must** be unique on the server the instance is running on. So, if you plan to run multiple instances of Omeka S in Docker containers, make shure to count the ports up properly. A good practice is to name consistantly:

```shell
### Docker
COMPOSE_PROJECT_NAME=omeks-s-instance-1
### DB
DB_NAME=omekas
DB_USERNAME=omekas
### Omeka
OMEKA_S_VERSION=4.1.0
### PORTS
PMA_PORT=8191
OMEKA_S_PORT=8111
```

```shell
### Docker
COMPOSE_PROJECT_NAME=omeks-s-instance-2
### DB
DB_NAME=omekas
DB_USERNAME=omekas
### Omeka
OMEKA_S_VERSION=4.1.0
### PORTS
PMA_PORT=8192
OMEKA_S_PORT=8112
```
etc.

DB_NAME and DB_USERNAME can (but must not) stay the same for all containers. The same applies to the passwords in secrets/DB_PASSWORD.txt and secrets/DB_ROOT_PASSWORD.txt.

docker-compose.yml is configured automatically on the server (via .env and /secrets), so you don't have to touch this file.

### 2. Upload

Upload the files and the secrets folder to the directory Omeka S shall run in.

This directory should have the following setup:

```shell
my-domain-for-omeka-s
    .env
    .docker-compose.yml
    /secrets
    |_DB_PASSWORD
    |_DB_ROOT_PASSWORD
```
### 3. Run the network

Navigate into this directory on your server and start a new instance of Omeka S with

```shell
docker compose up -d
```
The feedback in the console should look something like:

```shell
[+] Running 4/4
 ✔ Network omeka-s-instance-5-omeka-s-network  Created  
 ✔ Container omeka-s-instance-5-omeka-s-db-1   Started  
 ✔ Container omeka-s-instance-5-omeka-s-pma-1  Started  
 ✔ Container omeka-s-instance-5-omeka-s-app-1  Started     
```
Open the url of your instance in the browser. Omeka redirects to the install page where you can create the admin credentials, provide a name and set some basic configuration values.

The volumes for your instance have been automatically created at /var/lib/docker/volumes/<name from COMPOSE_PROJECT_NAME in .env>.

