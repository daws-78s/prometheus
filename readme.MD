# Prometheus

![alt text](prometheus.svg)

* Prometheus architecture contains master and node components. 
* It basically follows pull model. 
* Server periodically connects to nodes and fetch the metrics. 
* Node Exporter component in the servers are responsible to collect metrics from underlying server.

First let's make the nodes ready with exporters. Let's create 2 EC2 instances of t3.micro.

You can find automation scripts in this repo.

**NOTE: Make sure you are in /home/ec2-user/prometheus directory to run the script**

## Node Exporter (Linux)

First download the node exporter. Take the root access.

```
sudo su -
```
Move to opt directory.

```
cd /opt
```

Download Node exporter.

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
```

Extract

```
tar -xf https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
```

Rename

```
mv node_exporter-1.8.2.linux-amd64 node_exporter
```

Create a systemctl service

```
vim /etc/systemd/system/node_experter.service
```

copy the below content

```
[Unit]
Description=Node Exporter
After=network-online.target

[Service]
Restart=on-failure
ExecStart=/opt/node_exporter/node_exporter

[Install]
WantedBy=multi-user.target
```

Reload the daemon

```
systemctl daemon-reload
```

Enable the service

```
systemctl enable node_exporter
```

Start the service

```
systemctl start node_exporter
```