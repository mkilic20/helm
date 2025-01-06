# Helm Chart Structure for 2048 Game

This directory contains the Helm chart for deploying the 2048 game application with MySQL database integration.

## Chart Structure

```
charts/2048-game/
├── Chart.yaml             # Chart metadata and version information
├── values.yaml           # Default configuration values
├── templates/           # Kubernetes manifest templates
│   ├── deployment.yaml        # Game application deployment
│   ├── service.yaml          # Service for the game
│   ├── ingress.yaml          # Ingress configuration
│   ├── game-secrets.yaml     # Secrets for the application
│   └── mysql-init-configmap.yaml  # MySQL initialization
├── charts/              # Dependencies
│   └── mysql-9.23.0.tgz     # MySQL Helm chart
└── sql/                # SQL scripts
    └── init.sql        # Database initialization script
```

## Key Components

### Chart.yaml

```yaml
apiVersion: v2
name: 2048-game
description: A Helm chart for 2048 game with MySQL integration
version: 0.1.0
dependencies:
  - name: mysql
    version: 9.23.0
    repository: https://charts.bitnami.com/bitnami
```

### values.yaml

Contains configurable parameters for the deployment:

- Image repository and tag
- Service configurations
- Ingress settings
- MySQL configurations
- Resource limits and requests

### Templates

#### deployment.yaml

- Deploys the 2048 game application
- Configures environment variables for MySQL connection
- Sets up resource limits and health checks
- Mounts necessary volumes

#### service.yaml

- Creates a Kubernetes service for the game application
- Exposes the application port
- Enables communication between components

#### ingress.yaml

- Configures Ingress for external access
- Sets up host rules for 2048-game.local
- Manages TLS settings (if enabled)

#### game-secrets.yaml

- Stores sensitive information
- Manages MySQL credentials
- Contains application secrets

#### mysql-init-configmap.yaml

- Contains initialization SQL scripts
- Sets up database schema
- Creates necessary tables for score tracking

## MySQL Integration

The chart uses Bitnami's MySQL Helm chart as a dependency. Key features:

- Automated database initialization
- Persistent volume configuration
- Secure credential management
- Backup and restore capabilities

## Configuration Options

### Game Application

```yaml
image:
  repository: your-registry/2048-game
  tag: latest
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80

ingress:
  enabled: true
  host: "2048-game.local"
```

### MySQL Configuration

```yaml
mysql:
  auth:
    database: game_db
    username: game_user
  primary:
    persistence:
      size: 8Gi
```

## Installation

1. Update dependencies:

```bash
helm dependency update ./charts/2048-game
```

2. Install the chart:

```bash
helm install 2048-game ./charts/2048-game
```

3. Upgrade existing installation:

```bash
helm upgrade 2048-game ./charts/2048-game
```

## Customization

To override default values:

1. Create a custom-values.yaml file
2. Override specific values:

```yaml
image:
  tag: v1.0.0

ingress:
  host: "custom-domain.local"
```

3. Install with custom values:

```bash
helm install -f custom-values.yaml 2048-game ./charts/2048-game
```

## Troubleshooting

### Common Issues

1. MySQL Connection:

```bash
kubectl logs deployment/2048-game
kubectl describe pod -l app=2048-game
```

2. Ingress Issues:

```bash
kubectl get ingress
kubectl describe ingress 2048-game-ingress
```

3. Persistent Volume:

```bash
kubectl get pvc
kubectl describe pvc data-mysql-0
```
