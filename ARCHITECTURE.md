# Dockworks Architecture Overview

## Project Structure

This project follows the architecture pattern defined in `.augment/architecture.md`, where each service is organized in a nested directory structure following the pattern: `services/{{service-name}}/{{service-version}}/`

```
dockworks/
├── services/
│   ├── traefik/
│   │   └── 3-2/
│   │       ├── config/
│   │       ├── Dockerfile
│   │       └── README.md
│   ├── mysql/
│   │   └── 8/
│   │       ├── config/
│   │       ├── Dockerfile
│   │       └── README.md
│   ├── minio/
│   │   └── 2024-10-13/
│   │       ├── config/
│   │       ├── Dockerfile
│   │       └── README.md
│   ├── mailpit/
│   │   └── 1-21/
│   │       ├── config/
│   │       ├── Dockerfile
│   │       └── README.md
│   ├── rabbitmq/
│   │   └── 3/
│   │       ├── config/
│   │       ├── Dockerfile
│   │       └── README.md
│   └── redis/
│       └── 6/
│           ├── config/
│           ├── Dockerfile
│           └── README.md
├── docker-compose.yaml
├── .env.example
├── Makefile
└── README.md
```

## Services Overview

### 1. Traefik 3.2 (services/traefik/3-2)
- **Purpose**: Reverse proxy and load balancer
- **Container**: dockworks_traefik_3-2
- **Ports**: 80 (HTTP), 443 (HTTPS), 8080 (Dashboard)
- **Network Role**: Entry point for all HTTP/HTTPS traffic
- **Base Image**: traefik:v3.2

### 2. MySQL 8 (services/mysql/8)
- **Purpose**: Relational database
- **Container**: dockworks_mysql_8
- **Ports**: 3306
- **Data Persistence**: mysql_data volume
- **Base Image**: mysql:8

### 3. MinIO 2024-10-13 (services/minio/2024-10-13)
- **Purpose**: S3-compatible object storage
- **Container**: dockworks_minio_2024-10-13
- **Ports**: 9000 (API), 9001 (Console)
- **Data Persistence**: minio_data volume
- **Traefik Routes**: minio.localhost, minio-console.localhost
- **Base Image**: minio/minio:RELEASE.2024-10-13T13-34-11Z

### 4. Mailpit 1-21 (services/mailpit/1-21)
- **Purpose**: Email testing tool
- **Container**: dockworks_mailpit_1-21
- **Ports**: 1025 (SMTP), 8025 (Web UI)
- **Traefik Routes**: mailpit.localhost
- **Base Image**: axllent/mailpit:v1.21

### 5. RabbitMQ 3 (services/rabbitmq/3)
- **Purpose**: Message broker
- **Container**: dockworks_rabbitmq_3
- **Ports**: 5672 (AMQP), 15672 (Management UI)
- **Data Persistence**: rabbitmq_data volume
- **Traefik Routes**: rabbitmq.localhost
- **Base Image**: rabbitmq:3-management

### 6. Redis 6 (services/redis/6)
- **Purpose**: In-memory data store and cache
- **Container**: dockworks_redis_6
- **Ports**: 6379
- **Data Persistence**: redis_data volume
- **Base Image**: redis:6

## Network Architecture

All services are connected to the `dockworks-network` bridge network, enabling:
- Service-to-service communication using service names
- Isolation from other Docker networks
- Centralized network management

## Traefik Integration

Services with web interfaces are accessible through Traefik using the following domains:
- **MinIO API**: http://minio.localhost
- **MinIO Console**: http://minio-console.localhost
- **Mailpit**: http://mailpit.localhost
- **RabbitMQ Management**: http://rabbitmq.localhost
- **Traefik Dashboard**: http://localhost:8080

## Data Persistence

The following Docker volumes ensure data persistence across container restarts:
- `mysql_data`: MySQL database files
- `minio_data`: MinIO object storage
- `rabbitmq_data`: RabbitMQ messages and configuration
- `redis_data`: Redis data

## Service Naming Convention

### Directory Structure
Pattern: `services/{{service-name}}/{{service-version}}/`

Note: Version numbers use hyphens instead of dots in directory names (e.g., `3-2` instead of `3.2`).

Examples:
- `services/traefik/3-2/`
- `services/mysql/8/`
- `services/minio/2024-10-13/`
- `services/mailpit/1-21/`
- `services/rabbitmq/3/`
- `services/redis/6/`

### Docker Compose Service Names
Pattern: `{{service-name}}_{{service-version}}`
- `traefik_3-2`
- `mysql_8`
- `minio_2024-10-13`
- `mailpit_1-21`
- `rabbitmq_3`
- `redis_6`

### Container Names
Pattern: `dockworks_{{service-name}}_{{service-version}}`
- `dockworks_traefik_3-2`
- `dockworks_mysql_8`
- `dockworks_minio_2024-10-13`
- `dockworks_mailpit_1-21`
- `dockworks_rabbitmq_3`
- `dockworks_redis_6`

## Configuration Management

Each service has a `config/` directory where service-specific configuration files can be placed:
- `services/traefik/3-2/config/` - Traefik configuration files
- `services/mysql/8/config/` - MySQL configuration (my.cnf)
- `services/minio/2024-10-13/config/` - MinIO configuration
- `services/mailpit/1-21/config/` - Mailpit configuration
- `services/rabbitmq/3/config/` - RabbitMQ configuration (rabbitmq.conf)
- `services/redis/6/config/` - Redis configuration (redis.conf)

## Environment Variables

Environment variables are managed through:
1. `.env` file (copy from `.env.example`)
2. Docker Compose environment sections
3. Service-specific Dockerfiles

## Health Checks

All services include health checks to ensure:
- Proper startup sequencing
- Service availability monitoring
- Automatic restart on failure

## Benefits of This Architecture

1. **Isolation**: Each service version is self-contained with all dependencies
2. **Versioning**: Multiple versions of the same service can coexist side-by-side
3. **Scalability**: Easy to add new services or versions without affecting existing ones
4. **Maintainability**: Clear nested organization makes navigation and updates simple
5. **Documentation**: Each service has its own README with specific details
6. **Consistency**: Standardized structure across all services
7. **Clarity**: Nested directories clearly show service name and version hierarchy

