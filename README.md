# DevSecOps Pipeline for Spring PetClinic

A comprehensive DevSecOps implementation project built around the Spring PetClinic application, featuring containerized CI/CD pipeline with security analysis, monitoring, and automated deployment.

## Project Overview

This project implements a complete DevSecOps pipeline using Docker containers for all services, with Jenkins as the CI/CD orchestrator. The pipeline integrates security scanning, code quality analysis, monitoring, and automated deployment to a production environment.

## Architecture

The project consists of the following components:

### Core Services
- **Jenkins**: CI/CD pipeline orchestration and automation
- **SonarQube**: Static code analysis and quality gates
- **OWASP ZAP**: Dynamic security scanning and vulnerability assessment
- **Prometheus**: Metrics collection and monitoring
- **Grafana**: Visualization and dashboards
- **Production VM**: Target deployment environment

### Network Architecture
All services are connected via a custom Docker network (`devops-final-project`) enabling secure inter-service communication.

## Team Structure & Responsibilities

### Individual Work Phase

| Team Member | Role | Deadline | Dependencies |
|-------------|------|----------|-------------|
| **Person A (Marco)** | Docker Infrastructure & Core Setup | July 18 | None |
| **Person B (Swastik/Ian)** | Repository & Jenkins Pipeline | July 19 | Person A |
| **Person C (Swastik/Ian)** | Security Analysis, VM & Grafana Setup | July 19 | Person A |
| **Person D (Cynthia)** | Monitoring & Visualization | July 22 | Person A, C |
| **Person E (Kavi)** | Deployment & Integration | July 26 | Person B, C, D |

### Documentation Phase
- **Infrastructure & Security Documentation**: Person A + C (July 27)
- **Pipeline & Monitoring Documentation**: Person B + D (July 27)
- **Integration Documentation**: Person E (July 27)

### Final Phase
- **Demo Video**: All team members (July 27)
- **Bonus Automation**: Person A + E (July 28)
- **Final Review & Submission**: All team members (July 29)

## Key Features

### 🔧 Infrastructure
- Containerized microservices architecture
- Custom Docker network for service isolation
- Persistent volume management for data retention
- Environment-specific configurations

### 🚀 CI/CD Pipeline
- Automated builds triggered by SCM polling
- Multi-stage pipeline with quality gates
- Blue Ocean visualization
- Automated testing and deployment

### 🔐 Security Integration
- Static Application Security Testing (SAST) with SonarQube
- Dynamic Application Security Testing (DAST) with OWASP ZAP
- Automated security report generation
- Security-first deployment practices

### 📊 Monitoring & Observability
- Real-time metrics collection with Prometheus
- Custom Grafana dashboards
- Jenkins performance monitoring
- Infrastructure health tracking

### 🎯 Deployment Strategy
- Ansible-based configuration management
- VM-based production environment
- Automated application deployment
- Environment consistency validation

## Service Configuration

### Jenkins (Port 8080)
- CI/CD pipeline orchestration
- SCM integration and build triggers
- Plugin ecosystem for extended functionality
- Blue Ocean for visual pipeline management

### SonarQube (Port 9000)
- Code quality analysis
- Security vulnerability detection
- Technical debt tracking
- Quality gate enforcement

### Grafana (Port 3000)
- Monitoring dashboards
- Prometheus data visualization
- Custom metric displays
- Alert management

### OWASP ZAP (Port 8090)
- Security vulnerability scanning
- Automated penetration testing
- Security report generation
- Integration with CI/CD pipeline

## Getting Started

### Prerequisites
- Docker and Docker Compose
- Git for repository management
- Virtual Machine for production deployment
- Ansible for configuration management

### Quick Start
```bash
# Clone the repository
git clone <repository-url>
cd spring-petclinic

# Start the DevOps environment
cd devops
docker-compose up -d

# Verify services are running
docker-compose ps
```

### Service Access
- Jenkins: http://localhost:8080
- SonarQube: http://localhost:9000
- Grafana: http://localhost:3000
- OWASP ZAP: http://localhost:8090

## Project Deliverables

### 📋 Documentation (30 points)
- Comprehensive setup instructions
- Step-by-step configuration guides
- Troubleshooting documentation

### ⚙️ Configuration Files (30 points)
- Docker Compose configurations
- Jenkinsfile and pipeline scripts
- Ansible playbooks
- Service configuration files

### 📸 Screenshots (20 points)
- Service dashboards and interfaces
- Pipeline execution evidence
- Deployment verification
- Before/after code change comparisons

### 🎥 Demo Video (20 points)
- End-to-end pipeline demonstration
- Automated deployment showcase
- Monitoring and alerting features

### 🏆 Bonus Features (15 points)
- Advanced automation scripting
- One-command deployment
- Enhanced monitoring capabilities

## Technology Stack

- **Containerization**: Docker, Docker Compose
- **CI/CD**: Jenkins, Blue Ocean Plugin
- **Code Quality**: SonarQube
- **Security**: OWASP ZAP
- **Monitoring**: Prometheus, Grafana
- **Configuration Management**: Ansible
- **Source Control**: Git/GitHub
- **Application**: Spring Boot (PetClinic)

## Security Considerations

- Network isolation using custom Docker networks
- Secure service-to-service communication
- Automated security scanning in CI/CD pipeline
- Configuration management best practices
- Secret management and environment separation

## Monitoring Strategy

- **Application Metrics**: Performance, availability, errors
- **Infrastructure Metrics**: Resource utilization, container health
- **Pipeline Metrics**: Build success rates, deployment frequency
- **Security Metrics**: Vulnerability trends, scan results

## Future Enhancements

- Container orchestration with Kubernetes
- Advanced security scanning tools
- Multi-environment deployment strategies
- Enhanced monitoring and alerting
- Performance optimization strategies

---

**Project Deadline**: July 31, 2025  
**Course**: 17-636 DevOps  
**Institution**: Carnegie Mellon University