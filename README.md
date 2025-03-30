Here's a comprehensive `README.md` for using `rms01` as your Ansible control machine to deploy Rancher and RKE2:

```markdown
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
```

### 2. Verify SSH Access
```bash
ansible all -i inventory.ini -m ping
```
*Expected output*: All nodes should return `"ping": "pong"`

### 3. Deploy the Cluster
```bash
ansible-playbook -i inventory.ini k8s-cluster.yml
```

## ⏳ Deployment Timeline
| Phase | Duration | Description |
|-------|----------|-------------|
| Common Setup | ~2 min | Installs OpenSSH on all nodes |
| RKE2 Server | ~5 min | Configures control plane (cp01) |
| RKE2 Agents | ~3 min/node | Joins data plane nodes (dp01, dp02) |
| Rancher | ~5 min | Deploys Rancher on rms01 |

## 🔍 Post-Installation Verification

### Check Cluster Status
```bash
ssh julius@cp01
sudo /var/lib/rancher/rke2/bin/kubectl get nodes
```

### Access Rancher Dashboard
1. Open in browser: `https://192.168.1.159`
2. Complete initial setup:
   - Set admin password
   - Configure server URL (use `192.168.1.159`)

### Import Cluster to Rancher
1. In Rancher UI: **Cluster Management** → **Import Existing**
2. Run the provided command on cp01:
   ```bash
   sudo kubectl apply -f rancher-import.yaml
   ```

## 🛠️ Customization Options

### RKE2 Version
Edit `roles/rke2-server/tasks/main.yml`:
```yaml
shell: curl -sfL https://get.rke2.io | INSTALL_RKE2_CHANNEL=stable INSTALL_RKE2_VERSION=v1.24.12+rke2r1 sh -
```

### Rancher Version
Edit `roles/rancher/tasks/main.yml`:
```yaml
image: rancher/rancher:v2.7.5
```

## 🧹 Cleanup
To completely remove the deployment:
```bash
ansible-playbook -i inventory.ini cleanup.yml
```
*(Note: Requires separate cleanup playbook)*

## 📜 License
MIT License - See [LICENSE](LICENSE) for details
```

## Key Features Highlighted in This README:
1. **Clear Infrastructure Table** - Visual layout of your servers
2. **Step-by-Step Instructions** - From setup to verification
3. **Time Estimates** - Helps manage expectations
4. **Customization Section** - For version changes
5. **Rancher-Specific Guidance** - Focused on your management server

Would you like me to add any specific troubleshooting steps or security hardening recommendations?
