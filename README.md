# Kubernetes Cluster Deployment with Rancher and RKE2

This project automates the deployment of a production-ready Kubernetes cluster using:
- **RKE2** (Rancher Kubernetes Engine) for the cluster
- **Rancher** for cluster management
- **Ansible** for configuration management

## 📋 Prerequisites

### 🖥️ Infrastructure
| Hostname | IP Address | Role | Specifications |
|----------|------------|------|----------------|
| rms01 | 192.168.1.159 | Ansible Control + Rancher | 8GB RAM, 2 CPUs |
| cp01 | 192.168.1.162 | RKE2 Control Plane | 10GB RAM, 2 CPUs |
| dp01 | 192.168.1.160 | RKE2 Data Plane | 10GB RAM, 2 CPUs |
| dp02 | 192.168.1.161 | RKE2 Data Plane | 10GB RAM, 2 CPUs |

### 🔑 Access Requirements
- SSH key-based access to all servers as user `julius`
- Private key located at `~/.ssh/id_rsa` on rms01

## 🚀 Quick Start

### 1. Prepare the Control Machine (rms01)

```bash
# Connect to rms01
ssh julius@192.168.1.159

# Install Ansible and git
sudo apt update && sudo apt install -y ansible git

# Clone this repository
git clone https://github.com/your-repo/k8s-cluster.git
cd k8s-cluster
