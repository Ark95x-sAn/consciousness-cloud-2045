# Foundation Infrastructure Deployment Guide
## Consciousness Cloud 2045 - Phase 1 Setup

### Prerequisites
- Windows PC with admin access
- 16GB+ RAM, 4+ CPU cores
- 100GB+ free disk space
- Internet connection
- GitHub account

### Quick Start (30 Minutes)

```bash
# 1. Install Docker Desktop for Windows
# Download from: https://www.docker.com/products/docker-desktop
# Enable WSL2 integration

# 2. Install k3s (Lightweight Kubernetes)
wsl --install
curl -sfL https://get.k3s.io | sh -

# 3. Verify installation
kubectl get nodes

# 4. Deploy monitoring stack
kubectl create namespace monitoring
kubectl apply -f k8s/prometheus/
kubectl apply -f k8s/grafana/

# 5. Access dashboards
# Prometheus: http://localhost:9090
# Grafana: http://localhost:3000 (admin/admin)
```

### Directory Structure
```
consciousness-cloud-2045/
├── docs/                    # Documentation
├── k8s/                     # Kubernetes manifests
│   ├── prometheus/          # Monitoring
│   ├── grafana/            # Visualization  
│   ├── postgres/           # Database
│   └── services/           # Application services
├── scripts/                # Automation scripts
│   ├── setup.sh           # Initial setup
│   ├── deploy.sh          # Deployment
│   └── backup.sh          # Backup utilities
├── src/                    # Source code
│   ├── api/               # API services
│   ├── agents/            # AI agents
│   └── dashboard/         # Web dashboard
└── terraform/             # Infrastructure as Code
```

### Phase 1 Components

#### 1. Local Kubernetes Cluster (k3s)
- Lightweight, production-ready
- Single-node setup initially
- Easy scaling to multi-node

#### 2. Monitoring Stack
- **Prometheus**: Metrics collection
- **Grafana**: Visualization
- **Node Exporter**: System metrics
- **Alert Manager**: Notifications

#### 3. Database Layer
- **PostgreSQL**: Relational data
- **Redis**: Caching & queues
- **MongoDB**: Document storage (optional)

#### 4. Service Mesh
- **Traefik**: Ingress controller
- **NGINX**: Load balancing
- SSL/TLS termination

### Deployment Commands

```bash
# Clone repository
git clone https://github.com/Ark95x-sAn/consciousness-cloud-2045.git
cd consciousness-cloud-2045

# Run setup script
./scripts/setup.sh

# Deploy services
./scripts/deploy.sh --env=local

# Check status
kubectl get all -A

# Access logs
kubectl logs -f deployment/prometheus -n monitoring
```

### Configuration

Edit `.env` file:
```env
# Database
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_DB=consciousness_cloud
POSTGRES_USER=admin
POSTGRES_PASSWORD=changeme123

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# API
API_PORT=8080
API_SECRET_KEY=generate-secure-key

# Grafana
GRAFANA_ADMIN_PASSWORD=secure-password
```

### First Service Deployment

```yaml
# k8s/services/hello-world.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: hello-world
        image: nginxdemos/hello:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: hello-world
  namespace: default
spec:
  selector:
    app: hello-world
  ports:
  - port: 80
    targetPort: 80
  type: LoadBalancer
```

Deploy:
```bash
kubectl apply -f k8s/services/hello-world.yaml
kubectl get svc hello-world
```

### Monitoring & Observability

1. **View Metrics**
   - Navigate to Grafana: http://localhost:3000
   - Default login: admin/admin
   - Import dashboard: ID 1860 (Node Exporter)

2. **Check Logs**
```bash
# All pods
kubectl logs -f --all-containers=true -n monitoring

# Specific service
kubectl logs -f deployment/prometheus-server -n monitoring
```

3. **Resource Usage**
```bash
kubectl top nodes
kubectl top pods -A
```

### Troubleshooting

**Issue: Pods not starting**
```bash
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

**Issue: Service not accessible**
```bash
kubectl get svc
kubectl port-forward svc/<service-name> 8080:80
```

**Issue: Disk space**
```bash
docker system prune -a
kubectl delete pods --field-selector status.phase=Failed -A
```

### Next Steps

1. ✅ Deploy foundation (YOU ARE HERE)
2. 📝 Set up CI/CD pipeline
3. 🤖 Deploy first AI agent
4. 💰 Connect revenue tracking
5. 📊 Create business dashboard
6. 🔒 Implement security hardening
7. 🌐 Add external access (optional)

### Revenue-Generating Features

#### Week 1: MVP Service
- Basic monitoring dashboard
- Alert system setup
- Client onboarding portal
- **Deliverable**: "Sovereign Ops Stack in a Box"

#### Week 2: Automation
- Daily KPI reports
- Automated backup scripts
- Health check notifications
- **Deliverable**: "N8N Automation Template"

#### Week 3: Client Delivery
- Custom dashboards
- Training documentation
- 30-day support included
- **Price**: $2.5K-$10K setup fee

### Support & Resources

- **Documentation**: ./docs/
- **Issues**: GitHub Issues
- **Community**: Discord (TBD)
- **Updates**: Follow @Ark95x-sAn

### Cost Breakdown (Monthly)

**Local Setup:**
- Hardware (existing): $0
- Electricity (24/7): ~$15
- Internet: $50
- **Total**: ~$65/month

**Hybrid Cloud (Future):**
- Local: $65
- AWS/Azure minimal: $50-100
- **Total**: ~$150/month

**Target Revenue:**
- Month 1: $5K (2 clients)
- Month 2: $10K (4 clients)  
- Month 3: $15K (6 clients)
- **Profit Margin**: 90%+

### Security Best Practices

1. Change all default passwords
2. Enable firewall rules
3. Use secrets management
4. Regular backups
5. Monitor access logs
6. Keep systems updated

---

**Status**: Foundation Infrastructure Ready
**Next Action**: Run `./scripts/setup.sh`
**Support**: hello@consciousness-cloud.io (TBD)
