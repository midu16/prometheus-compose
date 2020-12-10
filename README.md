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

The disk space used by the prometheus databases is located under the ```/var/lib/prometheus/```:
```
[root@wien-ostechnix-lan ~]# du -sh /var/lib/prometheus/
400M	/var/lib/prometheus/


[midu@wien-ostechnix-lan prometheus-compose]$ sudo df -h --total
overlay                   150G   23G  128G  16% /var/lib/docker/overlay2/4e598aa600879ab3a462c680d21511c7f13be8a289096ac13dcc6e35c2e5127e/merged
shm                        64M     0   64M   0% /var/lib/docker/containers/334ac41d3c7797ee715e1b0cd588f369611f2351113681d4cea0c3ab891a4c6d/mounts/shm
```

```The values may differ in reality and vary by version```


## Logs

- First, we check the logs of the docker to see if the container is up and running without any errors or warnings:
```
[midu@wien-ostechnix-lan prometheus-compose]$ docker logs prometheus
level=info ts=2020-12-10T17:34:53.510Z caller=main.go:322 msg="No time or size retention was set so using the default time retention" duration=15d
level=info ts=2020-12-10T17:34:53.510Z caller=main.go:360 msg="Starting Prometheus" version="(version=2.23.0, branch=HEAD, revision=26d89b4b0776fe4cd5a3656dfa520f119a375273)"
level=info ts=2020-12-10T17:34:53.510Z caller=main.go:365 build_context="(go=go1.15.5, user=root@37609b3a0a21, date=20201126-10:56:17)"
level=info ts=2020-12-10T17:34:53.510Z caller=main.go:366 host_details="(Linux 4.18.0-193.28.1.el8_2.x86_64 #1 SMP Thu Oct 22 00:20:22 UTC 2020 x86_64 prometheus (none))"
level=info ts=2020-12-10T17:34:53.510Z caller=main.go:367 fd_limits="(soft=1048576, hard=1048576)"
level=info ts=2020-12-10T17:34:53.510Z caller=main.go:368 vm_limits="(soft=unlimited, hard=unlimited)"
level=info ts=2020-12-10T17:34:53.515Z caller=main.go:722 msg="Starting TSDB ..."
level=info ts=2020-12-10T17:34:53.515Z caller=web.go:528 component=web msg="Start listening for connections" address=0.0.0.0:9090
level=info ts=2020-12-10T17:34:53.524Z caller=head.go:645 component=tsdb msg="Replaying on-disk memory mappable chunks if any"
level=info ts=2020-12-10T17:34:53.524Z caller=head.go:659 component=tsdb msg="On-disk memory mappable chunks replay completed" duration=5.056µs
level=info ts=2020-12-10T17:34:53.524Z caller=head.go:665 component=tsdb msg="Replaying WAL, this may take a while"
level=info ts=2020-12-10T17:34:53.527Z caller=head.go:717 component=tsdb msg="WAL segment loaded" segment=0 maxSegment=1
level=info ts=2020-12-10T17:34:53.527Z caller=head.go:717 component=tsdb msg="WAL segment loaded" segment=1 maxSegment=1
level=info ts=2020-12-10T17:34:53.527Z caller=head.go:722 component=tsdb msg="WAL replay completed" checkpoint_replay_duration=60.356µs wal_replay_duration=3.376416ms total_replay_duration=3.479135ms
level=info ts=2020-12-10T17:34:53.530Z caller=main.go:742 fs_type=XFS_SUPER_MAGIC
level=info ts=2020-12-10T17:34:53.530Z caller=main.go:745 msg="TSDB started"
level=info ts=2020-12-10T17:34:53.530Z caller=main.go:871 msg="Loading configuration file" filename=/etc/prometheus/prometheus.yml
level=info ts=2020-12-10T17:34:53.532Z caller=main.go:902 msg="Completed loading of configuration file" filename=/etc/prometheus/prometheus.yml totalDuration=2.275402ms remote_storage=3.7µs web_handler=616ns query_engine=1.544µs scrape=1.654755ms scrape_sd=126.28µs notify=1.92µs notify_sd=3.753µs rules=20.151µs
level=info ts=2020-12-10T17:34:53.532Z caller=main.go:694 msg="Server is ready to receive web requests."
```
Note: As you can see in the output above, the container is "readz to receive web requests" and is listening to all the egress traffic.

- Second, we are gonna check and valid if the customizations are in place:
```
[midu@wien-ostechnix-lan prometheus-compose]$ docker exec -it prometheus hostname
prometheus
```
Note: The output above is validating that the docker hostname is as the one set into the docker-compose.yml.

- Third, we are gonna check the filesystem structure of the docker:
```
[midu@wien-ostechnix-lan prometheus-compose]$ docker exec -it prometheus df -h
Filesystem                Size      Used Available Use% Mounted on
overlay                 149.9G     22.7G    127.2G  15% /
tmpfs                    64.0M         0     64.0M   0% /dev
tmpfs                     3.7G         0      3.7G   0% /sys/fs/cgroup
/dev/mapper/cl-root     149.9G     22.7G    127.2G  15% /prometheus
/dev/mapper/cl-root     149.9G     22.7G    127.2G  15% /etc/resolv.conf
/dev/mapper/cl-root     149.9G     22.7G    127.2G  15% /etc/hostname
/dev/mapper/cl-root     149.9G     22.7G    127.2G  15% /etc/hosts
shm                      64.0M         0     64.0M   0% /dev/shm
/dev/mapper/cl-home      64.2G      4.1G     60.1G   6% /etc/prometheus/prometheus.yml
tmpfs                     3.7G         0      3.7G   0% /proc/asound
tmpfs                     3.7G         0      3.7G   0% /proc/acpi
tmpfs                    64.0M         0     64.0M   0% /proc/kcore
tmpfs                    64.0M         0     64.0M   0% /proc/keys
tmpfs                    64.0M         0     64.0M   0% /proc/timer_list
tmpfs                    64.0M         0     64.0M   0% /proc/sched_debug
tmpfs                     3.7G         0      3.7G   0% /proc/scsi
tmpfs                     3.7G         0      3.7G   0% /sys/firmware
```
Note: the *overlay* disk attached to the container is 150GB.

## Documentation
1. https://prometheus.io/docs/guides/tls-encryption/
2. https://docs.nginx.com/nginx-ingress-controller/overview/#what-is-the-ingress
3. https://prometheus.io/docs/prometheus/latest/storage/
4. https://wiki.opnfv.org/display/fastpath/Open+vSwitch+-+Provisioning+and+Monitoring+with+vswitch+Extended+NIC+stats+-+User+Guide
