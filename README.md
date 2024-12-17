### Context
While working with a customer in an air-gapped environment, I noticed that the ```./o11y-save-images.sh -i o11y-images.txt -f o11y-images.tar.gz``` script takes a significant amount of time to pull images and create the .tar file. To address this, I modified the script to pull images in parallel.

Pulling images with 4 concurrent pulls and compressing zip with fastest compression(gzip -1).

This will reduce the time needed for pulling and compressing images by **~40%.**  
This improvement has been tested on a Droplet.

Please test it and let me know if adding concurrent image pulling to the script would be worthwhile.

#### With concurrent pulling images.
 ```bash
root@smtpstackstate:~# time ./o11y-save-images_v1.sh -i o11y-images.txt -f o11y-images.tar.gz
xargs: warning: options --max-args and --replace/-I/-i are mutually exclusive, ignoring previous --max-args value
Image pull success: registry.rancher.com/suse-observability/elasticsearch-exporter:v1.7.0-03d6f56d
Image pull success: registry.rancher.com/suse-observability/clickhouse-backup:2.5.20-2b2c95ed
Image pull success: registry.rancher.com/suse-observability/envoy:v1.19.1-e418b2bd
Image pull success: registry.rancher.com/suse-observability/hadoop:3.4.1-java11-8-90a9d727
Image pull success: registry.rancher.com/suse-observability/hbase-master:2.5-7.8.2
Image pull success: registry.rancher.com/suse-observability/hbase-regionserver:2.5-7.8.2
Image pull success: registry.rancher.com/suse-observability/clickhouse:23.8.13-debian-12-r0-b9530c97
Image pull success: registry.rancher.com/suse-observability/jmx-exporter:0.17.0-129c430a
Image pull success: registry.rancher.com/suse-observability/kafka:3.3.1-08305c25
Image pull success: registry.rancher.com/suse-observability/kafkaup-operator:0.0.3
Image pull success: registry.rancher.com/suse-observability/elasticsearch:8.11.4-cf68e2fa
Image pull success: registry.rancher.com/suse-observability/nginx-prometheus-exporter:1.1.0-6743974546
Image pull success: registry.rancher.com/suse-observability/minio:RELEASE.2021-04-22T15-44-28Z-7f17e5ba
Image pull success: registry.rancher.com/suse-observability/stackgraph-hbase:2.5-7.8.2
Image pull success: registry.rancher.com/suse-observability/stackgraph-console:2.5-7.8.2
Image pull success: registry.rancher.com/suse-observability/stackpacks:20241112130618-master-3aa249e-prime-selfhosted
Image pull success: registry.rancher.com/suse-observability/stackstate-correlate:7.0.0-snapshot.20241204151219-master-db9515b
Image pull success: registry.rancher.com/suse-observability/stackstate-kafka-to-es:7.0.0-snapshot.20241204151219-master-db9515b
Image pull success: registry.rancher.com/suse-observability/spotlight:5.2.0-snapshot.143
Image pull success: registry.rancher.com/suse-observability/stackstate-receiver:7.0.0-snapshot.20241204151219-master-db9515b
Image pull success: registry.rancher.com/suse-observability/stackstate-server:7.0.0-snapshot.20241204151219-master-db9515b-2.5
Image pull success: registry.rancher.com/suse-observability/stackstate-ui:7.0.0-snapshot.20241204151219-master-db9515b
Image pull success: registry.rancher.com/suse-observability/sts-opentelemetry-collector:v0.0.15
Image pull success: registry.rancher.com/suse-observability/victoria-metrics:v1.93.14-e17e24af
Image pull success: registry.rancher.com/suse-observability/vmagent:v1.93.14-f69ecbeb
Image pull success: registry.rancher.com/suse-observability/container-tools:1.4.1
Image pull success: registry.rancher.com/suse-observability/tephra-server:2.5-7.8.2
Image pull success: registry.rancher.com/suse-observability/wait:1.0.10-025450d9
Image pull success: registry.rancher.com/suse-observability/vmbackup:v1.93.7-b11ba275
Image pull success: registry.rancher.com/suse-observability/zookeeper:3.8.4-c7c0422c
Creating o11y-images.tar.gz with 30 images
Images saved to o11y-images.tar.gz
real    10m19.094s
user    5m34.054s
sys     0m51.492s
root@smtpstackstate:~#
```
#### With Normal Process
```bash
root@smtpstackstate:~# time ./o11y-save-images.sh -i o11y-images.txt -f o11y-images.tar.gz
Image pull success: registry.rancher.com/suse-observability/clickhouse-backup:2.5.20-2b2c95ed
Image pull success: registry.rancher.com/suse-observability/clickhouse:23.8.13-debian-12-r0-b9530c97
Image pull success: registry.rancher.com/suse-observability/container-tools:1.4.1
Image pull success: registry.rancher.com/suse-observability/elasticsearch-exporter:v1.7.0-03d6f56d
Image pull success: registry.rancher.com/suse-observability/elasticsearch:8.11.4-cf68e2fa
Image pull success: registry.rancher.com/suse-observability/envoy:v1.19.1-e418b2bd
Image pull success: registry.rancher.com/suse-observability/hadoop:3.4.1-java11-8-90a9d727
Image pull success: registry.rancher.com/suse-observability/hbase-master:2.5-7.8.2
Image pull success: registry.rancher.com/suse-observability/hbase-regionserver:2.5-7.8.2
Image pull success: registry.rancher.com/suse-observability/jmx-exporter:0.17.0-129c430a
Image pull success: registry.rancher.com/suse-observability/kafka:3.3.1-08305c25
Image pull success: registry.rancher.com/suse-observability/kafkaup-operator:0.0.3
Image pull success: registry.rancher.com/suse-observability/minio:RELEASE.2021-04-22T15-44-28Z-7f17e5ba
Image pull success: registry.rancher.com/suse-observability/nginx-prometheus-exporter:1.1.0-6743974546
Image pull success: registry.rancher.com/suse-observability/spotlight:5.2.0-snapshot.143
Image pull success: registry.rancher.com/suse-observability/stackgraph-console:2.5-7.8.2
Image pull success: registry.rancher.com/suse-observability/stackgraph-hbase:2.5-7.8.2
Image pull success: registry.rancher.com/suse-observability/stackpacks:20241112130618-master-3aa249e-prime-selfhosted
Image pull success: registry.rancher.com/suse-observability/stackstate-correlate:7.0.0-snapshot.20241204151219-master-db9515b
Image pull success: registry.rancher.com/suse-observability/stackstate-kafka-to-es:7.0.0-snapshot.20241204151219-master-db9515b
Image pull success: registry.rancher.com/suse-observability/stackstate-receiver:7.0.0-snapshot.20241204151219-master-db9515b
Image pull success: registry.rancher.com/suse-observability/stackstate-server:7.0.0-snapshot.20241204151219-master-db9515b-2.5
Image pull success: registry.rancher.com/suse-observability/stackstate-ui:7.0.0-snapshot.20241204151219-master-db9515b
Image pull success: registry.rancher.com/suse-observability/sts-opentelemetry-collector:v0.0.15
Image pull success: registry.rancher.com/suse-observability/tephra-server:2.5-7.8.2
Image pull success: registry.rancher.com/suse-observability/victoria-metrics:v1.93.14-e17e24af
Image pull success: registry.rancher.com/suse-observability/vmagent:v1.93.14-f69ecbeb
Image pull success: registry.rancher.com/suse-observability/vmbackup:v1.93.7-b11ba275
Image pull success: registry.rancher.com/suse-observability/wait:1.0.10-025450d9
Image pull success: registry.rancher.com/suse-observability/zookeeper:3.8.4-c7c0422c
Creating o11y-images.tar.gz with 30 images
Images saved to o11y-images.tar.gz
real    17m0.698s
user    10m42.583s
sys     1m1.701s
root@smtpstackstate:~#
```
Also I don't see a significant difference in performance when pulling images in parallel using `docker pull`. Therefore, I think it’s better to pull the images in parallel.See the below graph for reference.

We can also adjust the number of concurrent image pulls as needed.
### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  Without concurrent docker pull   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;With 4 concurrent docker pull
![image](https://github.com/user-attachments/assets/3653e28b-9e14-48f3-9aa5-ac10ded23ff1)
![image](https://github.com/user-attachments/assets/5d89e64d-5324-441c-ac01-c0a197c12e50)
![image](https://github.com/user-attachments/assets/d3b86478-b214-4213-a699-768de158e71f)


