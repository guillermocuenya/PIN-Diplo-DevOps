# ☸️ Kubernetes Monitoring Stack — DevOps Capstone

> Final integrative project of the **Diploma in DevOps (Universidad Nacional de Córdoba)**: automated provisioning of a Kubernetes environment on Minikube with an NGINX workload and a **Prometheus + Grafana** monitoring stack installed via Helm.

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white) ![Minikube](https://img.shields.io/badge/Minikube-326CE5?style=flat-square&logo=kubernetes&logoColor=white) ![Helm](https://img.shields.io/badge/Helm-0F1689?style=flat-square&logo=helm&logoColor=white) ![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=flat-square&logo=prometheus&logoColor=white) ![Grafana](https://img.shields.io/badge/Grafana-F46800?style=flat-square&logo=grafana&logoColor=white) ![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnubash&logoColor=white) ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

## Overview
One script brings up the full environment and another tears it down, so the stack is reproducible end to end.

```
setup_devops.sh
 ├─ checks dependencies and starts Minikube
 ├─ deploys NGINX as a sample application
 ├─ installs Prometheus (namespace: monitoring) via Helm
 ├─ installs Grafana (namespace: grafana) via Helm with a PersistentVolumeClaim
 └─ prints the access URLs

cleanup_devops.sh
 └─ uninstalls the Helm releases, deletes namespaces and NGINX, stops and deletes Minikube
```

## Key features
- **Infrastructure automation** with idempotent Bash scripts.
- **Observability**: cluster and workload metrics in Prometheus, dashboards in Grafana.
- **Persistent storage** for Grafana dashboards via PVC.
- **Troubleshooting runbook** for common failure modes (pending pods, `SVC_UNREACHABLE`, unbound PVCs).

## Requirements
macOS or Linux with Docker, Minikube, kubectl and Helm (Homebrew on macOS).

## Usage
```bash
chmod +x scripts/setup_devops.sh scripts/cleanup_devops.sh
./scripts/setup_devops.sh      # provision
./scripts/cleanup_devops.sh    # tear down
```

Grafana admin password:
```bash
kubectl get secret --namespace grafana grafana -o jsonpath="{.data.admin-password}" | base64 --decode
```

## Documentation
- 📘 Full guide and troubleshooting (Spanish): [docs/GUIA.md](docs/GUIA.md)
- 📄 Project brief: `PIN_ Consigna Proyecto integrador final.pdf` · Final report: `PIN Final.pdf`

---

## Author

**Guillermo Cuenya** — Senior QA Automation Engineer / SDET · Telecommunications Engineer  
[LinkedIn](https://www.linkedin.com/in/guillermoadriancuenya/) · [GitHub](https://github.com/gcuenya1)
