# spring-elastic
# Read Me First
The following was discovered as part of building this project:

# Configurando o docker Elasticsearch
* baseado em: [Guia para o EalsticSearch no Docker](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#docker-cli-run-prod-mode)

#### Create a new docker network
* docker network create elastic

#### Pull the Elasticsearch Docker image
* docker pull docker.elastic.co/elasticsearch/elasticsearch:8.10.4

#### Optional: Install Cosign for your environment. Then use Cosign to verify the Elasticsearch image’s signature
* wget https://artifacts.elastic.co/cosign.pub
* cosign verify --key cosign.pub docker.elastic.co/elasticsearch/elasticsearch:8.10.4

#### Run docker container
```sh
docker run --name es01 --net elastic -p 9200:9200 -it -m 1GB docker.elastic.co/elasticsearch/elasticsearch:8.10.4
```
* OBS: (Use the -m flag to set a memory limit for the container. This removes the need to manually set the JVM size)

####  You can regenerate the credentials using the following commands.
```sh
docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
```

#### storing the elastic password as an environment variable in your shell
```sh
export ELASTIC_PASSWORD="your_password"
```

#### Copy the http_ca.crt SSL certificate from the container to your local machine
```sh
docker cp es01:/usr/share/elasticsearch/config/certs/http_ca.crt .
```

#### Make a REST API call to Elasticsearch to ensure the Elasticsearch container is running.
```sh
curl --cacert http_ca.crt -u elastic:{ELASTIC_PASSWORD} https://localhost:9200

```
