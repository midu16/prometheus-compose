# Description of prometheus-compose
docker-compose.yml file for the prometheus


The image used for the docker is the  upstream ```prom/prometheus:latest``` .

The image is maintained at ```https://hub.docker.com/r/prom/prometheus```.



For running the
```
$ docker-compose up -d
```

In order to calculate the amount of RAM memory used check the following ```https://www.robustperception.io/how-much-ram-does-prometheus-2-x-need-for-cardinality-and-ingestion```

Example values:
```
Scrape interval: 30 seconds
Bytes per Sample: 2

Results in the following results:
    - Samples per Second: 33.4k/s
    - Ingestion Memory: 1,374MiB
Combined Memory: 3,460MiB 


Scrape interval: 30 seconds
Bytes per Sample: 4

Results in the following results:
    - Samples per Second: 33.4k/s
    - Ingestion Memory: 2,747MiB
Combined Memory: 4,833MiB
```

```The values may differ in reality and vary by version```



## Documentation
1. https://prometheus.io/docs/guides/tls-encryption/
2. https://docs.nginx.com/nginx-ingress-controller/overview/#what-is-the-ingress
