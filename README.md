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

```sh
{status:"OK"}

CODE: 200
``` 

## This project can do

## Convert units

Endpoint

```sh
/api/v2/unit_conversion	JSON 	
```

Payload Structure:
```sh
{
"from": "Source value unit "(String); 
 "to": "Desired unit to get with the conversion"(String); 
 "value": Source numeric value(int, float,double)
}	
```

Response:
```sh
{
"unit": String selected unit,
 "value": converted value
} 
```

## Convert files to another formats


JSON -> CSV	
```sh
/api/v2/convert_json_to_csv	JSON File	CSV File
```
CSV -> JSON
```sh
	/api/v2/convert_csv_to_json	CSV File	JSON File
```
CSV -> XLSX
```sh
	/api/v2/convert_csv_to_xlsx	CSV File	XLSX File
```
XLSX -> CSV
```sh
	/api/v2/convert_xlsx_to_csv	XLSX File	CSV File 
```

