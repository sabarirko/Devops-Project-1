
## Prerequisites
- Jenkins server with necessary plugins (Git, Docker, Pipeline)
- Docker installed on build nodes
- Access to GitHub repository: https://github.com/hshar/website.git
- Configuration management tool (Ansible/Chef/Puppet) for initial setup

## DevOps Lifecycle Implementation

### 1. Infrastructure Setup
Infrastructure provisioning and software installation are automated using a configuration management tool. Required software includes:
- Git
- Docker
- Jenkins and plugins
- Testing frameworks

### 2. Git Workflow
We follow a feature branch workflow:
- `develop` branch for integration and testing
- `master` branch for production-ready code
- Feature branches are created from `develop` and merged via pull requests

### 3. Automated Build Pipeline (Jenkins)

#### Pipeline Jobs:
- **Job1: Build**
  - Triggers on push to `develop` or `master`
  - Containerizes application using Dockerfile
  - Builds Docker image from base: `hshar/webapp`
  - Places code in `/var/www/html` inside container

- **Job2: Test**
  - Runs automated tests
  - Executes on both `develop` and `master` branches

- **Job3: Prod**
  - Deploys to production **only** when triggered from `master` branch
  - `develop` branch only runs tests without deployment

### 4. Docker Configuration
The application is containerized using the provided Dockerfile:
```dockerfile
FROM hshar/webapp
COPY . /var/www/html
