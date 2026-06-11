# Config Server Pattern

### Concept
A centralized service that stores and serves configuration to all applications in a system. Instead of each app managing its own config files, they all pull configuration from one single place via an API.

### Benefits
- **Single source of truth** — one place to manage all configuration across all apps
- **Environment separation** — different configs for dev, UAT, prod without touching code
- **Dynamic updates** — change configuration at runtime without rebuilding or restarting apps
- **Security** — secrets and credentials stored and controlled centrally, not scattered across repos
- **Auditability** — track who changed what configuration and when
- **Consistency** — all microservices always get the same configuration values

### Implementations
- Spring Cloud Config Server
- HashiCorp Vault
- AWS Parameter Store / Secrets Manager
- Azure App Configuration
- Consul
- Kubernetes ConfigMaps + Secrets
- Zookeeper