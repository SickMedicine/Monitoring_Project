# Monitoring Stack Project

## Overview
This project provides a complete monitoring and log aggregation stack using Docker Compose, Prometheus, Grafana, Node Exporter, Logstash, and Ansible. It enables you to monitor system metrics (CPU, memory, disk, network) and aggregate logs, with automated deployment via Ansible.

---

## Prerequisites
- Ubuntu 20.04/22.04/24.04 (tested)
- Docker & Docker Compose
- Ansible
- Git (optional, for cloning)

---

## Setup Instructions

### 1. Clone the Repository
```bash
git clone <your-repo-url>
cd Monitoring_Project
```

### 2. Configure Inventory (if using remote hosts)
By default, the playbook runs locally. For remote hosts, edit your Ansible inventory.

### 3. Run the Ansible Playbook
```bash
cd ansible
ansible-playbook -i "localhost," -c local playbook.yml
```
This will:
- Ensure required directories exist
- Copy configuration files
- Start the monitoring stack via Docker Compose

---

## How to Start/Stop the Stack

### Start All Services
```bash
cd /home/test/monitoring  # or your chosen directory
sudo docker-compose up -d
```

### Stop All Services
```bash
sudo docker-compose down
```

### Check Status
```bash
sudo docker-compose ps
```

---

## Accessing the Services

- **Grafana:** [http://localhost:3000](http://localhost:3000) or `http://<your-vm-ip>:3000`
  - Default login: `admin` / `admin`
- **Prometheus:** [http://localhost:9090](http://localhost:9090) or `http://<your-vm-ip>:9090`
- **Node Exporter:** [http://localhost:9100/metrics](http://localhost:9100/metrics)

---

## Grafana Dashboard: Pre-built Panels

Pre-built Grafana panels are included in the file `test_project_dashboard.yml` in this repository. You do **not** need to manually create panels for the main metrics.

### How to Import the Dashboard
1. Log in to Grafana.
2. Click the **four squares icon** ("Dashboards") on the left sidebar, then **Import**.
3. Click **Upload JSON/YAML file** and select `test_project_dashboard.yml` from this repository.
4. Click **Import**.
5. The dashboard with all key panels (CPU, memory, disk, network, etc.) will appear and be ready to use.

---


## Troubleshooting
- **Ports already in use:** Ensure no other processes or containers are using 9090, 9100, 5044, etc. Use `sudo lsof -i :PORT` and `sudo docker ps` to check.
- **Read-only file system errors:** Use a writable directory (e.g., `/home/test/monitoring`) for configs and mounts.
- **No data in Grafana:**
  - Check Prometheus targets at `http://localhost:9090/targets`.
  - Ensure Node Exporter is UP and being scraped.
  - Try simple queries like `up` or `node_cpu_seconds_total` in Grafana Explore.
- **Permission denied with Docker:** Ensure your user is in the `docker` group or use `sudo`.
- **Reset Grafana password:**
  ```bash
  docker exec -it <grafana_container_id> grafana-cli admin reset-admin-password NEWPASSWORD
  ```

---

## Contact / Credits
- Author: SickMedicine (Avto Sikharulidze)
- For questions, open an issue or contact via GitHub.

---

**Happy Monitoring!** 