
# bnc-playground

This project helps bootstraping tools for Hyperledger Fabric and Ethereum Blockchains.

## Monitoring

The folder `resources` contains default config for monitoring of HLF with Grafana dashboards.
You can start the stack using the following commands:
* Run the exporter of the host machine metrics
```
docker run -d --name node_exporter --network bnc_network prom/node-exporter
```
* Run the exporter of the docker containers metrics
```
docker run -d --name cadvisor --network bnc_network -v /:/rootfs:ro -v /var/run:/var/run:rw -v /sys:/sys:ro -v /var/lib/docker:/var/lib/docker:ro -p 8080:8080 gcr.io/cadvisor/cadvisor:v0.38.4
```
* Run prometheus to gather metrics
```
docker run -d --name prometheus --network bnc_network -v $PWD/resources/prometheus.yml:/etc/prometheus/prometheus.yml -p 9090:9090 prom/prometheus --config.file=/etc/prometheus/prometheus.yml
```
* Run prometheus to gather metrics
```
docker run -d --name grafana --network bnc_network -p 3000:3000 -v grafana-data:/var/lib/grafana \
    -v $PWD/resources/grafana/datasource.yml:/etc/grafana/provisioning/datasources/datasource.yml \
	-v $PWD/resources/grafana/dashboards.yml:/etc/grafana/provisioning/dashboards/dashboards.yml \
	-v $PWD/resources/grafana/grafana.ini:/etc/grafana/grafana.ini \
	-v $PWD/resources/grafana/dashboards:/var/lib/grafana/dashboards \
        grafana/grafana:6.5.0
```

