Together, the sepitch-webapp and sepitch-ml images are for those who want to have the SE Pitch locally while avoiding the manual setup.  This is useful if you neaed to show the demo but aren't sure if there will be reliable internet access, or you just want to play around.  

Both images work together: the sepitch-webapp image has the jar (created using ml-gradle) of the sepitch demo code that gets started automatically when the image is instantiated, and the sepitch-ml has the MarkLogic database with the US demo data preloaded.

While it's possible to start both of these containers manually, an easier way is to use docker-compose.  To do so, copy the following information into a file called "docker-compose.yml", and preferably store that file in an appropriately named folder (e.g., "sepitch-docker-compose")

```
version: '3'
services:
  marklogic:
    image: mlregistry.marklogic.com/ajacobs/sepitch-ml:cust-data-1.0
    container_name: sepitch-ml
    ports:
      - "8000:8000"
      - "8001:8001"
      - "8002:8002"
      - "8022:8022"
  web:
    image: mlregistry.marklogic.com/ajacobs/sepitch-webapp:1.0
    container_name: sepitch-webapp
    depends_on: 
      - marklogic   
    ports:
      - "8077:8077"
networks:
  default:
    ipam:
      config:
        - subnet: 172.28.0.0/16
```

With the docker-compose.yml file now saved, open the Terminal and change to the same directory where the .yml file is stored and execute the following command:

```
docker-compose up -d
```

It may take a while to download the images the first time, but after that, you're done--you now have two separate docker containers that are running the sepitch demo.  Navigate to http://localhost:8077 to hit the front-end, or http://localhost:8022 to hit MarkLogic's app server directly.  If you have anything else running on any of the ports specified in the .yml file, close those processes before running the docker-compose command to avoid errors.

When you're done, you can run the following command from the directory you used initially to stop both containers:

```
docker-compose stop
```

Or if you want to delete the containers and network to have a fresh start, use:

```
docker-compose down
```

Or if you're completely done and don't want the images stored on your machine either, use:

```
docker-compose down --rmi all
```

