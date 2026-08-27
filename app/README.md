# 🔐 Kanidm Service

Docker Compose configuration for Kanidm identity management server with support for multiple environments and SSL configurations.

## 📁 Project Structure

- **`components/`** - Source components for building
  - `base/` - Base Kanidm configuration
  - `environments/` - Environments (devcontainer, forwarding, letsencrypt, step-ca)
- **`build/`** - Ready-to-use Docker Compose configurations
- **`stackbuilder.toml`** - Configuration for stackbuilder build system

## 🚀 Quick Start

### 1. Build configurations

```bash
sb build
```

### 2. Choose configuration

```bash
# For development with port forwarding
cd build/forwarding/
cp .env.example .env
# Edit .env file

# For production with Let's Encrypt SSL
cd build/letsencrypt/
cp .env.example .env
# Edit .env file

# For production with Step CA SSL
cd build/step-ca/
cp .env.example .env
# Edit .env file
```

### 3. Deploy

```bash
docker compose up --build -d
```

Access: `https://localhost:8443` (for forwarding mode)

## 🔧 Available Environments

- **forwarding** - Development with port forwarding (port 8443)
- **letsencrypt** - Production with Let's Encrypt SSL certificates
- **step-ca** - Production with Step CA SSL certificates
- **devcontainer** - Environment for DevContainer

## 🔧 Environment Variables

### Base Configuration

- `COMPOSE_PROJECT_NAME` - Project name for Docker Compose
- `KANIDM_VERSION` - Kanidm server version (default: `latest`)
- `KANIDM_LOG_LEVEL` - Log level (default: `info`)
- `KANIDM_BINDADDRESS` - Bind address (default: `[::]:8443`)
- `KANIDM_DOMAIN` - Domain name (default: localhost)
- `KANIDM_ORIGIN` - Origin URL (default: `https://localhost:8443`)
- `KANIDM_DB_PATH` - Database file path (default: `/data/kanidm.db`)
- `KANIDM_TLS_CHAIN` - TLS certificate chain path
- `KANIDM_TLS_KEY` - TLS private key path

### Let's Encrypt Configuration

- `VIRTUAL_PORT` - Port for nginx-proxy (default: `8443`)
- `VIRTUAL_HOST` - Domain for nginx-proxy
- `VIRTUAL_PROTO` - Protocol for nginx-proxy (default: `https`)
- `LETSENCRYPT_HOST` - Domain for SSL certificate
- `LETSENCRYPT_EMAIL` - Email for certificate registration

### Step CA Configuration

- `VIRTUAL_PORT` - Port for nginx-proxy (default: `8443`)
- `VIRTUAL_HOST` - Domain for nginx-proxy
- `VIRTUAL_PROTO` - Protocol for nginx-proxy (default: `https`)
- `LETSENCRYPT_HOST` - Domain for SSL certificate
- `LETSENCRYPT_EMAIL` - Email for certificate registration

## 🛠️ Development

### Adding New Environments

1. Create directory in `components/environments/` with `docker-compose.yml` and optional `.env.example` file
2. Add environment to `stackbuilder.toml`
3. Run `sb build` to generate new configurations

### Modifying Existing Components

1. Edit component files in `components/`
2. Run `sb build` to rebuild configurations
3. The `build/` directory will be completely recreated

## 🌐 Networks

- **forwarding**: `kanidm-network` (internal)
- **devcontainer**: `kanidm-workspace-network` (external)
- **letsencrypt**: `letsencrypt-network` (external)
- **step-ca**: `step-ca-network` (external)

## 🔒 Security

⚠️ **Production Checklist:**

- Configure proper domain and origin settings
- Use strong TLS certificates
- Configure firewall rules
- Regular security updates
- Review Kanidm security documentation

## 🆘 Troubleshooting

**Build Issues:**

- Ensure `sb` (stackbuilder) is installed
- Check component file syntax
- Verify all required files exist

**SSL Issues:**

- **Let's Encrypt**: Verify domain DNS and letsencrypt-manager
- **Step CA**: Check step-ca-manager and virtual network config

**Service Issues:**

- Check container logs: `docker logs kanidmd`
- Verify environment variables
- Ensure proper certificate configuration

## 📝 Notes

- The `build/` directory is automatically generated and should not be edited manually
- Environment variables in generated files use `$VARIABLE_NAME` format for proper interpolation
- Each generated configuration includes a complete `docker-compose.yml` and `.env.example`
- Missing `.env.*` files for components are handled gracefully
