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



