
# ELK (Elasticsearch_Logstash_Kibana)
Providing a comprehensive solution for data ingestion, storage, analysis, and visualization.


The ELK Stack is used for log and event data management, providing a comprehensive solution for data ingestion, storage, analysis, and visualization. It is widely used for monitoring, troubleshooting, and gaining insights from various types of data, including application logs, system metrics, and more.


## Docker_Compose.yaml

```javascript
services:
  elasticsearch:
    image: elasticsearch:7.14.2
    container_name: es
    environment:
      discovery.type: single-node
      ES_JAVA_OPTS: "-Xms512m -Xmx512m"     
    ports:
      - "9200:9200"
      - "9300:9300"
    healthcheck:
      test: ["CMD-SHELL", "curl --silent --fail localhost:9200/_cluster/health || exit 1"]
      interval: 10s
      timeout: 10s
      retries: 3
    networks:
      - elastic
  logstash:
    image: logstash:7.14.2
    container_name: log
    environment:
      discovery.seed_hosts: logstash
      LS_JAVA_OPTS: "-Xms512m -Xmx512m"
    volumes:
      - ./logstash/pipeline/logstash-nginx.config:/usr/share/logstash/pipeline/logstash-nginx.config
      - ./logstash/nginx.log:/home/nginx.log
    ports:
      - "5000:5000/tcp"
      - "5000:5000/udp"
      - "5044:5044"
      - "9600:9600"
    depends_on:
      - elasticsearch
    networks:
      - elastic
    command: logstash -f /usr/share/logstash/pipeline/logstash-nginx.config
  kibana:
    image: kibana:7.14.2
    container_name: kib
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch
    networks:
      - elastic
networks:
  elastic:
    driver: bridge
```


## Run Locally

Clone the project

```bash
  git clone https://github.com/Tarun-Tiwari10052001/ELK-Elasticsearch-Logstash-Kibana.git
```

Go to the ELK-Elasticsearch-Logstash-Kibana repo

```bash
  cd ELK-Elasticsearch-Logstash-Kibana
```

Run command


```bash
  docker-compose up -d
```
## Screenshots

![App Screenshot](https://github.com/Tarun-Tiwari10052001/Aws_file/blob/master/ELK_docker_compose_up.png)



Run command to check container working in proper way

```bash
  docker-compose ps
```

To check your local machine ip add

```bash
  ip add show wlp1s0
```
Output (in my case)

```bash
  wlp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether e4:02:9b:66:2f:47 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.151/24 brd 192.168.0.255 scope global dynamic noprefixroute wlp1s0
       valid_lft 5018sec preferred_lft 5018sec
    inet6 fe80::8cbb:184a:e2b3:7372/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```
Note the ip (in my case)

```bash
  192.168.0.151
```
and port 5601

## Screenshots

![App Screenshot](https://github.com/Tarun-Tiwari10052001/Aws_file/blob/master/elk_2.png)



# Browse 


1. Open you favourite Browser 
2. Put the local ip add with port you provided     (in my case i provide port 5601 )

## Screenshots
ENJOY 

![App Screenshot](https://github.com/Tarun-Tiwari10052001/Aws_file/blob/master/ELK_features1.png)

