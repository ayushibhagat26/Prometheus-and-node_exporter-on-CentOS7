# Prometheus-and-node_exporter-on-CentOS7
#### Update System
```
sudo yum update -y
```
#### Disable SELinux
```
sudo vim /etc/sysconfig/selinux
```
#### Change “SELINUX=enforcing” to “SELINUX=disabled”.
#### Change, save the file and then Reboot the System
```
sudo Reboot
```
#### Download Prometheus package
#### Go to official Prometheus downloads page, and copy the URL of the Linux “tar” file.
#### Run the following command to download the package. Paste the copied URL after wget in the below command:
``` 
wget https://github.com/prometheus/prometheus/releases/download/v2.17.2/prometheus-2.17.2.linux-amd64.tar.gz
```
#### Create a Prometheus user, required directories, and make Prometheus the user as the owner of those directories.
```
sudo useradd --no-create-home --shell /bin/false prometheus
```
```
sudo mkdir /etc/prometheus
```
```
sudo mkdir /var/lib/prometheus
```
```
sudo chown prometheus:prometheus /etc/prometheus
```
```
sudo chown prometheus:prometheus /var/lib/prometheus
```
#### Now go to Prometheus downloaded location and extract it.
```
sudo tar -xvzf prometheus-2.17.2.linux-amd64.tar.gz
```
#### Rename it as per your preference.
```
sudo mv prometheus-2.17.2.linux-amd64 prometheuspkg
```
#### Move the consoles and console_libraries directories from prometheuspkg to /etc/prometheus folder and change the ownership to prometheus user.
```
sudo cp -r prometheuspkg/consoles /etc/prometheus
```
```
sudo cp -r prometheuspkg/console_libraries /etc/prometheus
```
```
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
```
```
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
```
#### Copy prometheus and promtool binary from the prometheuspkg folder to /usr/local/bin
```
sudo cp prometheuspkg/prometheus /usr/local/bin/
```
```
sudo cp prometheuspkg/promtool /usr/local/bin/
```
#### Change the ownership to Prometheus user.
```
sudo chown prometheus:prometheus /usr/local/bin/prometheus
```
```
sudo chown prometheus:prometheus /usr/local/bin/promtool
```
#### Add and modify the Prometheus Configuration file.

#### Now we will create the prometheus.yml file
```
sudo vi /etc/prometheus/prometheus.yml

global:
  scrape_interval: 10s
  
scrape_configs: 
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090'] 
```
#### Change the ownership of the file.
```
sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml
```
#### Configure the Prometheus Service File.
```
sudo vi /etc/systemd/system/prometheus.service

[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
--config.file /etc/prometheus/prometheus.yml \
--storage.tsdb.path /var/lib/prometheus/ \
--web.console.templates=/etc/prometheus/consoles \
--web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```
Save and the exit file

### Reload the systemd service to register the Prometheus service and start the Prometheus service and check the status
```
sudo systemctl daemon-reload
```
```
sudo systemctl start prometheus
```
```
sudo systemctl status prometheus
```
#### Access Prometheus Web UI

#### Now you will be able to access the Prometheus UI on 9090 port of the Prometheus server. Make sure that port 9090 is open for the web interface.
```
http://<prometheus-ip>:9090/graph
```
## Configure Node Exporter on Centos

#### node_exporter is an exporter of machine metrics that can run on *Nix and Linux system.

In this tutorial, we will install the node_exporter on server2. We will monitor and get the metric of the server2.
 
### Go to the official Prometheus downloads section and download the latest.
```
wgethttps://github.com/prometheus/node_exporter/releases/download/v1.0.0-rc.0/node_exporter-1.0.0-rc.0.linux-amd64.tar.gz
```
### Extract the downloaded package.
```
sudo tar -xzf node_exporter-1.0.0-rc.0.linux-amd64.tar.gz
```
### create a user for node exporter and move binary to /usr/local/bin
```
sudo useradd -rs /bin/false nodeusr

```
```
sudo mv node_exporter-1.0.0-rc.0.linux-amd64/node_exporter /usr/local/bin/
```
### Create a node_exporter service file under systemd
```
sudo vi /etc/systemd/system/node_exporter.service
```
[Unit] 
Description=Node Exporter 
After=network.target
 
[Service]
User=nodeusr 
Group=nodeusr 
Type=simple 
ExecStart=/usr/local/bin/node_exporter

[Install] 
WantedBy=multi-user.target










