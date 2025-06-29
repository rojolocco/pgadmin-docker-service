# pgAdmin Docker Service

A Docker containerized setup for pgAdmin 4, a web-based administration tool for PostgreSQL databases.

## Overview

This repository contains Docker Compose configurations for running pgAdmin 4 in both production and development environments. pgAdmin is a feature-rich open-source administration and management platform for PostgreSQL, the world's most advanced open-source database.

## Features

- Easy deployment of pgAdmin 4 using Docker
- Persistent storage for pgAdmin data
- Separate configurations for development and production environments
- Integration with external network (caddy_network) for reverse proxy support

## Requirements

- Docker and Docker Compose installed on your system
- External Docker network named `caddy_network` (for reverse proxy integration)

## Configuration

### Environment Variables

The service uses the following environment variables:

- `PGADMIN_DEFAULT_EMAIL`: The email address used for the default pgAdmin administrator account
- `PGADMIN_DEFAULT_PASSWORD`: The password for the default pgAdmin administrator account

Create your `.env` file based on the provided `.env.example`:

```bash
cp .env.example .env
```

Then edit the `.env` file to set your secure password.

## Usage

### Development Environment

To run pgAdmin in development mode:

```bash
docker-compose -f docker-compose.dev.yaml up -d
```

This will:
- Use the latest pgAdmin 4 image
- Load environment variables from `.env.dev`
- Map port 8082 on your host to port 80 in the container
- Access pgAdmin at http://localhost:8082

Default development credentials:
- Email: admin@admin.com
- Password: admin123

### Production Environment

To run pgAdmin in production mode:

```bash
docker-compose up -d
```

This will:
- Use pgAdmin 4 version 9.3
- Load environment variables from `.env`
- Connect to the external caddy_network (assumes reverse proxy configuration)
- No direct port mapping (access through reverse proxy)

## Data Persistence

pgAdmin data is stored in a Docker volume named `pgadmin_data`, ensuring that your server configurations, connection history, and other settings persist across container restarts.

## Network Configuration

The service connects to an external network named `caddy_network`. This is typically used with a reverse proxy like Caddy, Traefik, or Nginx for handling SSL termination and routing.

To create this network if it doesn't exist:

```bash
docker network create caddy_network
```

## Security Considerations

- Always change the default administrator password in production
- Consider using a reverse proxy with SSL for secure access
- Restrict access to pgAdmin in production environments

## License

See the [LICENSE](LICENSE) file for details.
