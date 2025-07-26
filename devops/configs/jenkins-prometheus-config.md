# Jenkins Prometheus Plugin Configuration

## Overview
This document describes the configuration steps for setting up Jenkins to expose metrics for Prometheus monitoring.

## Plugin Installation
1. **Navigate to Plugin Management**:
   - Go to Jenkins → Manage Jenkins → Plugins
   - Click "Available plugins" tab
   - Search for "Prometheus metrics"
   - Install the "Prometheus metrics plugin"
   - Restart Jenkins to activate the plugin

## Plugin Configuration
1. **Enable Metrics Collection**:
   - Go to Jenkins → Manage Jenkins → System (Configure System)
   - Scroll down to find the "Prometheus" section
   - Enable the checkbox: "Collect metrics for Prometheus"
   - Click "Save"

## Metrics Endpoint
- **URL**: `http://localhost:8080/prometheus`
- **Format**: Prometheus text format
- **Update Interval**: Real-time metrics collection
- **Authentication**: Uses Jenkins security settings

## Available Metrics Categories
- **Jenkins Status**: `default_jenkins_up`, `jenkins_health_check_score`
- **Executors**: `default_jenkins_executors_available`, `default_jenkins_executors_busy`
- **Queue**: `jenkins_queue_size_value`
- **Jobs**: `jenkins_job_count_value`, `jenkins_runs_total_total`
- **JVM Metrics**: Memory, threads, garbage collection
- **HTTP Metrics**: Request rates, response codes

## Integration with Prometheus
- **Scrape Target**: `http://jenkins:8080/prometheus`
- **Scrape Interval**: 5 seconds (configured in prometheus.yml)
- **Job Name**: jenkins
- **Network**: devops-final-project Docker network

## Verification Steps
1. **Check Metrics Endpoint**: Access `http://localhost:8080/prometheus`
2. **Verify Prometheus Scraping**: Check `http://localhost:9090/targets`
3. **Monitor in Grafana**: View metrics in Jenkins dashboard

## Configuration Files
- **Prometheus Config**: `prometheus.yml` (contains Jenkins scrape configuration)
- **Dashboard Export**: `jenkins-dashboard.json` (Grafana dashboard configuration)

## Key Metrics Used in Dashboard
1. `default_jenkins_up` - Jenkins availability status
2. `jenkins_health_check_score` - Overall health score (0-1)
3. `default_jenkins_executors_available` - Available build executors
4. `default_jenkins_executors_busy` - Currently busy executors
5. `jenkins_queue_size_value` - Number of jobs waiting in queue

## Notes
- Plugin requires Jenkins restart for activation
- Disk usage collection warnings are normal if CloudBees Disk Usage plugin is not installed
- All metrics are collected in real-time and exposed at the `/prometheus` endpoint