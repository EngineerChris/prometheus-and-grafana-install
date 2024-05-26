
# Setting Up Prometheus and Grafana

## EC2 Instances (Servers) Set Up

### Prometheus EC2 Instance

- AMI: Ubuntu 22.04
- Instance Type: t2.micro
- Key Pair: Choose or create in the appropriate region
- Security Group:
  - Port 9090: Open to 0.0.0.0/0 (anywhere)
  - Port 22 (SSH): Open to 0.0.0.0/0 (anywhere)
- IAM Role: Attach a role with full access permissions to EC2

### Grafana EC2 Instance

- AMI: Ubuntu 22.04
- Instance Type: t2.micro
- Key Pair: Choose or create
- Security Group:
  - Port 3000: Open to 0.0.0.0/0 (anywhere)
  - Port 22 (SSH): Open to 0.0.0.0/0 (anywhere)

### Target Servers EC2 Instances (2 Instances)

- AMI: Ubuntu 22.04
- Instance Type: t2.micro
- Key Pair: Choose or create
- Security Group:
  - Port 9100: Open to 0.0.0.0/0 (Node Exporter)
  - Port 22 (SSH): Open to 0.0.0.0/0 (anywhere)
- Rename these instances to Web-Server1 and Web-Server2.

## Configuration of Servers

### Prometheus Server Installation and Configuration

- SSH to Prometheus server
- Clone git repo: `git clone https://github.com/EngineerChris/prometheus-and-grafana-install`
- Navigate to the directory: `cd service-discovery `
- Run `bash install-prometheus.sh` to install Prometheus

### Grafana Installation and Configuration

- SSH to Grafana server
- Clone the same repo: `git clone https://github.com/EngineerChris/prometheus-and-grafana-install`
- Navigate to the directory: `cd prometheus-and-grafana-install` (MAY NOT BE NECESSARY)
- Install Grafana: `bash install-grafana.sh`

### Target Server Installation and Configuration

#### Webser 1 (Repeat for Web-Server2)

- SSH to the Target instance (Server1)
- Clone the project repo: `git clone https://github.com/EngineerChris/prometheus-and-grafana-install`
- Navigate to the directory: `cd prometheus-and-grafana-install`  (MAY NOT BE NECESSARY)
- Install Node Exporter: `bash install-node-exporter.sh`

## Check Access to Servers UI

- Prometheus: `http://<prometheus-server-ip>:9090`
- Grafana: `http://<grafana-server-ip>:3000`
- Target Servers:
  - Web-Server1: `http://<web-server1-ip>:9100`
  - Web-Server2: `http://<web-server2-ip>:9100`

## Navigating Prometheus Server UI

- New UI: Use to run PromQL queries.
- Graph: Visualize metrics (Grafana provides a more robust solution).

## Grafana Server UI Configuration

- GrafanaServerIP:3000
- Sign-in: admin/admin


## Grafana UI Set up 
To see any visualizations in Grafana, there is a need to set up two things 
- Data Source 
- Dash Board for Visualization 

### Setting Up Data Source (Prometheus)

- Go to Settings (gear icon)
- Navigate to Data Sources
- Click on Add Data Source
- Add Prometheus Data Source:
  - Select Prometheus
  - Provide the Prometheus URL: `http://<PromServerIP>:9090`

## Setting Up Data Dashboard Visualization
There are two methods to populate Grafana Data Dashboard: 
1. Using Dashboard ID (may sometimes not work) 
2. Download Dashboard JSON File and Upload (the method used in this project) 
   - Download and Upload Dashboard JSON File: Click to download: Dashboard JSON File 
   - Upload the Dashboard File and Connect to Prometheus Data Source: 
      - Hover over the + sign on the left panel 
      - Select Import 
      - Click Upload JSON File and choose the downloaded file (1860_rev25.json) 
      - Choose Prometheus as the Data Source 
      - Click Import to complete the setup 
This setup will connect your dashboard to the Prometheus data source for visualization. 


## Stress the Webserver (Target Server)

- SSH to webser1 (that has node exporter installed that Prometheus is scaping metrics )
- Install stress tool: `sudo apt update && sudo apt-get install stress -y`
- Run stress test: `stress --cpu 5 --timeout 360s`

## Visualize the Grafana Dashboard Again

- After running the stress test, go back to the Grafana dashboard to visualize the updated metrics.
