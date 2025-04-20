---
title: Lab Setup for Infrastructure Compromise Testing
date: 2023-01-02 12:30:00 +/-TTTT
categories: [Red Team, Lab Setup, Docker]
tags: [red-team, lab-setup, docker, infrastructure, penetration-testing, security]     # TAG names should always be lowercase
---

# Lab Setup for Infrastructure Compromise Testing

A comprehensive guide to setting up a vulnerable infrastructure lab using Docker containers for red team exercises and penetration testing practice.

## Table of Contents
{:.no_toc}

1. TOC
{:toc}

## Introduction

This guide provides detailed instructions for setting up a vulnerable infrastructure lab using Docker containers. Originally created for the Init_Meetup CTF, this lab environment offers a secure platform for practicing infrastructure compromise techniques without affecting production systems.

## Prerequisites

Before proceeding with the setup, ensure you have the following:

- Ubuntu Server VM (version 22.04 or later recommended)
- Docker installed and configured
- Basic understanding of Docker concepts and commands
- Stable network connectivity
- Sufficient system resources (minimum 4GB RAM, 20GB storage)

## Step 1: Install Ubuntu Server

Download and install Ubuntu Server from the official website:

```bash
# Download Ubuntu Server 22.04 LTS
wget https://releases.ubuntu.com/22.04/ubuntu-22.04.3-live-server-amd64.iso
```

## Step 2: Install Docker

Install Docker and configure it for optimal performance:

```bash
# Update package lists and upgrade existing packages
sudo apt-get update && sudo apt-get upgrade -y

# Install required dependencies
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Add Docker repository
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io

# Start and enable Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Verify Docker installation
sudo docker --version
```

## Step 3: Pull Required Docker Images

Pull the necessary Docker images for the lab environment:

```bash
# Pull vulnerable images with specific versions
docker pull vulhub/nginx:1.19.0
docker pull vulhub/php:7.4
docker pull vulhub/mysql:5.7

# Verify downloaded images
docker images
```

## Step 4: Create Docker Network

Set up a dedicated network for the lab environment:

```bash
# Create a new Docker network with custom subnet
docker network create --subnet=172.20.0.0/16 redteam-lab

# Verify network creation
docker network ls
```

## Step 5: Run Containers

Deploy the containers with appropriate configurations:

```bash
# Run Nginx container with custom configuration
docker run -d --name nginx \
  --network redteam-lab \
  -p 80:80 \
  -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro \
  vulhub/nginx:1.19.0

# Run PHP container with necessary extensions
docker run -d --name php \
  --network redteam-lab \
  -v $(pwd)/php.ini:/usr/local/etc/php/php.ini:ro \
  vulhub/php:7.4

# Run MySQL container with secure configuration
docker run -d --name mysql \
  --network redteam-lab \
  -e MYSQL_ROOT_PASSWORD=redteam \
  -e MYSQL_DATABASE=vuln_db \
  -e MYSQL_USER=test_user \
  -e MYSQL_PASSWORD=test_pass \
  vulhub/mysql:5.7
```

## Security Considerations

When setting up this lab environment, consider the following security measures:

- No Ubuntu ports are exposed directly, preventing attacks from affecting the host server
- Each service runs in an isolated container, providing process separation
- The lab network is segregated from the host network
- Default credentials are used for demonstration purposes only
- Regular updates and patches should be applied to containers
- Network traffic should be monitored for suspicious activity

## Testing the Setup

Verify the lab environment is functioning correctly:

```bash
# Check running containers and their status
docker ps -a

# Test Nginx web server
curl -I http://localhost

# Test MySQL database connection
docker exec -it mysql mysql -uroot -predteam -e "SHOW DATABASES;"

# Verify PHP-FPM connection
docker exec -it nginx curl http://php:9000/status
```

## Best Practices

When working with this lab environment:

1. **Security**
   - Never use default credentials in production environments
   - Regularly update containers to the latest secure versions
   - Implement proper network segmentation
   - Monitor container logs for suspicious activity

2. **Performance**
   - Allocate appropriate system resources
   - Use container resource limits
   - Implement proper logging and monitoring
   - Regular backup of important data

3. **Maintenance**
   - Schedule regular updates
   - Monitor container health
   - Clean up unused resources
   - Document all changes and configurations

## Conclusion

This lab setup provides a secure environment for practicing infrastructure compromise techniques. Remember to:

- Follow security best practices
- Keep all components updated
- Monitor system activity
- Document your findings and lessons learned

## References

- [Docker Official Documentation](https://docs.docker.com/)
- [Vulhub GitHub Repository](https://github.com/vulhub/vulhub)
- [Ubuntu Server Documentation](https://ubuntu.com/server/docs)
- [Docker Security Best Practices](https://docs.docker.com/engine/security/)

## License

This work is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
