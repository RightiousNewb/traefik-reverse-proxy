# Traefik Reverse Proxy Setup

This repository demonstrates a practical implementation of Traefik as a reverse proxy with Docker, featuring automatic SSL certificate management through Let's Encrypt and Cloudflare integration.

## Overview

Traefik is a modern HTTP reverse proxy and load balancer that makes deploying microservices easy. This setup includes:
- Automatic SSL certificate generation and renewal
- Docker integration
- Cloudflare DNS integration
- Secure headers configuration
- HTTP to HTTPS redirection

## Prerequisites

- Docker and Docker Compose installed
- A domain name configured with Cloudflare
- Cloudflare API credentials

## Directory Structure

```
traefik-reverse-proxy/
├── traefik-config/
│   ├── config/
│   │   ├── traefik.yml
│   │   ├── config.yml
│   │   └── acme.json
│   └── docker-compose.yml
└── docker-compose.yml
```

## Setup Instructions

1. First, create the required network for Docker:
   ```bash
   docker network create backend
   ```

2. Set up the ACME (Let's Encrypt) configuration:
   ```bash
   touch traefik-config/config/acme.json
   chmod 600 traefik-config/config/acme.json
   ```

3. Configure Cloudflare credentials:
   - Open `traefik-config/docker-compose.yml`
   - Update the following environment variables:
     ```yaml
     CF_API_EMAIL: your_email@example.com
     CF_API_KEY: your_api_key  # or
     CF_DNS_API_TOKEN: your_dns_api_token
     ```

4. Start the Traefik reverse proxy:
   ```bash
   docker-compose -f traefik-config/docker-compose.yml up -d
   docker-compose -f traefik-config/docker-compose.yml logs -f
   ```


5. Deploy your application:
   ```bash
   docker-compose up -d
   docker-compose logs -f
   ```

## Configuration Details

### Main Application (`docker-compose.yml`)
The main application is configured with Traefik labels for:
- Automatic HTTPS redirection
- SSL certificate management
- Secure headers
- Load balancing

### Traefik Configuration (`traefik-config/docker-compose.yml`)
The Traefik service is configured with:
- Port mappings (80, 443)
- Docker socket access
- SSL certificate management
- Cloudflare integration

## Security Considerations

- The `acme.json` file permissions are set to 600 to ensure only the owner can read/write
- Secure headers are enabled by default
- HTTP to HTTPS redirection is enforced
- No new privileges are allowed for the container

## Customization

To add more services, follow the pattern in the main `docker-compose.yml`:
1. Add your service configuration
2. Configure appropriate Traefik labels
3. Connect to the `backend` network

## Troubleshooting

1. Check Traefik logs:
   ```bash
   docker logs traefik
   ```

2. Verify network connectivity:
   ```bash
   docker network inspect backend
   ```

3. Ensure all containers are running:
   ```bash
   docker ps
   ```

## Contributing

Feel free to submit issues and enhancement requests!