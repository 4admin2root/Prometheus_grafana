# Prometheus_grafana
## what
### overview
https://prometheus.io/docs/introduction/overview/   
http://docs.grafana.org/

### architecture
https://prometheus.io/assets/architecture.svg


## how

### start prometheus

* create config file
```
cat config/prometheus.yml
```

* download docker image
```
docker pull prom/prometheus:v2.0.0
```

* run 
```

docker run -d --name prom -p 9090:9090 -v /tmp/prometheus.yml:/etc/prometheus/prometheus.yml  prom/prometheus:v2.0.0
# for mac
docker run -d --name prom -p 9090:9090 -v /tmp/prometheus-mac.yml:/etc/prometheus/prometheus.yml  prom/prometheus:v2.0.0
```

* view prometheus metircs
```
curl localhost:9090/metrics
```

* browser prometheus web   
http://localhost:9090/graph



### start exporter
* node exporter
```
# mac
wget https://github.com/prometheus/node_exporter/releases/download/v0.15.0/node_exporter-0.15.0.darwin-amd64.tar.gz  && tar -xvf node_exporter-0.15.0.darwin-amd64.tar.gz

# linux server
wget https://github.com/prometheus/node_exporter/releases/download/v0.15.0/node_exporter-0.15.0.linux-amd64.tar.gz && tar -xvf node_exporter-0.15.0.linux-amd64.tar.gz 

# run
# ./node_exporter -h
nohup ./node_exporter &

curl http://localhost:9100/metrics
```

 node exporter (docker , not not recommended)
```
# linux server
docker run -d -p 9100:9100 \
  -v "/proc:/host/proc:ro" \
  -v "/sys:/host/sys:ro" \
  -v "/:/rootfs:ro" \
  --net="host" \
  quay.io/prometheus/node-exporter:v0.15.0 \
    --path.procfs /host/proc \
    --path.sysfs /host/sys \
    --collector.filesystem.ignored-mount-points "^/(sys|proc|dev|host|etc)($|/)"
```

* cadivsor
```
sudo docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  google/cadvisor:latest
  
curl http://localhost:8080/metrics
```

* browse prometheus    
http://localhost:9090/targets


* java

### start grafana
* run
```
docker run -d --name grafana -p 3000:3000 grafana/grafana:4.4.3
sleep 10
curl localhost:3000
```
* config data sources   
browse localhost:3000 with username 'admin' and password 'admin'   
name: default
type: prometheus
url : http://172.17.0.1:9090    
    for mac : http://docker.for.mac.host.internal:9090

* dashboard id
host stats : 718    
node expoter server metris : 405   
docker and system monitoring : 893

