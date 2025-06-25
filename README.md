# Authentik Docker Compose Setup

This repository contains a Docker Compose configuration for running Authentik, an open-source Identity Provider.

## 🔄 Automated Updates

This repository uses [Renovate](https://github.com/renovatebot/renovate) for automated dependency updates:
- **Patch updates**: Automatically merged for security and bug fixes
- **Minor updates**: Require manual review for PostgreSQL and Redis
- **Major updates**: Require manual review and are labeled as breaking changes
- **Authentik updates**: Patch updates auto-merge, minor/major require review

## Prerequisites

- Docker and Docker Compose installed
- Git

## Setup Instructions

### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd <repository-name>
```

### 2. Create Required Secret Files

You need to create the following secret files before running the containers. You can either generate secure random secrets or use your own values:

```bash
# Create secrets directory
mkdir -p secrets

# Generate secure random secrets using OpenSSL
openssl rand -base64 32 > secrets/authentik_secret_key.txt
openssl rand -base64 24 > secrets/postgres_password.txt
openssl rand -base64 24 > secrets/authentik_postgressql__password.txt
```

**Note**: The Authentik secret key should be at least 32 characters long. The OpenSSL command generates a 32-byte random string encoded in base64, which provides excellent security.

### 3. Create Required Networks

```bash
docker network create frontend
docker network create backend
```

### 4. Start the Services

```bash
docker-compose up -d
```

## Directory Structure

```
.
├── docker-compose.yaml      # Main Docker Compose configuration
├── renovate.json           # Renovate configuration for automated updates
├── secrets/                # Secret files (not in version control)
│   ├── authentik_secret_key.txt
│   ├── postgres_password.txt
│   └── authentik_postgressql__password.txt
├── certs/                   # SSL certificates (not in version control)
├── media/                   # Authentik media files
├── custom-templates/        # Custom templates (optional)
└── .gitignore              # Git ignore rules
```

## Services

- **postgresql**: PostgreSQL database for Authentik
- **redis**: Redis cache for Authentik
- **server**: Authentik main server
- **worker**: Authentik background worker

## Environment Variables

The following environment variables are configured:

- `AUTHENTIK_SECRET_KEY`: Secret key for Authentik
- `AUTHENTIK_REDIS__HOST`: Redis host
- `AUTHENTIK_POSTGRESQL__*`: PostgreSQL connection details

## Automated Updates with Renovate

This repository is configured to automatically:
- Check for updates every Monday before 4 AM UTC
- Auto-merge patch updates for PostgreSQL and Redis
- Auto-merge patch updates for Authentik
- Create PRs for minor and major updates requiring review
- Apply appropriate labels and assignees

### Renovate Configuration

The `renovate.json` file configures:
- **Auto-merge**: Patch updates for infrastructure components
- **Manual review**: Minor and major updates for Authentik
- **Labels**: Automatic labeling for dependency updates
- **Schedule**: Weekly updates to minimize disruption