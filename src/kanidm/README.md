# 🔐 Kanidm Service

A modular Docker Compose configuration system for Kanidm identity management server with support for multiple environments and SSL configurations.

## 🏗️ Project Structure

```sh
src/kanidm/
├── components/                              # Source compose components
│   ├── base/                               # Base components
│   │   ├── docker-compose.yml              # Main Kanidm service
│   │   └── .env.example                    # Base environment variables
│   └── environments/                       # Environment components
│       ├── devcontainer/
│       │   └── docker-compose.yml          # DevContainer environment
│       ├── forwarding/
│       │   └── docker-compose.yml          # Development with port forwarding
│       ├── letsencrypt/
│       │   ├── docker-compose.yml          # Let's Encrypt SSL
│       │   └── .env.example                # Let's Encrypt variables
│       └── step-ca/
│           ├── docker-compose.yml          # Step CA SSL
│           └── .env.example                # Step CA variables
│   └── extensions/                         # Extension components (future use)
│       └── .gitkeep                        # Placeholder for extensions
├── build/                        # Generated configurations (auto-generated)
│   ├── devcontainer/
│   │   └── base/                 # DevContainer + base
│   ├── forwarding/
│   │   └── base/                 # Development + base
│   ├── letsencrypt/
│   │   └── base/                 # Let's Encrypt + base
│   └── step-ca/
│       └── base/                 # Step CA + base
├── build.sh                      # Build script
└── README.md                     # This file
```

## 🚀 Quick Start

### 1. Build Configurations

Run the build script to generate all possible combinations:

```bash
./build.sh
```

This will create all combinations in the `build/` directory.

### 2. Choose Your Configuration

Navigate to the desired configuration directory:

```bash
# For development with port forwarding
cd build/forwarding/base/

# For DevContainer environment
cd build/devcontainer/base/

# For production with Let's Encrypt SSL
cd build/letsencrypt/base/

# For production with Step CA SSL
cd build/step-ca/base/
```

### 3. Configure Environment

Copy and edit the environment file:

```bash
cp .env.example .env
# Edit .env with your values
```

### 4. Deploy

Start the services:

```bash
docker-compose up -d
```

Access: `https://localhost:8443` (for forwarding mode)

## 🔧 Available Configurations

### Environments

- **devcontainer**: Development environment with workspace network
- **forwarding**: Development environment with port forwarding (8443)
- **letsencrypt**: Production with Let's Encrypt SSL certificates
- **step-ca**: Production with Step CA SSL certificates

### Generated Combinations

Each environment provides a base configuration:

- `devcontainer/base` - DevContainer development setup
- `forwarding/base` - Development with port forwarding
- `letsencrypt/base` - Production with Let's Encrypt SSL
- `step-ca/base` - Production with Step CA SSL

## 🔧 Environment Variables

### Base Configuration

- `COMPOSE_PROJECT_NAME`: Project name for Docker Compose
- `KANIDM_VERSION`: Kanidm server version (default: latest)
- `KANIDM_LOG_LEVEL`: Log level (default: info)
- `KANIDM_BINDADDRESS`: Bind address (default: [::]:8443)
- `KANIDM_DOMAIN`: Domain name (default: localhost)
- `KANIDM_ORIGIN`: Origin URL (default: <https://localhost:8443>)
- `KANIDM_DB_PATH`: Database file path (default: /data/kanidm.db)
- `KANIDM_TLS_CHAIN`: TLS certificate chain path
- `KANIDM_TLS_KEY`: TLS private key path

### Let's Encrypt Configuration

- `VIRTUAL_PORT`: Port for nginx-proxy (default: 8443)
- `VIRTUAL_HOST`: Domain for nginx-proxy
- `VIRTUAL_PROTO`: Protocol for nginx-proxy (default: https)
- `LETSENCRYPT_HOST`: Domain for SSL certificate
- `LETSENCRYPT_EMAIL`: Email for certificate registration

### Step CA Configuration

- `VIRTUAL_PORT`: Port for nginx-proxy (default: 8443)
- `VIRTUAL_HOST`: Domain for nginx-proxy
- `VIRTUAL_PROTO`: Protocol for nginx-proxy (default: https)
- `LETSENCRYPT_HOST`: Domain for SSL certificate
- `LETSENCRYPT_EMAIL`: Email for certificate registration

## 🛠️ Development

### Adding New Environments

1. Create directory in `components/environments/` with `docker-compose.yml` and optional `.env.example` file
2. Run `./build.sh` to generate new combinations

### File Naming Convention

All component files follow the standard Docker Compose naming convention (`docker-compose.yml`) for:

- **VS Code compatibility**: Full support for Docker Compose language features and IntelliSense
- **IDE integration**: Proper syntax highlighting and validation in all major editors
- **Tool compatibility**: Works with Docker Compose plugins and extensions
- **Standard compliance**: Follows official Docker Compose file naming patterns

### Modifying Existing Components

1. Edit the component files in `components/`
2. Run `./build.sh` to regenerate configurations
3. The `build/` directory will be completely recreated

## 🌐 Networks

- **Development**: `kanidm-network` (internal)
- **DevContainer**: `kanidm-workspace-network` (external)
- **Let's Encrypt**: `letsencrypt-network` (external)
- **Step CA**: `step-ca-network` (external)

## 🔒 Security

⚠️ **Production Checklist:**

- Configure proper domain and origin settings
- Use strong TLS certificates
- Configure firewall rules
- Regular security updates
- Review Kanidm security documentation

## 🆘 Troubleshooting

**Build Issues:**

- Ensure `yq` is installed: <https://github.com/mikefarah/yq#install>
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
- Missing `.env.*` files for components are handled gracefully by the build script

## 🔄 Migration from Legacy Deploy Script

The legacy `deploy.sh` script has been removed in favor of the new build system:

**Legacy approach:**

```bash
./deploy.sh --forwarding
```

**New approach:**

```bash
./build.sh
cd build/forwarding/base/
cp .env.example .env
docker-compose up -d
