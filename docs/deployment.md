# ToolJet Deployment Guide

## Overview

ToolJet supports multiple deployment options ranging from simple Docker containers to complex Kubernetes clusters. This guide covers the various deployment strategies and their configurations.

## Deployment Architecture

### Core Components

**ToolJet Server**
- NestJS API server
- Handles authentication, data processing, and API requests
- Requires PostgreSQL and Redis

**ToolJet Client**
- React frontend application
- Serves the visual app builder and runtime
- Can be served statically or through the server

**PostgreSQL Database**
- Primary data storage
- Stores application data, user information, and configurations
- Required for all deployments

**Redis**
- Session storage and job queues
- Caching layer for performance optimization
- Required for production deployments

**PostgREST (Optional)**
- RESTful API for ToolJet Database
- Converts PostgreSQL schema to REST API
- Required only for ToolJet Database features

## Deployment Options

### 1. Docker Deployment

#### Single Container (Quick Start)

```bash
docker run \
  --name tooljet \
  --restart unless-stopped \
  -p 80:80 \
  --platform linux/amd64 \
  -v tooljet_data:/var/lib/postgresql/13/main \
  tooljet/try:ee-lts-latest
```

#### Docker Compose (Recommended)

```yaml
version: "3"

services:
  tooljet:
    image: tooljet/tooljet-ce:latest
    restart: always
    ports:
      - "80:80"
    environment:
      - PG_HOST=postgres
      - PG_PORT=5432
      - PG_USER=postgres
      - PG_PASS=postgres
      - PG_DB=tooljet
      - REDIS_URL=redis://redis:6379
      - SECRET_KEY_BASE=your-secret-key
      - LOCKBOX_KEY=your-lockbox-key
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:13
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=tooljet
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    restart: always
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

### 2. Kubernetes Deployment

#### Basic Kubernetes Manifest

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tooljet-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      component: tooljet
  template:
    metadata:
      labels:
        component: tooljet
    spec:
      containers:
        - name: tooljet
          image: tooljet/tooljet-ce:latest
          ports:
            - containerPort: 3000
          env:
            - name: PG_HOST
              value: "postgres-service"
            - name: PG_USER
              valueFrom:
                secretKeyRef:
                  name: tooljet-secrets
                  key: pg-user
            - name: PG_PASS
              valueFrom:
                secretKeyRef:
                  name: tooljet-secrets
                  key: pg-password
          resources:
            requests:
              memory: "1Gi"
              cpu: "500m"
            limits:
              memory: "2Gi"
              cpu: "1000m"
```

#### Service Configuration

```yaml
apiVersion: v1
kind: Service
metadata:
  name: tooljet-service
spec:
  selector:
    component: tooljet
  ports:
    - port: 80
      targetPort: 3000
  type: LoadBalancer
```

### 3. Helm Chart Deployment

#### Install ToolJet with Helm

```bash
# Add ToolJet Helm repository
helm repo add tooljet https://charts.tooljet.ai
helm repo update

# Install ToolJet
helm install tooljet tooljet/tooljet \
  --set postgresql.enabled=true \
  --set redis.enabled=true \
  --set apps.tooljet.secret.data.pg_user=postgres \
  --set apps.tooljet.secret.data.pg_password=your-password
```

#### Custom Values Configuration

```yaml
# values.yaml
apps:
  tooljet:
    service:
      type: ClusterIP
      host: "https://tooljet.yourdomain.com"
    deployment:
      resources:
        requests:
          memory: 1024Mi
          cpu: 1
        limits:
          memory: 2048Mi
          cpu: 2
    hpa:
      min: 2
      max: 10
      threshold:
        cpu: 0.75
        ram: 768Mi

postgresql:
  enabled: true
  auth:
    username: postgres
    password: your-secure-password
  primary:
    persistence:
      enabled: true
      size: 20Gi

redis:
  enabled: true
  auth:
    enabled: false
```

### 4. Cloud Platform Deployments

#### AWS ECS Deployment

```json
{
  "family": "tooljet",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "1024",
  "memory": "2048",
  "executionRoleArn": "arn:aws:iam::account:role/ecsTaskExecutionRole",
  "taskRoleArn": "arn:aws:iam::account:role/ecsTaskRole",
  "containerDefinitions": [
    {
      "name": "tooljet",
      "image": "tooljet/tooljet-ce:latest",
      "portMappings": [
        {
          "containerPort": 3000,
          "protocol": "tcp"
        }
      ],
      "environment": [
        {
          "name": "PG_HOST",
          "value": "your-rds-endpoint"
        }
      ],
      "secrets": [
        {
          "name": "PG_PASS",
          "valueFrom": "arn:aws:secretsmanager:region:account:secret:tooljet/db-password"
        }
      ]
    }
  ]
}
```

#### Google Cloud Run

```yaml
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: tooljet
spec:
  template:
    metadata:
      annotations:
        autoscaling.knative.dev/maxScale: "10"
    spec:
      containers:
        - image: tooljet/tooljet-ce:latest
          ports:
            - containerPort: 3000
          env:
            - name: PG_HOST
              value: "your-cloud-sql-instance"
            - name: PG_USER
              value: "postgres"
            - name: PG_PASS
              valueFrom:
                secretKeyRef:
                  name: tooljet-secrets
                  key: db-password
          resources:
            limits:
              cpu: "2"
              memory: "2Gi"
```

## Environment Configuration

### Required Environment Variables

