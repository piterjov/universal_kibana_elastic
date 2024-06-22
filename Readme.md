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
   ```

docker-compose up -d

Access Kibana:

Open your browser and go to http://localhost:5601 to access the Kibana interface.
docker-compose logs elasticsearch
docker-compose logs kibana
Verify Elasticsearch Health:

Ensure that Elasticsearch is fully started and ready:

sh
Copy code
curl -X GET "localhost:9200/\_cluster/health?wait_for_status=yellow&timeout=50s"
Recreate Containers:

If you make changes to the docker-compose.yml file, recreate the containers:

docker-compose down
docker-compose up -d

Potential Issues
Kibana server is not ready yet:

Ensure Elasticsearch is fully started before Kibana tries to connect.
Verify that the ELASTICSEARCH_HOSTS environment variable in universal_kibana points to the correct service name (http://universal_elasticsearch:9200).
Elasticsearch memory issues:

Increase memory allocation by setting ES_JAVA_OPTS in the universal_elasticsearch service.
