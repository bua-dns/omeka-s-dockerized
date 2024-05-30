# Create Instances of Omeka S with Docker

**NOTE**  
This is a work in progress and currently has minimal documentation. More information will be added soon.

## Prerequisites

You need a server environment with Docker and Docker Compose installed. Ensure you have the following versions installed on your system:

* Docker: Version 26.0.2 or later
* Docker Compose: Version 2.26.1 or later

Docker Compose v2 has other commands to mange networks than v1. Please verify your setup before proceeding with the deployment. 

To run Omeka S, a (sub)domain is required where the instance can be accessed at the root level (not in a subdirectory).

## Workflow

### 1. Configuration

In the **.env** file, set the following values:

```bash
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
PMA_PORT and OMEKA_S_PORT must be unique on the server where the instance is running. If you plan to run multiple instances of Omeka S in Docker containers, ensure to increment the ports appropriately. A good practice is to name them consistently:

```bash
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

```bash
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

DB_NAME and DB_USERNAME can (but do not have to) remain the same for all containers. The same applies to the passwords in secrets/DB_PASSWORD.txt and secrets/DB_ROOT_PASSWORD.txt.

The docker-compose.yml file is configured automatically on the server (via .env and /secrets), so you do not need to modify this file.

2. Upload
Upload the files and the secrets folder to the directory where Omeka S will run.

This directory should have the following structure:

```bash
my-domain-for-omeka-s
    .env
    .docker-compose.yml
    |secrets
    |_DB_PASSWORD.txt
    |_DB_ROOT_PASSWORD.txt
```
```bash
my-domain-for-omeka-s/
├── .env
├── docker-compose.yml
└── secrets/
    ├── DB_PASSWORD.txt
    └── DB_ROOT_PASSWORD.txt
```
### 3. Run the network

Navigate to this directory on your server and start a new instance of Omeka S with:

```bash
docker compose up -d
```
The feedback in the console should look something like this:

```bash
[+] Running 4/4
 ✔ Network omeka-s-instance-5-omeka-s-network  Created  
 ✔ Container omeka-s-instance-5-omeka-s-db-1   Started  
 ✔ Container omeka-s-instance-5-omeka-s-pma-1  Started  
 ✔ Container omeka-s-instance-5-omeka-s-app-1  Started     
```
Open the URL of your instance in the browser. Omeka will redirect to the installation page where you can create the admin credentials, provide a name, and set some basic configuration values.

The volumes for your instance have been automatically created at /var/lib/docker/volumes/<name from COMPOSE_PROJECT_NAME in .env>.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.



