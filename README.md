# dockworks

## Overview

Dockworks is a project for managing containerized services with a structured and consistent architecture.

## Architecture

This project follows a specific directory structure convention for organizing services. Each service is self-contained and follows a standardized layout to ensure consistency and maintainability.

### Service Directory Structure

Services are organized using the following nested directory pattern:

```
services/
└── {{service-name}}/
    └── {{service-version}}/
        ├── config/
        │   └── [configuration files here]
        ├── Dockerfile
        └── [other related files for this service]
```

**Key Points:**

- **Nested Structure**: Services follow a nested directory pattern `services/{{service-name}}/{{service-version}}/`
  - `{{service-name}}`: The actual name of the service (e.g., `api`, `web`, `database`, `traefik`, `mysql`)
  - `{{service-version}}`: The version of the service (e.g., `v1`, `v2`, `1.0.0`, `latest`, `8`, `6`)

- **Configuration**: All configuration files must be organized in a `config/` subdirectory within each service version directory

- **Dockerfile**: Each service must have a `Dockerfile` at the root level of its version directory

- **Self-Contained**: All files related to a specific service version (configuration, source code, scripts, etc.) must be located within that service version's directory

### Examples

```
services/api/v1/              # API service version 1
services/web/v2/              # Web service version 2
services/database/v1/         # Database service version 1
services/traefik/3-2/         # Traefik reverse proxy v3.2
services/mysql/8/             # MySQL 8 database
services/minio/2024-10-13/    # MinIO release 2024-10-13
services/mailpit/1-21/        # Mailpit v1.21
services/redis/6/             # Redis 6 cache
```

### Detailed Architecture Documentation

For comprehensive architecture documentation, including best practices, benefits, and detailed examples, please refer to:

**[Architecture Documentation](.augment/architecture.md)**

## Getting Started

### Prerequisites

- Docker
- Docker Compose

### Quick Start

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd dockworks
   ```

2. **Copy the environment file**
   ```bash
   cp .env.example .env
   ```

3. **Start all services**
   ```bash
   docker-compose up -d
   ```

4. **Check service status**
   ```bash
   docker-compose ps
   ```

### Available Services

The following services are included in this setup:

| Service | Service Directory | Container Name | Ports | Web Access |
|---------|------------------|---------------|-------|------------|
| **Traefik 3.2** | services/traefik/3-2/ | dockworks_traefik_3-2 | 80, 443, 8080 | http://localhost:8080 (Dashboard) |
| **MySQL 8** | services/mysql/8/ | dockworks_mysql_8 | 3306 | - |
| **MinIO 2024-10-13** | services/minio/2024-10-13/ | dockworks_minio_2024-10-13 | 9000, 9001 | http://minio.localhost (API)<br>http://minio-console.localhost (Console) |
| **Mailpit 1-21** | services/mailpit/1-21/ | dockworks_mailpit_1-21 | 1025, 8025 | http://mailpit.localhost |
| **RabbitMQ 3** | services/rabbitmq/3/ | dockworks_rabbitmq_3 | 5672, 15672 | http://rabbitmq.localhost |
| **Redis 6** | services/redis/6/ | dockworks_redis_6 | 6379 | - |

### Service Details

#### Traefik
- **Purpose**: Reverse proxy and load balancer
- **Dashboard**: http://localhost:8080
- **Configuration**: Automatically discovers Docker containers

#### MySQL 8
- **Purpose**: Relational database
- **Default Credentials**:
  - Root Password: `root`
  - Database: `dockworks`
  - User: `dockworks`
  - Password: `dockworks`
- **Connection**: `mysql -h 127.0.0.1 -P 3306 -u dockworks -p`

#### MinIO
- **Purpose**: S3-compatible object storage
- **Default Credentials**:
  - Username: `minioadmin`
  - Password: `minioadmin`
- **API**: http://minio.localhost
- **Console**: http://minio-console.localhost

#### Mailpit
- **Purpose**: Email testing tool
- **SMTP**: localhost:1025
- **Web UI**: http://mailpit.localhost

#### RabbitMQ 3
- **Purpose**: Message broker
- **Default Credentials**:
  - Username: `rabbitmq`
  - Password: `rabbitmq`
- **AMQP**: localhost:5672
- **Management UI**: http://rabbitmq.localhost

#### Redis 6
- **Purpose**: In-memory data store and cache
- **Connection**: `redis-cli -h 127.0.0.1 -p 6379`

### Common Commands

```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# View logs
docker-compose logs -f

# View logs for specific service
docker-compose logs -f mysql_8

# Restart a service
docker-compose restart mysql_8

# Rebuild and restart services
docker-compose up -d --build

# Remove all containers and volumes
docker-compose down -v
```

### Network Configuration

All services are connected to the `dockworks-network` bridge network, allowing them to communicate with each other using their service names.

### Data Persistence

The following volumes are created for data persistence:
- `mysql_data`: MySQL database files
- `minio_data`: MinIO object storage
- `rabbitmq_data`: RabbitMQ data
- `redis_data`: Redis data

## Contributing

(Add your contributing guidelines here)
