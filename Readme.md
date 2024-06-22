# Elasticsearch and Kibana Setup

This repository contains a Docker Compose configuration to set up Elasticsearch and Kibana in a centralized setup that can be used across multiple projects.

## Prerequisites

- Docker
- Docker Compose

## Getting Started

1. **Clone the repository**:

   ```sh
   git clone <repository-url>
   cd <repository-directory>
Start the services:

sh
Copy code
docker-compose up -d
Access Kibana:

Open your browser and go to http://localhost:5601 to access the Kibana interface.

Configuration
docker-compose.yml
The docker-compose.yml file defines the services:

yaml
Copy code
version: '7.16'
services:
  universal_elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.16.3
    container_name: universal_elasticsearch
    environment:
      - discovery.type=single-node
      - xpack.security.enabled=false
      - ES_JAVA_OPTS=-Xms1g -Xmx1g
    ports:
      - "9200:9200"
      - "9300:9300"
    volumes:
      - esdata:/usr/share/elasticsearch/data
    networks:
      - elastic
    healthcheck:
      test: ["CMD-SHELL", "curl -f http://localhost:9200 || exit 1"]
      interval: 30s
      timeout: 10s
      retries: 5

  universal_kibana:
    image: docker.elastic.co/kibana/kibana:7.16.3
    container_name: universal_kibana
    ports:
      - "5601:5601"
    environment:
      - ELASTICSEARCH_HOSTS=http://universal_elasticsearch:9200
    depends_on:
      universal_elasticsearch:
        condition: service_healthy
    networks:
      - elastic

volumes:
  esdata:
    driver: local

networks:
  elastic:
    driver: bridge
Debugging Tips
Check Docker Compose Logs:

sh
Copy code
docker-compose logs elasticsearch
docker-compose logs kibana
Verify Elasticsearch Health:

Ensure that Elasticsearch is fully started and ready:

sh
Copy code
curl -X GET "localhost:9200/_cluster/health?wait_for_status=yellow&timeout=50s"
Recreate Containers:

If you make changes to the docker-compose.yml file, recreate the containers:

sh
Copy code
docker-compose down
docker-compose up -d
Potential Issues
Kibana server is not ready yet:

Ensure Elasticsearch is fully started before Kibana tries to connect.
Verify that the ELASTICSEARCH_HOSTS environment variable in universal_kibana points to the correct service name (http://universal_elasticsearch:9200).
Elasticsearch memory issues:

Increase memory allocation by setting ES_JAVA_OPTS in the universal_elasticsearch service.