```env
# Database Configuration
PG_HOST=localhost
PG_PORT=5432
PG_USER=postgres
PG_PASS=your-password
PG_DB=tooljet

# Application Security
SECRET_KEY_BASE=your-secret-key-base
LOCKBOX_KEY=your-lockbox-key

# Redis Configuration
REDIS_URL=redis://localhost:6379

# Email Configuration (Optional)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USERNAME=your-email@gmail.com
SMTP_PASSWORD=your-app-password

# Application Settings
SERVE_CLIENT=true
PORT=3000
NODE_ENV=production
```

### Optional Environment Variables

```env
# PostgREST Configuration (for ToolJet Database)
POSTGREST_HOST=localhost
POSTGREST_PORT=3001

# File Storage
STORAGE_DRIVER=local
STORAGE_LOCAL_UPLOAD_PATH=./uploads

# Monitoring
SENTRY_DSN=your-sentry-dsn
ENABLE_OPENTELEMETRY=true

# Feature Flags
ENABLE_AI_FEATURES=true
ENABLE_MARKETPLACE=true
```

## Security Considerations

### Production Security Checklist

- [ ] **Database Security**
  - Use strong passwords for database access
  - Enable SSL/TLS for database connections
  - Restrict database access to application servers only

- [ ] **Application Security**
  - Use strong SECRET_KEY_BASE and LOCKBOX_KEY
  - Enable HTTPS in production
  - Configure proper CORS settings
  - Enable rate limiting

- [ ] **Infrastructure Security**
  - Use private networks for database and Redis
  - Implement proper firewall rules
  - Regular security updates for base images
  - Use secrets management for sensitive data

### SSL/TLS Configuration

#### Nginx Reverse Proxy

```nginx
server {
    listen 443 ssl http2;
    server_name tooljet.yourdomain.com;

    ssl_certificate /path/to/certificate.crt;
    ssl_certificate_key /path/to/private.key;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## Monitoring and Observability

### Health Checks

ToolJet provides health check endpoints:

- **Server Health**: `GET /api/health`
- **Database Health**: `GET /api/health/db`
- **Redis Health**: `GET /api/health/redis`

### Logging Configuration

```env
# Logging Level
LOG_LEVEL=info

# Log Format
LOG_FORMAT=json

# Log Output
LOG_OUTPUT=stdout
```

### Metrics Collection

ToolJet supports OpenTelemetry for metrics collection:

```env
ENABLE_OPENTELEMETRY=true
OTEL_EXPORTER_OTLP_ENDPOINT=http://your-otel-collector:4317
```

## Backup and Recovery

### Database Backup

```bash
# Create backup
pg_dump -h $PG_HOST -U $PG_USER -d $PG_DB > backup_$(date +%Y%m%d_%H%M%S).sql

# Restore backup
psql -h $PG_HOST -U $PG_USER -d $PG_DB < backup_file.sql
```

### Automated Backup Script

```bash
#!/bin/bash
BACKUP_DIR="/backups"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="tooljet_backup_$DATE.sql"

pg_dump -h $PG_HOST -U $PG_USER -d $PG_DB > $BACKUP_DIR/$BACKUP_FILE
gzip $BACKUP_DIR/$BACKUP_FILE

# Keep only last 7 days of backups
find $BACKUP_DIR -name "tooljet_backup_*.sql.gz" -mtime +7 -delete
```

## Scaling Considerations

### Horizontal Scaling

**Load Balancer Configuration**
- Use sticky sessions for WebSocket connections
- Configure health checks for backend services
- Implement proper session management

**Database Scaling**
- Use read replicas for read-heavy workloads
- Implement connection pooling
- Consider database sharding for large deployments

### Vertical Scaling

**Resource Requirements**
- **Minimum**: 1 CPU, 2GB RAM
- **Recommended**: 2 CPU, 4GB RAM
- **High Load**: 4 CPU, 8GB RAM

## Troubleshooting

### Common Issues

**Database Connection Issues**
```bash
# Check database connectivity
psql -h $PG_HOST -U $PG_USER -d $PG_DB -c "SELECT 1;"

# Check database logs
docker logs postgres-container
```

**Memory Issues**
```bash
# Check memory usage
docker stats tooljet-container

# Monitor Node.js memory
node --max-old-space-size=4096 server.js
```

**Performance Issues**
```bash
# Check slow queries
# Enable query logging in PostgreSQL
# Monitor Redis memory usage
redis-cli info memory
```

### Debug Mode

```env
# Enable debug logging
NODE_ENV=development
LOG_LEVEL=debug

# Enable detailed error messages
DEBUG=tooljet:*
```

## Maintenance

### Regular Maintenance Tasks

- **Database Maintenance**: Regular VACUUM and ANALYZE
- **Log Rotation**: Implement log rotation for application logs
- **Security Updates**: Regular updates of base images and dependencies
- **Backup Verification**: Test backup restoration procedures

### Update Procedures

```bash
# Update ToolJet
docker pull tooljet/tooljet-ce:latest
docker-compose down
docker-compose up -d

# Run database migrations
docker exec tooljet-container npm run db:migrate
```

## Support and Resources

### Documentation
- [Official Documentation](https://docs.tooljet.ai)
- [Deployment Guides](https://docs.tooljet.ai/docs/setup/)
- [Troubleshooting Guide](https://docs.tooljet.ai/docs/troubleshooting/)

### Community Support
- [GitHub Issues](https://github.com/ToolJet/ToolJet/issues)
- [Slack Community](https://tooljet.ai/slack)
- [Discord Server](https://discord.gg/tooljet)
