# Wiki.js with PostgreSQL, Elasticsearch, and Kibana - Docker Compose Setup

## Overview
This Docker Compose setup deploys **Wiki.js**, a powerful and extensible wiki platform, along with **PostgreSQL** as the database, **Elasticsearch** for search functionality, and **Kibana** for visualization and monitoring.

## This is not for Production. It is only meant for your local development

## Services
### 1. **PostgreSQL** (`wikijs_postgres`)
- Image: `postgres:14`
- Purpose: Database storage for Wiki.js
- Exposed Port: `5432`
- Persistent Storage: `pgdata` volume

### 2. **Elasticsearch** (`wikijs_elastic`)
- Image: `docker.elastic.co/elasticsearch/elasticsearch:8.10.2`
- Purpose: Search engine for Wiki.js
- Exposed Port: `9200`
- Persistent Storage: `esdata` volume
- Security: Disabled (`xpack.security.enabled=false`)
- Memory Allocation: `512MB`

### 3. **Wiki.js** (`wikijs`)
- Image: `requarks/wiki:latest`
- Purpose: Main Wiki.js application
- Exposed Port: `3000`
- Dependencies: `postgres`, `elasticsearch`
- Environment Variables:
  - `DB_TYPE=postgres`
  - `DB_HOST=postgres`
  - `DB_PORT=5432`
  - `DB_USER=wikijs`
  - `DB_PASS=wikijs_password`
  - `DB_NAME=wiki`
  - `SEARCH_TYPE=elasticsearch`
  - `SEARCH_HOST=http://elasticsearch:9200`

### 4. **Kibana** (`wikijs_kibana`)
- Image: `docker.elastic.co/kibana/kibana:8.10.2`
- Purpose: Visualization and monitoring of Elasticsearch data
- Exposed Port: `5601`
- Dependency: `elasticsearch`

## Prerequisites
Ensure you have the following installed on your machine:
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## How to Use
### 1. **Clone this repository (if applicable)**
```sh
 git clone <repository-url>
 cd <repository-folder>
```

### 2. **Start the services**
```sh
docker-compose up -d
```

### 3. **Access the services**
- **Wiki.js:** [http://localhost:3000](http://localhost:3000)
- **Elasticsearch API:** [http://localhost:9200](http://localhost:9200)
- **Kibana:** [http://localhost:5601](http://localhost:5601)

### 4. **Stop the services**
```sh
docker-compose down
```

## Notes
- Ensure that your system has enough memory to run Elasticsearch and Kibana efficiently.
- By default, security in Elasticsearch is disabled. In production, consider enabling authentication.
- PostgreSQL and Elasticsearch data are persisted using Docker volumes (`pgdata` and `esdata`).

## Troubleshooting
### **Check Running Containers**
```sh
docker ps
```

### **View Logs for a Specific Service**
```sh
docker-compose logs -f wikijs
```

### **Restart a Service**
```sh
docker-compose restart wikijs
```

## License
This project is licensed under the MIT License.

