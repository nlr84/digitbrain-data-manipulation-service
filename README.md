# digitbrain-data-manipulation-service
DIGITBrain Data Manipulation Service

# DIGITBRAIN Data Manipulation API

This project is developed in python together with FLASK and Pint python libraries. 
For its use, it will be containerized in Docker.

## Build Docker image

You can directly use Data Manipulation API image, or

### Build with docker-compose command

```sh
docker-compose  build
```

### Build with docker command

```sh
docker build -t DataManipulatioAPI -f Dockerfile .
```

## Run

### Run with docker-compose command

```sh
docker-compose up -d
```

### Run with docker command

```sh
docker run -d --name DataManipulatioAPI -p 5000:5000 DataManipulatioAPI
```


## Check if docker container is running

In example, openning _http://LocalContainerIP:5000/api/v2/healthcheck_, you will see

```
{status:"OK"}

CODE: 200
``` 

## This project can do

1. Convert magnitudes to other units compatible with the same magnitude.
2. Convert json files to csv and vice versa.
3. Convert csv files to xslx and vice versa.


