# Open Data Mesh: Platform and Meta-service #

Open Data Mesh Platform is a platform that manages the full lifecycle of a data product from deployment to retirement. It use the [Data Product Descriptor Specification](https://dpds.opendatamesh.org/) to to create, deploy and operate data product containers in a mesh architecture. 


This repository contains a dockerzied version of:

* the services exposed by the platform product plane
* the Meta service adapter for [blindata.io](https://blindata.io/)


The original projects repositories are:

* [Platform](https://github.com/opendatamesh-initiative/odm-platform-pp-services)
* [Meta-service](https://github.com/opendatamesh-initiative/odm-platform-up-services-meta-blindata)

# Run Project #

## Prerequisites ##
* Docker
* docker-compose

## Run with docker-compose ##
### Clone repository
Clone the repository and move it to the project root folder

```bash
git clone https://bitbucket.org/quantyca/odm-platform-and-metaservice-docker/src/master/
cd odm-platform-and-metaservice-docker
```

### Build image
Build the docker-compose images of the applications and a default PostgreSQL DB.

Before building it, create a `.env` file in the root directory of the project similar to the following one:
```.dotenv
DATABASE_PORT=5432
DATABASE_NAME=odmplatformandmetaservicedb
DATABASE_USERNAME=usr
DATABASE_PASSWORD=pwd

METASERVICE_SPRING_PORT=8595

PLATFORM_SPRING_PORT=8585
```

Then, build the docker-compose file:
```bash
docker-compose build
```

### Run application
Run the docker-compose images.
```bash
docker-compose up
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

* [http://localhost:8585/api/v1/pp/swagger-ui/index.html](http://localhost:8585/api/v1/pp/swagger-ui/index.html)

The Meta-service will be auto invoked from the platform to register the notification events. 
You can check the notification in the shared PostgreSQL DB.

## Database
The application run using a dockerized postgresql running on *localhost* on given port (from *.env* file).
You can use your favourite sql client providing the proper connection parameters to check it.