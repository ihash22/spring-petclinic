# Jenkins Monitoring Setup Guide
**Person D (Cynthia) - Monitoring & Visualization**

## Overview
This guide provides step-by-step instructions for setting up Jenkins monitoring using Prometheus and Grafana visualization in a Docker environment.

## Prerequisites
- Docker and Docker Compose installed
- All DevOps services running via docker-compose.yml
- Jenkins accessible at http://localhost:8080

## Step 1: Install Prometheus Plugin in Jenkins

1. **Access Jenkins**:
   ```
   URL: http://localhost:8080
   Username: admin
   Password: 2e67464067c94c29beb5a4dcbe5ed594
   ```

2. **Navigate to Plugin Management**:
   - Go to: Manage Jenkins → Plugins
   - Click "Available plugins" tab
   - Search for: "Prometheus metrics"

3. **Install Plugin**:
   - Select "Prometheus metrics plugin"
   - Click "Install without restart"
   - Restart Jenkins when installation completes

## Step 2: Configure Jenkins Prometheus Plugin

1. **Enable Metrics Collection**:
   - Go to: Manage Jenkins → System
   - Find "Prometheus" section
   - Check: "Collect metrics for Prometheus"
   - Click "Save"

2. **Verify Metrics Endpoint**:
   - Access: http://localhost:8080/prometheus
   - Should display Jenkins metrics in Prometheus format

## Step 3: Configure Grafana Data Source

1. **Access Grafana**:
   ```
   URL: http://localhost:3000
   Username: admin
   Password: admin
   ```

2. **Add Prometheus Data Source**:
   - Go to: Connections → Data sources
   - Click "Add data source"
   - Select "Prometheus"
   - Configure:
     - Name: prometheus
     - URL: http://prometheus:9090
     - Access: Server (default)
   - Click "Save & test"
   - Verify: "Successfully queried the Prometheus API"

## Step 4: Create Jenkins Dashboard

1. **Create New Dashboard**:
   - Click "+" → Dashboard
   - Click "Add visualization"

2. **Add Gauge Panels** with these queries:

   **Panel 1: Jenkins Status**
   - Query: `default_jenkins_up`
   - Visualization: Gauge
   - Title: "Jenkins Status"
   - Thresholds: Red (0-0.5), Green (0.5-1)

   **Panel 2: Health Score**
   - Query: `jenkins_health_check_score`
   - Visualization: Gauge
   - Title: "Health Score"
   - Unit: Percent (0-1)
   - Thresholds: Red (0-0.7), Yellow (0.7-0.9), Green (0.9-1)

   **Panel 3: Available Executors**
   - Query: `default_jenkins_executors_available`
   - Visualization: Gauge
   - Title: "Available Executors"

   **Panel 4: Busy Executors**
   - Query: `default_jenkins_executors_busy`
   - Visualization: Gauge
   - Title: "Busy Executors"

   **Panel 5: Queue Size**
   - Query: `jenkins_queue_size_value`
   - Visualization: Gauge
   - Title: "Jobs in Queue"
   - Thresholds: Green (0-2), Yellow (3-5), Red (5+)

3. **Save Dashboard**:
   - Click "Save dashboard"
   - Title: "Jenkins Monitoring Dashboard"
   - Click "Save"

## Step 5: Verification

1. **Check Prometheus Targets**:
   - Go to: http://localhost:9090/targets
   - Verify Jenkins target shows "UP" status

2. **Verify Dashboard**:
   - All 5 gauge panels should display current values
   - Metrics should update automatically

## Troubleshooting

### Jenkins Plugin Issues
- Ensure Jenkins is restarted after plugin installation
- Check Jenkins logs for any plugin-related errors
- Disk usage warnings are normal if CloudBees plugin not installed

### Prometheus Connection Issues
- Verify Docker network connectivity: `docker network ls`
- Check container names match configuration
- Use container names (not localhost) for inter-container communication

### Grafana Data Source Issues
- Use `http://prometheus:9090` (not localhost) for data source URL
- Ensure Access is set to "Server" mode
- Test connection before proceeding to dashboard creation

## File Structure
```
devops/
├── configs/
│   ├── jenkins-dashboard.json          # Grafana dashboard export
│   ├── jenkins-prometheus-config.md    # Plugin configuration details
│   └── MONITORING_SETUP_GUIDE.md       # This setup guide
├── screenshots/
│   ├── 01-docker-services-running.png
│   ├── 02-prometheus-web-interface.png
│   ├── 03-jenkins-metrics-endpoint.png
│   ├── 04-grafana-datasource-config.png
│   ├── 05-grafana-jenkins-dashboard.png
│   └── 06-prometheus-targets.png
├── docker-compose.yml                  # Container orchestration
└── prometheus.yml                      # Prometheus configuration
```

## Key Metrics Monitored
- **Jenkins Availability**: Real-time status monitoring
- **Health Score**: Overall system health (0-1 scale)
- **Executor Management**: Available vs busy build executors
- **Queue Monitoring**: Build queue size and bottlenecks

## Integration Points
- **Data Flow**: Jenkins → Prometheus → Grafana
- **Network**: devops-final-project Docker network
- **Update Frequency**: 5-second scrape interval
- **Access Points**: Jenkins (8080), Prometheus (9090), Grafana (3000)