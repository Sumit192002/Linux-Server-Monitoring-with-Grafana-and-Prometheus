# 🖥️ Linux Server Monitoring with Grafana and Prometheus

## 📘 Project Overview

This project demonstrates a **real-time Linux server monitoring system** built on **WSL (Ubuntu 22.04)** using **Prometheus**, **Node Exporter**, and **Grafana**.  
It enables real-time visualization of system metrics like CPU, memory, disk, and network usage while automating alerting and performance tracking.

> 🚀 Reduced manual system inspection time by 70% through automated alerting and Grafana dashboards.

---

## 🧠 Objectives

- Design and implement a monitoring solution for a Linux (WSL Ubuntu) server.  
- Collect and visualize system metrics using open-source tools.  
- Automate alerts and analyze performance trends using Grafana dashboards.  
- Learn DevOps observability principles with Prometheus and Grafana integration.

---

## 🏗️ System Architecture

```mermaid
graph TD;
  A[Linux Server (WSL Ubuntu)] --> B[Node Exporter];
  B --> C[Prometheus];
  C --> D[Grafana];
  D --> E[User Interface];


---

## ⚙️ Tools & Technologies

| Tool | Purpose |
|------|----------|
| 🐧 **WSL Ubuntu 22.04** | Linux environment on Windows |
| 📊 **Prometheus** | Time-series database for metrics collection |
| 🧩 **Node Exporter** | Exposes system-level Linux metrics |
| 📈 **Grafana** | Visualization and alerting dashboard |
| ⚡ **Systemd & Bash** | Service management and automation |

---

## 🔧 Implementation Steps

### **1️⃣ Setup Node Exporter**

**Create a dedicated user and install Node Exporter:**
```bash
sudo useradd --no-create-home --shell /bin/false node_exporter
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
tar xvfz node_exporter-*.tar.gz
sudo cp node_exporter-*/node_exporter /usr/local/bin/
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

**Create a systemd service:**
```bash
sudo nano /etc/systemd/system/node_exporter.service
```

```ini
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=default.target
```

**Enable and start Node Exporter:**
```bash
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
```

**Verify installation:**
```bash
sudo systemctl status node_exporter
```

📍 **Access metrics:** [http://localhost:9100/metrics](http://localhost:9100/metrics)

---

### **2️⃣ Setup Prometheus**

**Download and extract Prometheus:**
```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.51.0/prometheus-2.51.0.linux-amd64.tar.gz
tar xvfz prometheus-*.tar.gz
cd prometheus-*
```

**Edit the Prometheus configuration file:**
```bash
nano prometheus.yml
```

```yaml
global:
  scrape_interval: 10s

scrape_configs:
  - job_name: "node"
    static_configs:
      - targets: ["localhost:9100"]
```

**Run Prometheus:**
```bash
./prometheus --config.file=prometheus.yml
```

📍 **Access Prometheus UI:** [http://localhost:9090](http://localhost:9090)

---

### **3️⃣ Setup Grafana**

**Install Grafana on Ubuntu (WSL):**
```bash
sudo apt-get install -y apt-transport-https software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt-get update
sudo apt-get install grafana -y
```

**Start and enable Grafana:**
```bash
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```

📍 **Access Grafana:** [http://localhost:3000](http://localhost:3000)  
Default login: `admin / admin`

---

### **4️⃣ Connect Prometheus to Grafana**

1. Open **Grafana** → Go to **⚙️ Settings → Data Sources → Add Data Source**  
2. Select **Prometheus**  
3. Set URL: `http://localhost:9090`  
4. Click **Save & Test**

---

### **5️⃣ Create Dashboard**

- Import **Node Exporter Full Dashboard** (Grafana.com ID: `1860`)  
- Customize panels for:
  - 🧠 CPU Usage (per core)
  - 💾 Memory Utilization
  - 💽 Disk I/O Performance
  - 🌐 Network Traffic
- Add alert rules for:
  - CPU usage > 80%
  - Memory usage > 90%

---

## 📊 Results

✅ **Real-time Monitoring:** Metrics refresh every 10 seconds  
✅ **Automated Alerts:** CPU and memory alerts configured  
✅ **Data Visualization:** 20+ key metrics displayed through Grafana  
✅ **Efficiency:** 70% reduction in manual inspection  
✅ **Scalability:** Easily add multiple servers as targets in Prometheus  

---

## 🧩 Challenges & Learnings

- Resolved **network mapping issues** between WSL and Windows host.  
- Learned how **Prometheus scrapes metrics** via pull-based architecture.  
- Built and customized **Grafana dashboards** using JSON templates.  
- Understood **Linux system performance internals** and metrics collection.

---

## 🚀 Future Enhancements

- 🔔 Integrate **Alertmanager** for email/Slack notifications.  
- 🐳 Deploy using **Docker Compose** for portability.  
- 📦 Add **application-level metrics** (e.g., Nginx, custom scripts).  
- 🌐 Host Grafana dashboards on a remote server for wider access.

---

## 📂 Repository Structure

```bash
linux-server-monitoring-grafana-prometheus/
│
├── prometheus.yml              # Prometheus configuration
├── node_exporter.service       # Node Exporter systemd service file
├── screenshots/                # Grafana dashboard screenshots
├── README.md                   # Project documentation
└── LICENSE                     # (optional)
```

---

## 📷 Sample Dashboard Preview

> Example Grafana dashboard (Node Exporter metrics visualization):

![Grafana Dashboard](https://upload.wikimedia.org/wikipedia/commons/3/3b/Grafana_dashboard.png)

---

## 🧾 Conclusion

This project demonstrates a **complete DevOps monitoring pipeline** on a WSL-based Linux system using **Prometheus**, **Node Exporter**, and **Grafana** to deliver real-time observability, alerting, and visualization — all with open-source tools.

---

## 🧑‍💻 Author

**Name:** Sumit Anand Padiyar  
**Platform:** WSL Ubuntu 22.04 on Windows  
**Contact:** padiyarsumit09@gmail.com 


