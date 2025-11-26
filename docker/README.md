# Concept-Graphs -- Docker Setup Guide

This guide explains how to configure, build, and run the Concept-Graphs
environment using Docker and Docker Compose.

## Configure Dataset Path

Before building the container, edit
**[`docker-compose.yml`](./docker-compose.yml)** and update the dataset
directory mapping to point to your local dataset folder:

``` yaml
volumes:
  - /path/to/your/dataset:/home/user/data
```

Replace `/path/to/your/dataset` with the actual path on your machine.

## Build the Docker Image

To build the image defined in the `Dockerfile`, run:

``` bash
docker compose build
```

## Start the Docker Container

Start the service in detached mode:

``` bash
docker compose up -d
```

> **Note:** In order to enable GUI applications (OpenCV windows, visualization tools, etc.) run the following command:
```bash
xhost +local:docker
```

## Access the Running Container

Open an interactive shell inside the container:

``` bash
docker exec -it concept_graphs bash
```

## Run Concept-Graphs

Inside the container, run:

``` bash
./run_concept-graphs.sh
```

This will start the Concept-Graphs pipeline, considering the Replica dataset (same one used in the main documentation)