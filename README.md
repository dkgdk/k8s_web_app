Kubernetes Web App – Odoo + PostgreSQL + Monitoring Stack
-------------------------------------------------------------

This repository contains a complete Helm-based Kubernetes deployment for an Odoo web application, integrated with PostgreSQL and a full monitoring + alert system using Grafana, Prometheus, and PostgreSQL exporter.

Project Overview
Component	Description
Odoo	Main business application deployed on Kubernetes
PostgreSQL	Database backend for Odoo
Prometheus	Collects metrics from all monitored components
Grafana	Visualization and dashboarding for metrics
PostgreSQL Exporter	Exposes PostgreSQL metrics to Prometheus
Alert System	Monitors container & port-forward status, sends email alerts
GitHub Actions	Automatically lints Helm charts and sends mail if build fails

Deployment Steps
Prerequisites

Kubernetes cluster (Minikube, EKS, GKE, etc.)

Helm installed (v3+)

Docker installed (for local monitoring)

kubectl configured and connected to the cluster

2️Deploy the Stack
helm install odoo .


To upgrade:

helm upgrade odoo .


To uninstall:

helm uninstall odoo

Monitoring Setup
Grafana & Prometheus

After deploying, run:

kubectl port-forward -n monitoring svc/grafana-service 3000:3000 &
kubectl port-forward -n monitoring svc/prometheus-service 9090:9090 &


Then open:

Grafana: http://localhost:3000

Prometheus: http://localhost:9090

Default Grafana login (if unchanged):

User: admin
Password: admin

PostgreSQL Exporter

Automatically discovered by Prometheus

Metrics available under /metrics endpoint

Email Alert System
Script: auto_container_watch_send_mail.sh

This script:

Monitors Docker containers & Kubernetes port-forwards

Restarts them automatically if stopped

Sends email alerts on:

Container failure

Restart events

Port-forward failures

Run manually:
bash scripts/auto_container_watch_send_mail.sh

Example alerts:

Container Exited → Odoo stopped unexpectedly

Port-forward Restarted → Prometheus/Grafana forward was lost and recovered

GitHub Action: Helm Chart Validation

Your workflow file: .github/workflows/helm-check.yml

This workflow:

Runs on main branch push or manual dispatch

Validates your Helm charts (helm lint, helm template)

Sends email notifications if any errors occur

Secret Configuration
Secret Name	Description
SMTP_SERVER	e.g. smtp.gmail.com
SMTP_PORT	usually 587
SMTP_USER	your sender email
SMTP_PASS	SMTP password or App Password
ALERT_EMAILS	space or comma separated recipient list (e.g. admin@example.com, dev@example.com)

You can add them in your repository settings:

Settings → Secrets and variables → Actions → New repository secret

Workflow Output
Helm lint successful
Templates rendered
Email alert sent on failure

.

Contributing

Pull requests are welcome!
For major changes, please open an issue first to discuss what you'd like to improve.
