# Authentik Docker Compose Setup

This repository contains a Docker Compose configuration for running Authentik, an open-source Identity Provider.

## ⚠️ Security Notice

This repository contains sensitive configuration files that are excluded from version control via `.gitignore`. Before running this setup, you must create the required secret files.

## 🔄 Automated Updates

This repository uses [Renovate](https://github.com/renovatebot/renovate) for automated dependency updates:
- **Patch updates**: Automatically merged for security and bug fixes
- **Minor updates**: Require manual review for PostgreSQL and Redis
- **Major updates**: Require manual review and are labeled as breaking changes
- **Authentik updates**: Patch updates auto-merge, minor/major require review

## 🚀 Migration from Production Setup

If you're migrating from a production setup with `authentik-prod-1-*` naming:

### Option 1: Automated Migration (Recommended)
```bash
# 1. Create a backup first
./backup-data.sh

# 2. Run the migration script
./migrate-to-public.sh
```

### Option 2: Manual Migration
```bash
# 1. Stop current containers
docker-compose down

# 2. Ensure networks exist
docker network create frontend
docker network create backend

# 3. Start with new configuration
docker-compose up -d
```

**Data Safety**: All your data will be preserved during migration:
- ✅ PostgreSQL database data
- ✅ Redis cache data  
- ✅ Media files
- ✅ Custom templates
- ✅ SSL certificates

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

You need to create the following secret files before running the containers:

```bash
# Create secrets directory
mkdir -p secrets

# Create secret files (replace with your actual values)
echo "your-secret-key-here" > secrets/authentik_secret_key.txt
echo "your-postgres-password-here" > secrets/postgres_password.txt
echo "your-authentik-postgres-password-here" > secrets/authentik_postgressql__password.txt
```

### 3. Create SSL Certificates (Optional)

If you need SSL certificates, create them in the `certs/` directory:

```bash
mkdir -p certs
# Add your SSL certificates here
```

### 4. Create Required Networks

```bash
docker network create frontend
docker network create backend
```

### 5. Start the Services

```bash
docker-compose up -d
```

## Directory Structure

```
.
├── docker-compose.yaml      # Main Docker Compose configuration
├── renovate.json           # Renovate configuration for automated updates
├── .env.example            # Example environment variables
├── backup-data.sh          # Data backup script
├── migrate-to-public.sh    # Migration script for production setups
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

## Ports

- **9000**: HTTP port
- **9443**: HTTPS port

## Security Considerations

1. **Never commit secrets**: All secret files are excluded from version control
2. **Use strong passwords**: Generate strong, unique passwords for all services
3. **SSL certificates**: Use proper SSL certificates for production deployments
4. **Network security**: The setup uses separate frontend and backend networks
5. **Regular updates**: Renovate automatically handles dependency updates
6. **Sanitized configuration**: Production-specific naming has been removed
7. **Data persistence**: Named volumes ensure data survives container restarts

## Environment Variables

The following environment variables are configured:

- `AUTHENTIK_SECRET_KEY`: Secret key for Authentik
- `AUTHENTIK_REDIS__HOST`: Redis host
- `AUTHENTIK_POSTGRESQL__*`: PostgreSQL connection details

See `.env.example` for additional configuration options.

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

## Troubleshooting

### Check Service Status

```bash
docker-compose ps
```

### View Logs

```bash
docker-compose logs -f [service-name]
```

### Health Checks

The services include health checks for PostgreSQL and Redis. Check their status:

```bash
docker-compose ps
```

### Data Backup

Create a backup before making changes:

```bash
./backup-data.sh
```

## Contributing

When contributing to this repository:

1. Never commit secret files
2. Update the README if configuration changes
3. Test changes in a development environment first
4. Follow the existing naming conventions (sanitized for public use)

## License

[Add your license information here] 