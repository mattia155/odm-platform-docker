# Open Data Mesh: Platform and Meta-service #

Open Data Mesh Platform is a platform that manages the full lifecycle of a data product from deployment to retirement. It use the [Data Product Descriptor Specification](https://dpds.opendatamesh.org/) to to create, deploy and operate data product containers in a mesh architecture. 


This repository contains a dockerized version of:

* the services exposed by the platform product plane
* the Meta service adapter for [blindata.io](https://blindata.io/)
* the Policy service based on [OPA](https://www.openpolicyagent.org/) 


The original projects repositories are:

* [Platform](https://github.com/opendatamesh-initiative/odm-platform-pp-services)
* [Meta-service](https://github.com/opendatamesh-initiative/odm-platform-up-services-meta-blindata)
* [Policy service](https://github.com/opendatamesh-initiative/odm-platform-up-services-policy-opa)

# Run Project #

## Prerequisites ##
* Docker
* docker-compose

## Run with docker-compose ##
### Clone repository
Clone the repository and move it to the project root folder

```bash
git clone https://github.com/opendatamesh-initiative/odm-platform-docker.git
cd odm-platform-docker
```

### Build image
Build the docker-compose images of the applications and a default PostgreSQL DB.

Before building it, create a `.env` file in the root directory of the project similar to the following one:
```.dotenv
DATABASE_PORT=5433
DATABASE_NAME=odm-demo-db
DATABASE_USERNAME=usr
DATABASE_PASSWORD=pwd

SKIP_POLICYSERVICE=false
OPA_PORT=8181
POLICYSERVICE_SPRING_PORT=9001

SKIP_METASERVICE=false
BLINDATA_URL=<blindata-url>
BLINDATA_USER=<blindata-user>
BLINDATA_PWD=<blindata-pwd>
BLINDATA_TENANT=<blindata-tenant-uuid>
BLINDATA_ROLE=<blindata-role-uuid>
METASERVICE_SPRING_PORT=9002

PLATFORM_SPRING_PORT=8001
```

Then, build the docker-compose file:
```bash
docker-compose build
```

*Note: the Blindata parameters have the following meaning:*
```yaml
BLINDATA_URL: the url where Blindata application is reachable
BLINDATA_USER: the username used to log in Blindata
BLINDATA_PWD: the password to connect in Blindata
BLINDATA_TENANT: the UUID of the tenant where you have to operate
BLINDATA_ROLE: A possible role identifier (UUID). You need this identifier to create or update responsibilities in Blindata
```

### Run application
Run the docker-compose images.
```bash
docker-compose up
```

*_NOTE: it's possible to run only a subset of all the available services with the command reported below; but in that scenario you need to be careful of dependencies within modules. Every module require a DB and the policyservice require an OPA server._

```bash
docker-compose up servicename1 servicename2 ...
```

_For example, the following command will create only a PostgreSQL DB, an OPA Server and the PolicyService Server_

```bash
docker-compose up PostgreSQL opa policyservice
```

### Stop application
Stop the docker-compose images
```bash
docker-compose down
```
To restart a stopped application execute the following commands:

```bash
docker-compose up
```

To rebuild it from scratch execute the following commands :
```bash
docker-compose build --no-cache
```

# Test it

## REST services

You can invoke REST endpoints of the platform module through *OpenAPI UI* available at the following url:

* [http://localhost:8001/api/v1/pp/swagger-ui/index.html](http://localhost:8001/api/v1/pp/swagger-ui/index.html)

The Meta-service and the Policy service will be auto invoked from the platform to register the notification events or handle policies. 
You can check notifications and policies in the shared PostgreSQL DB.

## Database
The application run using a dockerized postgresql running on *localhost* on given port (from *.env* file).
You can use your favourite sql client providing the proper connection parameters to check it.
