---
layout: post
title: Data Engineering Week 1 - Docker and Postgresql 
date: 2023-11-07 08:57:00-0400
description: Introduction to data engineering by Alexey Grigorev
tags: data-engineering
categories: data
giscus_comments: false
related_posts: false
toc:
  beginning: true
---

The Data Engineering Zoomcamp is a 7 week data engineering course. It offers free instruction on the most widely used technologies and tools in data engineering and the cloud. During week 1, participants learns to build a data pipeline and retrieve and ingest data using Docker, Postgres, Docker-compose, Terraform and Google Cloud. Key topics covered in week 1 include: Docker, Postgres, Docker-compose, Terraform, Google Cloud, and Google VM.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/blog/data/de.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Data Engineering Pipeline
</div>

### Docker and Postgres

**Introduction of the Docker**

Docker is an open-source platform that allows developer to easily create, deploy and run applications in containers. Containers are lightweight portable and self suffcient environments that allow applications to run consistently across different environment.

**Introduction of Postgres**

Postgres is a versatile database that is designed for transactional purposes rather than analytics. Despite this, it is powerful and sometimes employed as a data warehouse solution.

### Docker command

```shell
docker run hello-world

# -it: run iterative mode

docker run -it ubuntu bash \
docker run -it python:3.9 \
docker run -it --entrypoint=bash python:3.9`

# build an image called taxi-ingest:v001

docker build -t taxi-ingest:v001 .
docker run -it test:pandas`
```

**Ingesting data into the database**

Once you have created this Dockerfile, you can build the image by running the command docker build -t mypostgres . and then run it with docker run -p 5432:5432 mypostgres. This will start the container and map port 5432 on the host to port 5432 in the container. Dockerfile

```python
FROM python:3.9.1

RUN apt-get install wget
RUN pip install pandas sqlalchemy psycopg2

WORKDIR /app
COPY ingest_data.py ingest_data.py

ENTRYPOINT [ "python", "ingest_data.py" ]

```

By running the above commands, your dataset will be loaded and ready for use in the postgres container.

**Running Postgres locally with Docker**

`docker run -it \
 -e POSTGRES_USER="root" \
 -e POSTGRES_PASSWORD="root" \
 -e POSTGRES_DB="ny_taxi" \
 -v "$(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data"\
 -p 5432:5432 \
 postgres:13`

To interact with Postgres in command line we can install [pgcli](https://www.pgcli.com/install).

**Install pgcli with pip**

`pip install pgcli`

**Connect with the postgres**

`pgcli -h localhost -p 5432 -u root -d ny_taxi`

**Check with tables**

`\dt`

**Jupyter Notebook**

we will import yellow trip taxi data from nyc taxi website and read the data as iteration in chunks to insert into the sql database.

```python
# read the data with pandas
df_iter = pd.read_csv('./yellow_tripdata_2021-07.csv', iterator=True, chunksize=100000, low_memory=False)

# used next(iter object) to iterate through chunks
df = next(df_iter)

# convert pick and drop time into datetime objects
df.tpep_pickup_datetime = pd.to_datetime(df.tpep_pickup_datetime)
df.tpep_dropoff_datetime = pd.to_datetime(df.tpep_dropoff_datetime)

# insert to sql
%time df.to_sql(name='yellow_taxi_data', con=engine, if_exists='replace')

from time import time
while True:
    t_start = time()

    df = next(df_iter)

    df.tpep_pickup_datetime = pd.to_datetime(df.tpep_pickup_datetime)
    df.tpep_dropoff_datetime = pd.to_datetime(df.tpep_dropoff_datetime)

    df.to_sql(name='yellow_taxi_data', con=engine, if_exists='append')

    t_end = time()

    print('inserted another chunk..., took %.3f second' % (t_end - t_start))
```

**pgAdmin**

- It’s not convenient to use pgcli for data exploration and querying. Instead, we will use [pgAdmin](https://www.pgadmin.org/), the standard graphical tool for postgres. We can run it with docker. However, this docker container can’t access the postgres container. We need to link them.

`docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  dpage/pgadmin4`

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/blog/data/pgadmin.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    pgadmin dashboard
</div>

**Docker Network**

```shell
# To link the database with pgadmin
# Connect a container to a network
docker network create pgnetwork

docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v "$(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data"\
  -p 5432:5432 \
  --name pgdatabase \
  --network pgnetwork \
  postgres:13

docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  --name pgadmin \
  --network pgnetwork \
  dpage/pgadmin4

# Ingest data
docker run -it \
    --network=pgnetwork \
    taxi-ingest:v001 \
        --user=root \
        --password=root \
        --host=pgdatabase \
        --port=5432 \
        --db=ny_taxi \
        --table_name=green_taxi_trips \
        --url=${URL}

```

It works, but we need to keep two terminal tabs running, manually create a network - and a bunch of other things. Let’s use docker compose that will take care of that.

**Docker-compose**

Docker Compose is a powerful tool that makes it easy to define and run multi-container Docker applications, simplifying the process of development, testing, and deployment. It allows developers to define all the services and dependencies of an application in a single file, and then start, stop, and manage those services with simple commands. Docker Compose also allows developers to define networks and volumes that can be shared between services. The docker-compose.yml file is a YAML file that defines the services, networks, and volumes needed for the application.

**Start the application in "detached" mode: the containers run in the background and the terminal is free for other commands.**
`docker-compose up -d`

**Stop and remove the containers**
`docker-compose down`

docker-compose.yml file

```yaml
services:
  pgdatabase:
    image: postgres:13
    environment:
      - POSTGRES_USER=root
      - POSTGRES_PASSWORD=root
      - POSTGRES_DB=ny_taxi
    volumes:
      - "./ny_taxi_postgres_data:/var/lib/postgresql/data:rw"
    ports:
      - "5432:5432"
  pgadmin:
    image: dpage/pgadmin4
    environment:
      - PGADMIN_DEFAULT_EMAIL=admin@admin.com
      - PGADMIN_DEFAULT_PASSWORD=root
    ports:
      - "8080:80"
```

### Terraform + Google Cloud Platform

**Terraform**

[Terraform](https://www.terraform.io/) is an open-source infrastructure as code software tool that enables you to safely and predictably create, change, and improve infrastructure. First of all, follow the instruction to [install Terraform](https://developer.hashicorp.com/terraform/downloads?ajs_aid=1ddc0b9c-f34a-4de4-8a92-f83ddc38f58b&product_intent=terraform).

Command:

1. `terraform init`:
   - Initializes & configures the backend, installs plugins/providers, & checks out an existing configuration from a version control
2. `terraform plan`:
   - Matches/previews local changes against a remote state, and proposes an Execution Plan.
3. `terraform apply`:
   - Asks for approval to the proposed plan, and applies changes to cloud
4. `terraform destroy`
   - Removes your stack from the Cloud

**Google Account**

Step 1: Create a new google account and get free 300€.

Step 2: Create a new project.

Step 3: Create a service account

- Assign the role as Viewer.
- Create and download the key.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/blog/data/gcp.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Google Cloud Platform
</div>

**Install Google SDK**

```shell
~ ./google-cloud-sdk/install.sh
#Welcome to the Google Cloud CLI!

~ gcloud init

export GOOGLE_APPLICATION_CREDENTIALS="<path/to/your/service-account-authkeys>.json"

# Refresh token/session, and verify authentication
gcloud auth application-default login
```