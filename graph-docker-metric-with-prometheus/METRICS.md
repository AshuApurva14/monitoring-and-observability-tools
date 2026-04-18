# Graph Docker Metrics with Prometheus

---

## Step 1 - Enable Metrics

The first task is to enable the new experimental flag that will enable the metrics feature and define the metrics address to listen on localhost:9323.

The command below will update the systemd configuration used to start Docker to set the flags when the daemon starts and then restarts Docker.

```
echo '{ "metrics-addr" : "127.0.0.1:9323", "experimental" : true }' > /etc/docker/daemon.json
systemctl restart docker

# Below commands

root@host01:~$ echo '{ "metrics-addr" : "127.0.0.1:9323", "experimental" : true }' > /etc/docker/daemon.json
root@host01:~$ systemctl restart docker
root@host01:~$ echo '{ "metrics-addr" : "127.0.0.1:9323", "experimental" : true }' > /etc/docker/daemon.json
root@host01:~$ sudo systemctl status docker
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2026-04-18 18:42:10 UTC; 33s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 4068 (dockerd)
      Tasks: 10
     Memory: 25.2M
     CGroup: /system.slice/docker.service
             └─4068 /usr/bin/dockerd

Apr 18 18:42:09 host01 dockerd[4068]: time="2026-04-18T18:42:09.851296943Z" level=info msg="detected 127.0.0.53 names>
Apr 18 18:42:09 host01 dockerd[4068]: time="2026-04-18T18:42:09.881797828Z" level=info msg="[graphdriver] using prior>
Apr 18 18:42:09 host01 dockerd[4068]: time="2026-04-18T18:42:09.882137231Z" level=info msg="Loading containers: start>
Apr 18 18:42:10 host01 dockerd[4068]: time="2026-04-18T18:42:10.172730625Z" level=info msg="Default bridge (docker0) >
Apr 18 18:42:10 host01 dockerd[4068]: time="2026-04-18T18:42:10.230102687Z" level=info msg="Loading containers: done."
Apr 18 18:42:10 host01 dockerd[4068]: time="2026-04-18T18:42:10.249858978Z" level=info msg="Docker daemon" commit=463>
Apr 18 18:42:10 host01 dockerd[4068]: time="2026-04-18T18:42:10.250224471Z" level=info msg="Daemon has completed init>

```

### Metrics

```
root@host01:~$ curl localhost:9323/metrics
# HELP builder_builds_failed_total Number of failed image builds
# TYPE builder_builds_failed_total counter
builder_builds_failed_total{reason="build_canceled"} 0
builder_builds_failed_total{reason="build_target_not_reachable_error"} 0
builder_builds_failed_total{reason="command_not_supported_error"} 0
builder_builds_failed_total{reason="dockerfile_empty_error"} 0
builder_builds_failed_total{reason="dockerfile_syntax_error"} 0
builder_builds_failed_total{reason="error_processing_commands_error"} 0
builder_builds_failed_total{reason="missing_onbuild_arguments_error"} 0
builder_builds_failed_total{reason="unknown_instruction_error"} 0
# HELP builder_builds_triggered_total Number of triggered image builds
# TYPE builder_builds_triggered_total counter
builder_builds_triggered_total 0
# HELP engine_daemon_container_actions_seconds The number of seconds it takes to process each container action
# TYPE engine_daemon_container_actions_seconds histogram
engine_daemon_container_actions_seconds_bucket{action="changes",le="0.005"} 1
engine_daemon_container_actions_seconds_bucket{action="changes",le="0.01"} 1
engine_daemon_container_actions_seconds_bucket{action="changes",le="0.025"} 1
engine_daemon_container_actions_seconds_bucket{action="changes",le="0.05"} 1
engine_daemon_container_actions_seconds_bucket{action="changes",le="0.1"} 1
engine_daemon_container_actions_seconds_bucket{action="changes",le="0.25"} 1
engine_daemon_container_actions_seconds_bucket{action="changes",le="0.5"} 1
engine_daemon_container_actions_seconds_bucket{action="changes",le="1"} 1
engine_daemon_container_actions_seconds_bucket{action="changes",le="2.5"} 1
engine_daemon_container_actions_seconds_bucket{action="changes",le="5"} 1
engine_daemon_container_actions_seconds_bucket{action="changes",le="10"} 1
engine_daemon_container_actions_seconds_bucket{action="changes",le="+Inf"} 1
engine_daemon_container_actions_seconds_sum{action="changes"} 0
engine_daemon_container_actions_seconds_count{action="changes"} 1
engine_daemon_container_actions_seconds_bucket{action="commit",le="0.005"} 1
engine_daemon_container_actions_seconds_bucket{action="commit",le="0.01"} 1
engine_daemon_container_actions_seconds_bucket{action="commit",le="0.025"} 1
engine_daemon_container_actions_seconds_bucket{action="commit",le="0.05"} 1
engine_daemon_container_actions_seconds_bucket{action="commit",le="0.1"} 1
engine_daemon_container_actions_seconds_bucket{action="commit",le="0.25"} 1
engine_daemon_container_actions_seconds_bucket{action="commit",le="0.5"} 1
engine_daemon_container_actions_seconds_bucket{action="commit",le="1"} 1
engine_daemon_container_actions_seconds_bucket{action="commit",le="2.5"} 1
engine_daemon_container_actions_seconds_bucket{action="commit",le="5"} 1
engine_daemon_container_actions_seconds_bucket{action="commit",le="10"} 1
engine_daemon_container_actions_seconds_bucket{action="commit",le="+Inf"} 1
engine_daemon_container_actions_seconds_sum{action="commit"} 0
engine_daemon_container_actions_seconds_count{action="commit"} 1
engine_daemon_container_actions_seconds_bucket{action="create",le="0.005"} 1
engine_daemon_container_actions_seconds_bucket{action="create",le="0.01"} 1
engine_daemon_container_actions_seconds_bucket{action="create",le="0.025"} 1
engine_daemon_container_actions_seconds_bucket{action="create",le="0.05"} 1
engine_daemon_container_actions_seconds_bucket{action="create",le="0.1"} 1
engine_daemon_container_actions_seconds_bucket{action="create",le="0.25"} 1
engine_daemon_container_actions_seconds_bucket{action="create",le="0.5"} 1
engine_daemon_container_actions_seconds_bucket{action="create",le="1"} 1
engine_daemon_container_actions_seconds_bucket{action="create",le="2.5"} 1
engine_daemon_container_actions_seconds_bucket{action="create",le="5"} 1
engine_daemon_container_actions_seconds_bucket{action="create",le="10"} 1
engine_daemon_container_actions_seconds_bucket{action="create",le="+Inf"} 1
engine_daemon_container_actions_seconds_sum{action="create"} 0
engine_daemon_container_actions_seconds_count{action="create"} 1
engine_daemon_container_actions_seconds_bucket{action="delete",le="0.005"} 1
engine_daemon_container_actions_seconds_bucket{action="delete",le="0.01"} 1
engine_daemon_container_actions_seconds_bucket{action="delete",le="0.025"} 1
engine_daemon_container_actions_seconds_bucket{action="delete",le="0.05"} 1
engine_daemon_container_actions_seconds_bucket{action="delete",le="0.1"} 1
engine_daemon_container_actions_seconds_bucket{action="delete",le="0.25"} 1
engine_daemon_container_actions_seconds_bucket{action="delete",le="0.5"} 1
engine_daemon_container_actions_seconds_bucket{action="delete",le="1"} 1
engine_daemon_container_actions_seconds_bucket{action="delete",le="2.5"} 1
engine_daemon_container_actions_seconds_bucket{action="delete",le="5"} 1
engine_daemon_container_actions_seconds_bucket{action="delete",le="10"} 1
engine_daemon_container_actions_seconds_bucket{action="delete",le="+Inf"} 1
engine_daemon_container_actions_seconds_sum{action="delete"} 0
engine_daemon_container_actions_seconds_count{action="delete"} 1
engine_daemon_container_actions_seconds_bucket{action="start",le="0.005"} 1
engine_daemon_container_actions_seconds_bucket{action="start",le="0.01"} 1
engine_daemon_container_actions_seconds_bucket{action="start",le="0.025"} 1
engine_daemon_container_actions_seconds_bucket{action="start",le="0.05"} 1
engine_daemon_container_actions_seconds_bucket{action="start",le="0.1"} 1
engine_daemon_container_actions_seconds_bucket{action="start",le="0.25"} 1
engine_daemon_container_actions_seconds_bucket{action="start",le="0.5"} 1
engine_daemon_container_actions_seconds_bucket{action="start",le="1"} 1
engine_daemon_container_actions_seconds_bucket{action="start",le="2.5"} 1
engine_daemon_container_actions_seconds_bucket{action="start",le="5"} 1
engine_daemon_container_actions_seconds_bucket{action="start",le="10"} 1
engine_daemon_container_actions_seconds_bucket{action="start",le="+Inf"} 1
engine_daemon_container_actions_seconds_sum{action="start"} 0
engine_daemon_container_actions_seconds_count{action="start"} 1
# HELP engine_daemon_container_states_containers The count of containers in various states
# TYPE engine_daemon_container_states_containers gauge
engine_daemon_container_states_containers{state="paused"} 0
engine_daemon_container_states_containers{state="running"} 0
engine_daemon_container_states_containers{state="stopped"} 0
# HELP engine_daemon_engine_cpus_cpus The number of cpus that the host system of the engine has
# TYPE engine_daemon_engine_cpus_cpus gauge
engine_daemon_engine_cpus_cpus 2
# HELP engine_daemon_engine_info The information related to the engine and the OS it is running on
# TYPE engine_daemon_engine_info gauge
engine_daemon_engine_info{architecture="x86_64",commit="463850e",daemon_id="1b41e836-9be3-4dd0-bd84-4d6ccab7da00",graphdriver="overlay2",kernel="5.15.0-72-generic",os="Ubuntu 22.04.2 LTS",os_type="linux",os_version="22.04",version="24.0.1"} 1
# HELP engine_daemon_engine_memory_bytes The number of bytes of memory that the host system of the engine has
# TYPE engine_daemon_engine_memory_bytes gauge
engine_daemon_engine_memory_bytes 1.505415168e+09
# HELP engine_daemon_events_subscribers_total The number of current subscribers to events
# TYPE engine_daemon_events_subscribers_total gauge
engine_daemon_events_subscribers_total 0
# HELP engine_daemon_events_total The number of events logged
# TYPE engine_daemon_events_total counter
engine_daemon_events_total 0
# HELP engine_daemon_health_check_start_duration_seconds The number of seconds it takes to prepare to run health checks
# TYPE engine_daemon_health_check_start_duration_seconds histogram
engine_daemon_health_check_start_duration_seconds_bucket{le="0.005"} 0
engine_daemon_health_check_start_duration_seconds_bucket{le="0.01"} 0
engine_daemon_health_check_start_duration_seconds_bucket{le="0.025"} 0
engine_daemon_health_check_start_duration_seconds_bucket{le="0.05"} 0
engine_daemon_health_check_start_duration_seconds_bucket{le="0.1"} 0
engine_daemon_health_check_start_duration_seconds_bucket{le="0.25"} 0
engine_daemon_health_check_start_duration_seconds_bucket{le="0.5"} 0
engine_daemon_health_check_start_duration_seconds_bucket{le="1"} 0
engine_daemon_health_check_start_duration_seconds_bucket{le="2.5"} 0
engine_daemon_health_check_start_duration_seconds_bucket{le="5"} 0
engine_daemon_health_check_start_duration_seconds_bucket{le="10"} 0
engine_daemon_health_check_start_duration_seconds_bucket{le="+Inf"} 0
engine_daemon_health_check_start_duration_seconds_sum 0
engine_daemon_health_check_start_duration_seconds_count 0
# HELP engine_daemon_health_checks_failed_total The total number of failed health checks
# TYPE engine_daemon_health_checks_failed_total counter
engine_daemon_health_checks_failed_total 0
# HELP engine_daemon_health_checks_total The total number of health checks
# TYPE engine_daemon_health_checks_total counter
engine_daemon_health_checks_total 0
# HELP engine_daemon_host_info_functions_seconds The number of seconds it takes to call functions gathering info about the host
# TYPE engine_daemon_host_info_functions_seconds histogram
engine_daemon_host_info_functions_seconds_bucket{function="operating_system",le="0.005"} 1
engine_daemon_host_info_functions_seconds_bucket{function="operating_system",le="0.01"} 1
engine_daemon_host_info_functions_seconds_bucket{function="operating_system",le="0.025"} 1
engine_daemon_host_info_functions_seconds_bucket{function="operating_system",le="0.05"} 1
engine_daemon_host_info_functions_seconds_bucket{function="operating_system",le="0.1"} 1
engine_daemon_host_info_functions_seconds_bucket{function="operating_system",le="0.25"} 1
engine_daemon_host_info_functions_seconds_bucket{function="operating_system",le="0.5"} 1
engine_daemon_host_info_functions_seconds_bucket{function="operating_system",le="1"} 1
engine_daemon_host_info_functions_seconds_bucket{function="operating_system",le="2.5"} 1
engine_daemon_host_info_functions_seconds_bucket{function="operating_system",le="5"} 1
engine_daemon_host_info_functions_seconds_bucket{function="operating_system",le="10"} 1
engine_daemon_host_info_functions_seconds_bucket{function="operating_system",le="+Inf"} 1
engine_daemon_host_info_functions_seconds_sum{function="operating_system"} 5.907e-05
engine_daemon_host_info_functions_seconds_count{function="operating_system"} 1
engine_daemon_host_info_functions_seconds_bucket{function="os_version",le="0.005"} 1
engine_daemon_host_info_functions_seconds_bucket{function="os_version",le="0.01"} 1
engine_daemon_host_info_functions_seconds_bucket{function="os_version",le="0.025"} 1
engine_daemon_host_info_functions_seconds_bucket{function="os_version",le="0.05"} 1
engine_daemon_host_info_functions_seconds_bucket{function="os_version",le="0.1"} 1
engine_daemon_host_info_functions_seconds_bucket{function="os_version",le="0.25"} 1
engine_daemon_host_info_functions_seconds_bucket{function="os_version",le="0.5"} 1
engine_daemon_host_info_functions_seconds_bucket{function="os_version",le="1"} 1
engine_daemon_host_info_functions_seconds_bucket{function="os_version",le="2.5"} 1
engine_daemon_host_info_functions_seconds_bucket{function="os_version",le="5"} 1
engine_daemon_host_info_functions_seconds_bucket{function="os_version",le="10"} 1
engine_daemon_host_info_functions_seconds_bucket{function="os_version",le="+Inf"} 1
engine_daemon_host_info_functions_seconds_sum{function="os_version"} 1.997e-05
engine_daemon_host_info_functions_seconds_count{function="os_version"} 1
engine_daemon_host_info_functions_seconds_bucket{function="system_info",le="0.005"} 0
engine_daemon_host_info_functions_seconds_bucket{function="system_info",le="0.01"} 0
engine_daemon_host_info_functions_seconds_bucket{function="system_info",le="0.025"} 1
engine_daemon_host_info_functions_seconds_bucket{function="system_info",le="0.05"} 1
engine_daemon_host_info_functions_seconds_bucket{function="system_info",le="0.1"} 1
engine_daemon_host_info_functions_seconds_bucket{function="system_info",le="0.25"} 1
engine_daemon_host_info_functions_seconds_bucket{function="system_info",le="0.5"} 1
engine_daemon_host_info_functions_seconds_bucket{function="system_info",le="1"} 1
engine_daemon_host_info_functions_seconds_bucket{function="system_info",le="2.5"} 1
engine_daemon_host_info_functions_seconds_bucket{function="system_info",le="5"} 1
engine_daemon_host_info_functions_seconds_bucket{function="system_info",le="10"} 1
engine_daemon_host_info_functions_seconds_bucket{function="system_info",le="+Inf"} 1
engine_daemon_host_info_functions_seconds_sum{function="system_info"} 0.01967554
engine_daemon_host_info_functions_seconds_count{function="system_info"} 1
# HELP etcd_debugging_snap_save_marshalling_duration_seconds The marshalling cost distributions of save called by snapshot.
# TYPE etcd_debugging_snap_save_marshalling_duration_seconds histogram
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="0.001"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="0.002"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="0.004"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="0.008"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="0.016"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="0.032"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="0.064"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="0.128"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="0.256"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="0.512"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="1.024"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="2.048"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="4.096"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="8.192"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_bucket{le="+Inf"} 0
etcd_debugging_snap_save_marshalling_duration_seconds_sum 0
etcd_debugging_snap_save_marshalling_duration_seconds_count 0
# HELP etcd_debugging_snap_save_total_duration_seconds The total latency distributions of save called by snapshot.
# TYPE etcd_debugging_snap_save_total_duration_seconds histogram
etcd_debugging_snap_save_total_duration_seconds_bucket{le="0.001"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="0.002"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="0.004"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="0.008"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="0.016"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="0.032"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="0.064"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="0.128"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="0.256"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="0.512"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="1.024"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="2.048"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="4.096"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="8.192"} 0
etcd_debugging_snap_save_total_duration_seconds_bucket{le="+Inf"} 0
etcd_debugging_snap_save_total_duration_seconds_sum 0
etcd_debugging_snap_save_total_duration_seconds_count 0
# HELP etcd_disk_wal_fsync_duration_seconds The latency distributions of fsync called by WAL.
# TYPE etcd_disk_wal_fsync_duration_seconds histogram
etcd_disk_wal_fsync_duration_seconds_bucket{le="0.001"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="0.002"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="0.004"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="0.008"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="0.016"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="0.032"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="0.064"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="0.128"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="0.256"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="0.512"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="1.024"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="2.048"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="4.096"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="8.192"} 0
etcd_disk_wal_fsync_duration_seconds_bucket{le="+Inf"} 0
etcd_disk_wal_fsync_duration_seconds_sum 0
etcd_disk_wal_fsync_duration_seconds_count 0
# HELP etcd_disk_wal_write_bytes_total Total number of bytes written in WAL.
# TYPE etcd_disk_wal_write_bytes_total gauge
etcd_disk_wal_write_bytes_total 0
# HELP etcd_snap_db_fsync_duration_seconds The latency distributions of fsyncing .snap.db file
# TYPE etcd_snap_db_fsync_duration_seconds histogram
etcd_snap_db_fsync_duration_seconds_bucket{le="0.001"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="0.002"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="0.004"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="0.008"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="0.016"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="0.032"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="0.064"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="0.128"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="0.256"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="0.512"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="1.024"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="2.048"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="4.096"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="8.192"} 0
etcd_snap_db_fsync_duration_seconds_bucket{le="+Inf"} 0
etcd_snap_db_fsync_duration_seconds_sum 0
etcd_snap_db_fsync_duration_seconds_count 0
# HELP etcd_snap_db_save_total_duration_seconds The total latency distributions of v3 snapshot save
# TYPE etcd_snap_db_save_total_duration_seconds histogram
etcd_snap_db_save_total_duration_seconds_bucket{le="0.1"} 0
etcd_snap_db_save_total_duration_seconds_bucket{le="0.2"} 0
etcd_snap_db_save_total_duration_seconds_bucket{le="0.4"} 0
etcd_snap_db_save_total_duration_seconds_bucket{le="0.8"} 0
etcd_snap_db_save_total_duration_seconds_bucket{le="1.6"} 0
etcd_snap_db_save_total_duration_seconds_bucket{le="3.2"} 0
etcd_snap_db_save_total_duration_seconds_bucket{le="6.4"} 0
etcd_snap_db_save_total_duration_seconds_bucket{le="12.8"} 0
etcd_snap_db_save_total_duration_seconds_bucket{le="25.6"} 0
etcd_snap_db_save_total_duration_seconds_bucket{le="51.2"} 0
etcd_snap_db_save_total_duration_seconds_bucket{le="+Inf"} 0
etcd_snap_db_save_total_duration_seconds_sum 0
etcd_snap_db_save_total_duration_seconds_count 0
# HELP etcd_snap_fsync_duration_seconds The latency distributions of fsync called by snap.
# TYPE etcd_snap_fsync_duration_seconds histogram
etcd_snap_fsync_duration_seconds_bucket{le="0.001"} 0
etcd_snap_fsync_duration_seconds_bucket{le="0.002"} 0
etcd_snap_fsync_duration_seconds_bucket{le="0.004"} 0
etcd_snap_fsync_duration_seconds_bucket{le="0.008"} 0
etcd_snap_fsync_duration_seconds_bucket{le="0.016"} 0
etcd_snap_fsync_duration_seconds_bucket{le="0.032"} 0
etcd_snap_fsync_duration_seconds_bucket{le="0.064"} 0
etcd_snap_fsync_duration_seconds_bucket{le="0.128"} 0
etcd_snap_fsync_duration_seconds_bucket{le="0.256"} 0
etcd_snap_fsync_duration_seconds_bucket{le="0.512"} 0
etcd_snap_fsync_duration_seconds_bucket{le="1.024"} 0
etcd_snap_fsync_duration_seconds_bucket{le="2.048"} 0
etcd_snap_fsync_duration_seconds_bucket{le="4.096"} 0
etcd_snap_fsync_duration_seconds_bucket{le="8.192"} 0
etcd_snap_fsync_duration_seconds_bucket{le="+Inf"} 0
etcd_snap_fsync_duration_seconds_sum 0
etcd_snap_fsync_duration_seconds_count 0
# HELP go_gc_duration_seconds A summary of the pause duration of garbage collection cycles.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 6.3221e-05
go_gc_duration_seconds{quantile="0.25"} 7.1061e-05
go_gc_duration_seconds{quantile="0.5"} 8.2171e-05
go_gc_duration_seconds{quantile="0.75"} 0.00010649
go_gc_duration_seconds{quantile="1"} 0.000146591
go_gc_duration_seconds_sum 0.000469534
go_gc_duration_seconds_count 5
# HELP go_goroutines Number of goroutines that currently exist.
# TYPE go_goroutines gauge
go_goroutines 38
# HELP go_info Information about the Go environment.
# TYPE go_info gauge
go_info{version="go1.20.4"} 1
# HELP go_memstats_alloc_bytes Number of bytes allocated and still in use.
# TYPE go_memstats_alloc_bytes gauge
go_memstats_alloc_bytes 1.0486264e+07
# HELP go_memstats_alloc_bytes_total Total number of bytes allocated, even if freed.
# TYPE go_memstats_alloc_bytes_total counter
go_memstats_alloc_bytes_total 2.1296328e+07
# HELP go_memstats_buck_hash_sys_bytes Number of bytes used by the profiling bucket hash table.
# TYPE go_memstats_buck_hash_sys_bytes gauge
go_memstats_buck_hash_sys_bytes 1.456208e+06
# HELP go_memstats_frees_total Total number of frees.
# TYPE go_memstats_frees_total counter
go_memstats_frees_total 131318
# HELP go_memstats_gc_sys_bytes Number of bytes used for garbage collection system metadata.
# TYPE go_memstats_gc_sys_bytes gauge
go_memstats_gc_sys_bytes 8.686064e+06
# HELP go_memstats_heap_alloc_bytes Number of heap bytes allocated and still in use.
# TYPE go_memstats_heap_alloc_bytes gauge
go_memstats_heap_alloc_bytes 1.0486264e+07
# HELP go_memstats_heap_idle_bytes Number of heap bytes waiting to be used.
# TYPE go_memstats_heap_idle_bytes gauge
go_memstats_heap_idle_bytes 6.496256e+06
# HELP go_memstats_heap_inuse_bytes Number of heap bytes that are in use.
# TYPE go_memstats_heap_inuse_bytes gauge
go_memstats_heap_inuse_bytes 1.3623296e+07
# HELP go_memstats_heap_objects Number of allocated objects.
# TYPE go_memstats_heap_objects gauge
go_memstats_heap_objects 106469
# HELP go_memstats_heap_released_bytes Number of heap bytes released to OS.
# TYPE go_memstats_heap_released_bytes gauge
go_memstats_heap_released_bytes 4.17792e+06
# HELP go_memstats_heap_sys_bytes Number of heap bytes obtained from system.
# TYPE go_memstats_heap_sys_bytes gauge
go_memstats_heap_sys_bytes 2.0119552e+07
# HELP go_memstats_last_gc_time_seconds Number of seconds since 1970 of last garbage collection.
# TYPE go_memstats_last_gc_time_seconds gauge
go_memstats_last_gc_time_seconds 1.7765378502842033e+09
# HELP go_memstats_lookups_total Total number of pointer lookups.
# TYPE go_memstats_lookups_total counter
go_memstats_lookups_total 0
# HELP go_memstats_mallocs_total Total number of mallocs.
# TYPE go_memstats_mallocs_total counter
go_memstats_mallocs_total 237787
# HELP go_memstats_mcache_inuse_bytes Number of bytes in use by mcache structures.
# TYPE go_memstats_mcache_inuse_bytes gauge
go_memstats_mcache_inuse_bytes 2400
# HELP go_memstats_mcache_sys_bytes Number of bytes used for mcache structures obtained from system.
# TYPE go_memstats_mcache_sys_bytes gauge
go_memstats_mcache_sys_bytes 15600
# HELP go_memstats_mspan_inuse_bytes Number of bytes in use by mspan structures.
# TYPE go_memstats_mspan_inuse_bytes gauge
go_memstats_mspan_inuse_bytes 206400
# HELP go_memstats_mspan_sys_bytes Number of bytes used for mspan structures obtained from system.
# TYPE go_memstats_mspan_sys_bytes gauge
go_memstats_mspan_sys_bytes 212160
# HELP go_memstats_next_gc_bytes Number of heap bytes when next garbage collection will take place.
# TYPE go_memstats_next_gc_bytes gauge
go_memstats_next_gc_bytes 2.145064e+07
# HELP go_memstats_other_sys_bytes Number of bytes used for other system allocations.
# TYPE go_memstats_other_sys_bytes gauge
go_memstats_other_sys_bytes 506136
# HELP go_memstats_stack_inuse_bytes Number of bytes in use by the stack allocator.
# TYPE go_memstats_stack_inuse_bytes gauge
go_memstats_stack_inuse_bytes 851968
# HELP go_memstats_stack_sys_bytes Number of bytes obtained from system for stack allocator.
# TYPE go_memstats_stack_sys_bytes gauge
go_memstats_stack_sys_bytes 851968
# HELP go_memstats_sys_bytes Number of bytes obtained from system.
# TYPE go_memstats_sys_bytes gauge
go_memstats_sys_bytes 3.1847688e+07
# HELP go_threads Number of OS threads created.
# TYPE go_threads gauge
go_threads 11
# HELP logger_log_entries_size_greater_than_buffer_total Number of log entries which are larger than the log buffer
# TYPE logger_log_entries_size_greater_than_buffer_total counter
logger_log_entries_size_greater_than_buffer_total 0
# HELP logger_log_read_operations_failed_total Number of log reads from container stdio that failed
# TYPE logger_log_read_operations_failed_total counter
logger_log_read_operations_failed_total 0
# HELP logger_log_write_operations_failed_total Number of log write operations that failed
# TYPE logger_log_write_operations_failed_total counter
logger_log_write_operations_failed_total 0
# HELP process_cpu_seconds_total Total user and system CPU time spent in seconds.
# TYPE process_cpu_seconds_total counter
process_cpu_seconds_total 0.2
# HELP process_max_fds Maximum number of open file descriptors.
# TYPE process_max_fds gauge
process_max_fds 1.048576e+06
# HELP process_open_fds Number of open file descriptors.
# TYPE process_open_fds gauge
process_open_fds 25
# HELP process_resident_memory_bytes Resident memory size in bytes.
# TYPE process_resident_memory_bytes gauge
process_resident_memory_bytes 7.6447744e+07
# HELP process_start_time_seconds Start time of the process since unix epoch in seconds.
# TYPE process_start_time_seconds gauge
process_start_time_seconds 1.77653772891e+09
# HELP process_virtual_memory_bytes Virtual memory size in bytes.
# TYPE process_virtual_memory_bytes gauge
process_virtual_memory_bytes 1.5014912e+09
# HELP process_virtual_memory_max_bytes Maximum amount of virtual memory available in bytes.
# TYPE process_virtual_memory_max_bytes gauge
process_virtual_memory_max_bytes 1.8446744073709552e+19
# HELP promhttp_metric_handler_requests_in_flight Current number of scrapes being served.
# TYPE promhttp_metric_handler_requests_in_flight gauge
promhttp_metric_handler_requests_in_flight 1
# HELP promhttp_metric_handler_requests_total Total number of scrapes by HTTP status code.
# TYPE promhttp_metric_handler_requests_total counter
promhttp_metric_handler_requests_total{code="200"} 0
promhttp_metric_handler_requests_total{code="500"} 0
promhttp_metric_handler_requests_total{code="503"} 0
# HELP swarm_dispatcher_scheduling_delay_seconds Scheduling delay is the time a task takes to go from NEW to RUNNING state.
# TYPE swarm_dispatcher_scheduling_delay_seconds histogram
swarm_dispatcher_scheduling_delay_seconds_bucket{le="0.005"} 0
swarm_dispatcher_scheduling_delay_seconds_bucket{le="0.01"} 0
swarm_dispatcher_scheduling_delay_seconds_bucket{le="0.025"} 0
swarm_dispatcher_scheduling_delay_seconds_bucket{le="0.05"} 0
swarm_dispatcher_scheduling_delay_seconds_bucket{le="0.1"} 0
swarm_dispatcher_scheduling_delay_seconds_bucket{le="0.25"} 0
swarm_dispatcher_scheduling_delay_seconds_bucket{le="0.5"} 0
swarm_dispatcher_scheduling_delay_seconds_bucket{le="1"} 0
swarm_dispatcher_scheduling_delay_seconds_bucket{le="2.5"} 0
swarm_dispatcher_scheduling_delay_seconds_bucket{le="5"} 0
swarm_dispatcher_scheduling_delay_seconds_bucket{le="10"} 0
swarm_dispatcher_scheduling_delay_seconds_bucket{le="+Inf"} 0
swarm_dispatcher_scheduling_delay_seconds_sum 0
swarm_dispatcher_scheduling_delay_seconds_count 0
# HELP swarm_manager_configs_total The number of configs in the cluster object store
# TYPE swarm_manager_configs_total gauge
swarm_manager_configs_total 0
# HELP swarm_manager_leader Indicates if this manager node is a leader
# TYPE swarm_manager_leader gauge
swarm_manager_leader 0
# HELP swarm_manager_networks_total The number of networks in the cluster object store
# TYPE swarm_manager_networks_total gauge
swarm_manager_networks_total 0
# HELP swarm_manager_nodes The number of nodes
# TYPE swarm_manager_nodes gauge
swarm_manager_nodes{state="disconnected"} 0
swarm_manager_nodes{state="down"} 0
swarm_manager_nodes{state="ready"} 0
swarm_manager_nodes{state="unknown"} 0
# HELP swarm_manager_secrets_total The number of secrets in the cluster object store
# TYPE swarm_manager_secrets_total gauge
swarm_manager_secrets_total 0
# HELP swarm_manager_services_total The number of services in the cluster object store
# TYPE swarm_manager_services_total gauge
swarm_manager_services_total 0
# HELP swarm_manager_tasks_total The number of tasks in the cluster object store
# TYPE swarm_manager_tasks_total gauge
swarm_manager_tasks_total{state="accepted"} 0
swarm_manager_tasks_total{state="assigned"} 0
swarm_manager_tasks_total{state="complete"} 0
swarm_manager_tasks_total{state="failed"} 0
swarm_manager_tasks_total{state="new"} 0
swarm_manager_tasks_total{state="orphaned"} 0
swarm_manager_tasks_total{state="pending"} 0
swarm_manager_tasks_total{state="preparing"} 0
swarm_manager_tasks_total{state="ready"} 0
swarm_manager_tasks_total{state="rejected"} 0
swarm_manager_tasks_total{state="remove"} 0
swarm_manager_tasks_total{state="running"} 0
swarm_manager_tasks_total{state="shutdown"} 0
swarm_manager_tasks_total{state="starting"} 0
# HELP swarm_node_manager Whether this node is a manager or not
# TYPE swarm_node_manager gauge
swarm_node_manager 0
# HELP swarm_raft_snapshot_latency_seconds Raft snapshot create latency.
# TYPE swarm_raft_snapshot_latency_seconds histogram
swarm_raft_snapshot_latency_seconds_bucket{le="0.005"} 0
swarm_raft_snapshot_latency_seconds_bucket{le="0.01"} 0
swarm_raft_snapshot_latency_seconds_bucket{le="0.025"} 0
swarm_raft_snapshot_latency_seconds_bucket{le="0.05"} 0
swarm_raft_snapshot_latency_seconds_bucket{le="0.1"} 0
swarm_raft_snapshot_latency_seconds_bucket{le="0.25"} 0
swarm_raft_snapshot_latency_seconds_bucket{le="0.5"} 0
swarm_raft_snapshot_latency_seconds_bucket{le="1"} 0
swarm_raft_snapshot_latency_seconds_bucket{le="2.5"} 0
swarm_raft_snapshot_latency_seconds_bucket{le="5"} 0
swarm_raft_snapshot_latency_seconds_bucket{le="10"} 0
swarm_raft_snapshot_latency_seconds_bucket{le="+Inf"} 0
swarm_raft_snapshot_latency_seconds_sum 0
swarm_raft_snapshot_latency_seconds_count 0
# HELP swarm_raft_transaction_latency_seconds Raft transaction latency.
# TYPE swarm_raft_transaction_latency_seconds histogram
swarm_raft_transaction_latency_seconds_bucket{le="0.005"} 0
swarm_raft_transaction_latency_seconds_bucket{le="0.01"} 0
swarm_raft_transaction_latency_seconds_bucket{le="0.025"} 0
swarm_raft_transaction_latency_seconds_bucket{le="0.05"} 0
swarm_raft_transaction_latency_seconds_bucket{le="0.1"} 0
swarm_raft_transaction_latency_seconds_bucket{le="0.25"} 0
swarm_raft_transaction_latency_seconds_bucket{le="0.5"} 0
swarm_raft_transaction_latency_seconds_bucket{le="1"} 0
swarm_raft_transaction_latency_seconds_bucket{le="2.5"} 0
swarm_raft_transaction_latency_seconds_bucket{le="5"} 0
swarm_raft_transaction_latency_seconds_bucket{le="10"} 0
swarm_raft_transaction_latency_seconds_bucket{le="+Inf"} 0
swarm_raft_transaction_latency_seconds_sum 0
swarm_raft_transaction_latency_seconds_count 0
# HELP swarm_store_batch_latency_seconds Raft store batch latency.
# TYPE swarm_store_batch_latency_seconds histogram
swarm_store_batch_latency_seconds_bucket{le="0.005"} 0
swarm_store_batch_latency_seconds_bucket{le="0.01"} 0
swarm_store_batch_latency_seconds_bucket{le="0.025"} 0
swarm_store_batch_latency_seconds_bucket{le="0.05"} 0
swarm_store_batch_latency_seconds_bucket{le="0.1"} 0
swarm_store_batch_latency_seconds_bucket{le="0.25"} 0
swarm_store_batch_latency_seconds_bucket{le="0.5"} 0
swarm_store_batch_latency_seconds_bucket{le="1"} 0
swarm_store_batch_latency_seconds_bucket{le="2.5"} 0
swarm_store_batch_latency_seconds_bucket{le="5"} 0
swarm_store_batch_latency_seconds_bucket{le="10"} 0
swarm_store_batch_latency_seconds_bucket{le="+Inf"} 0
swarm_store_batch_latency_seconds_sum 0
swarm_store_batch_latency_seconds_count 0
# HELP swarm_store_lookup_latency_seconds Raft store read latency.
# TYPE swarm_store_lookup_latency_seconds histogram
swarm_store_lookup_latency_seconds_bucket{le="0.005"} 0
swarm_store_lookup_latency_seconds_bucket{le="0.01"} 0
swarm_store_lookup_latency_seconds_bucket{le="0.025"} 0
swarm_store_lookup_latency_seconds_bucket{le="0.05"} 0
swarm_store_lookup_latency_seconds_bucket{le="0.1"} 0
swarm_store_lookup_latency_seconds_bucket{le="0.25"} 0
swarm_store_lookup_latency_seconds_bucket{le="0.5"} 0
swarm_store_lookup_latency_seconds_bucket{le="1"} 0
swarm_store_lookup_latency_seconds_bucket{le="2.5"} 0
swarm_store_lookup_latency_seconds_bucket{le="5"} 0
swarm_store_lookup_latency_seconds_bucket{le="10"} 0
swarm_store_lookup_latency_seconds_bucket{le="+Inf"} 0
swarm_store_lookup_latency_seconds_sum 0
swarm_store_lookup_latency_seconds_count 0
# HELP swarm_store_memory_store_lock_duration_seconds Duration for which the raft memory store lock was held.
# TYPE swarm_store_memory_store_lock_duration_seconds histogram
swarm_store_memory_store_lock_duration_seconds_bucket{le="0.005"} 0
swarm_store_memory_store_lock_duration_seconds_bucket{le="0.01"} 0
swarm_store_memory_store_lock_duration_seconds_bucket{le="0.025"} 0
swarm_store_memory_store_lock_duration_seconds_bucket{le="0.05"} 0
swarm_store_memory_store_lock_duration_seconds_bucket{le="0.1"} 0
swarm_store_memory_store_lock_duration_seconds_bucket{le="0.25"} 0
swarm_store_memory_store_lock_duration_seconds_bucket{le="0.5"} 0
swarm_store_memory_store_lock_duration_seconds_bucket{le="1"} 0
swarm_store_memory_store_lock_duration_seconds_bucket{le="2.5"} 0
swarm_store_memory_store_lock_duration_seconds_bucket{le="5"} 0
swarm_store_memory_store_lock_duration_seconds_bucket{le="10"} 0
swarm_store_memory_store_lock_duration_seconds_bucket{le="+Inf"} 0
swarm_store_memory_store_lock_duration_seconds_sum 0
swarm_store_memory_store_lock_duration_seconds_count 0
# HELP swarm_store_read_tx_latency_seconds Raft store read tx latency.
# TYPE swarm_store_read_tx_latency_seconds histogram
swarm_store_read_tx_latency_seconds_bucket{le="0.005"} 0
swarm_store_read_tx_latency_seconds_bucket{le="0.01"} 0
swarm_store_read_tx_latency_seconds_bucket{le="0.025"} 0
swarm_store_read_tx_latency_seconds_bucket{le="0.05"} 0
swarm_store_read_tx_latency_seconds_bucket{le="0.1"} 0
swarm_store_read_tx_latency_seconds_bucket{le="0.25"} 0
swarm_store_read_tx_latency_seconds_bucket{le="0.5"} 0
swarm_store_read_tx_latency_seconds_bucket{le="1"} 0
swarm_store_read_tx_latency_seconds_bucket{le="2.5"} 0
swarm_store_read_tx_latency_seconds_bucket{le="5"} 0
swarm_store_read_tx_latency_seconds_bucket{le="10"} 0
swarm_store_read_tx_latency_seconds_bucket{le="+Inf"} 0
swarm_store_read_tx_latency_seconds_sum 0
swarm_store_read_tx_latency_seconds_count 0
# HELP swarm_store_write_tx_latency_seconds Raft store write tx latency.
# TYPE swarm_store_write_tx_latency_seconds histogram
swarm_store_write_tx_latency_seconds_bucket{le="0.005"} 0
swarm_store_write_tx_latency_seconds_bucket{le="0.01"} 0
swarm_store_write_tx_latency_seconds_bucket{le="0.025"} 0
swarm_store_write_tx_latency_seconds_bucket{le="0.05"} 0
swarm_store_write_tx_latency_seconds_bucket{le="0.1"} 0
swarm_store_write_tx_latency_seconds_bucket{le="0.25"} 0
swarm_store_write_tx_latency_seconds_bucket{le="0.5"} 0
swarm_store_write_tx_latency_seconds_bucket{le="1"} 0
swarm_store_write_tx_latency_seconds_bucket{le="2.5"} 0
swarm_store_write_tx_latency_seconds_bucket{le="5"} 0
swarm_store_write_tx_latency_seconds_bucket{le="10"} 0
swarm_store_write_tx_latency_seconds_bucket{le="+Inf"} 0
swarm_store_write_tx_latency_seconds_sum 0
swarm_store_write_tx_latency_seconds_count 0
root@host01:~$ 

```
---

## Step 2 - Configure Prometheus

- The Prometheus server requires a configuration file that defines the endpoints to scrape along with how frequently the metrics should be accessed.

- The first half of the configuration defines the intervals.



```
global:
  scrape_interval:     15s  # Defines the intervals
  evaluation_interval: 15s

```

- The second half defines the servers and ports that Prometheus should scrape data from. In this example, we have defined three targets running on different ports.

```
scrape_configs:
  - job_name: 'prometheus'

    static_configs:
      - targets: ['127.0.0.1:9090', '127.0.0.1:9100', '127.0.0.1:9323']
        labels:
          group: 'prometheus'


```


- 9090 is Prometheus itself. Prometheus exposes information related to its internal metrics and performance.

- 9100 is the Node Exporter Prometheus process. This exposes information about the Node, such as disk space, memory and CPU usage.

- 9323 is the Docker Metrics port just exposed. This reports information from the Docker Engine.

---

## Step 3 - Start Prometheus


Prometheus runs as a Docker Container with a UI available on port 9090. Prometheus uses the configuration to scrape the targets, collect and store the metrics before making them available via API that allows dashboards, graphing and alerting.

#### Task

The following command launches the container with the Prometheus configuration. Any data created by Prometheus will be stored on the host, in the directory /prometheus/data. When we update the container, the data will be persisted.

```
root@host01:~$ docker run -d --net=host \
>     -v /root/prometheus.yml:/etc/prometheus/prometheus.yml \
>     --name prometheus-server \
>     prom/prometheus
Unable to find image 'prom/prometheus:latest' locally
latest: Pulling from prom/prometheus
a6680b927350: Pull complete 
a2243411ec8e: Pull complete 
ce2ce3cfbbe4: Extracting [=======================================>           ]   63.5MB/81.28MB
e3e60763add3: Download complete 
5b801831dd96: Download complete 
9fa05948455e: Download complete 
ce2ce3cfbbe4: Pull complete 
e3e60763add3: Pull complete 
5b801831dd96: Pull complete 
9fa05948455e: Pull complete 
d70e68dbd61c: Pull complete 
f86ef4240a84: Pull complete 
259391b25b59: Pull complete 
f60c291aa4d9: Pull complete 
Digest: sha256:5550dc63da361dc30f6fe02ac0e4dfc736ededfef3c8d12a634db04a67824d78
Status: Downloaded newer image for prom/prometheus:latest
f35277c4d215e20be4af673fb3374e9fbe403c926172d988ef1a6c2f81d64c3f

root@host01:~$ docker run -d --net=host \
>     -v /root/prometheus.yml:/etc/prometheus/prometheus.yml \
>     --name prometheus-server \
>     prom/prometheus
docker: Error response from daemon: Conflict. The container name "/prometheus-server" is already in use by container "f35277c4d215e20be4af673fb3374e9fbe403c926172d988ef1a6c2f81d64c3f". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.

root@host01:~$ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS          PORTS     NAMES
f35277c4d215   prom/prometheus   "/bin/prometheus --c…"   47 seconds ago   Up 46 seconds             prometheus-server

root@host01:~$ docker ps -a
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS          PORTS     NAMES
f35277c4d215   prom/prometheus   "/bin/prometheus --c…"   53 seconds ago   Up 52 seconds             prometheus-server

root@host01:~$ docker logs f3
time=2026-04-18T19:00:41.087Z level=INFO source=main.go:1678 msg="updated GOGC" old=100 new=75
time=2026-04-18T19:00:41.089Z level=INFO source=main.go:744 msg="Leaving GOMAXPROCS=2: CPU quota undefined" component=automaxprocs
time=2026-04-18T19:00:41.089Z level=INFO source=memlimit.go:198 msg="GOMEMLIMIT is updated" component=automemlimit package=github.com/KimMachineGun/automemlimit/memlimit GOMEMLIMIT=1354873651 previous=9223372036854775807
time=2026-04-18T19:00:41.089Z level=INFO source=main.go:851 msg="Starting Prometheus Server" mode=server version="(version=3.11.2, branch=HEAD, revision=f0f0fdd679dcd6df320b0558b20919f7cd44c407)"
time=2026-04-18T19:00:41.089Z level=INFO source=main.go:856 msg="operational information" build_context="(go=go1.26.2, platform=linux/amd64, user=root@d2684485c347, date=20260413-11:53:40, tags=netgo,builtinassets)" host_details="(Linux 5.15.0-72-generic #79-Ubuntu SMP Wed Apr 19 08:22:18 UTC 2023 x86_64 host01 (none))" fd_limits="(soft=1048576, hard=1048576)" vm_limits="(soft=unlimited, hard=unlimited)"
time=2026-04-18T19:00:41.264Z level=INFO source=web.go:710 msg="Start listening for connections" component=web address=0.0.0.0:9090
time=2026-04-18T19:00:41.265Z level=INFO source=main.go:1410 msg="Starting TSDB ..."
time=2026-04-18T19:00:41.270Z level=INFO source=tls_config.go:354 msg="Listening on" component=web address=[::]:9090
time=2026-04-18T19:00:41.270Z level=INFO source=tls_config.go:357 msg="TLS is disabled." component=web http2=false address=[::]:9090
time=2026-04-18T19:00:41.271Z level=INFO source=head.go:698 msg="Replaying on-disk memory mappable chunks if any" component=tsdb
time=2026-04-18T19:00:41.272Z level=INFO source=head.go:784 msg="On-disk memory mappable chunks replay completed" component=tsdb duration=1.41µs
time=2026-04-18T19:00:41.272Z level=INFO source=head.go:792 msg="Replaying WAL, this may take a while" component=tsdb
time=2026-04-18T19:00:41.272Z level=INFO source=head.go:865 msg="WAL segment loaded" component=tsdb segment=0 maxSegment=0 duration=240.632µs
time=2026-04-18T19:00:41.272Z level=INFO source=head.go:902 msg="WAL replay completed" component=tsdb checkpoint_replay_duration=31.74µs wal_replay_duration=358.233µs wbl_replay_duration=220ns chunk_snapshot_load_duration=0s mmap_chunk_replay_duration=1.41µs total_replay_duration=667.876µs
time=2026-04-18T19:00:41.274Z level=INFO source=main.go:1431 msg="filesystem information" fs_type=EXT4_SUPER_MAGIC
time=2026-04-18T19:00:41.274Z level=INFO source=main.go:1434 msg="TSDB started"
time=2026-04-18T19:00:41.274Z level=INFO source=main.go:1632 msg="Loading configuration file" filename=/etc/prometheus/prometheus.yml
time=2026-04-18T19:00:41.275Z level=INFO source=main.go:1048 msg="TSDB retention updated" duration=15d size=0B percentage=0
time=2026-04-18T19:00:41.276Z level=INFO source=main.go:1671 msg="Completed loading of configuration file" db_storage=113.341µs remote_storage=1.86µs web_handler=430ns query_engine=670ns scrape=917.149µs scrape_sd=22.44µs notify=1.55µs notify_sd=860ns rules=1.141µs tracing=5.74µs filename=/etc/prometheus/prometheus.yml totalDuration=1.408945ms
time=2026-04-18T19:00:41.276Z level=INFO source=main.go:1395 msg="Server is ready to receive web requests."
time=2026-04-18T19:00:41.276Z level=INFO source=manager.go:209 msg="Starting rule manager..." component="rule manager"
root@host01:~$

```
---


## Step 4 - Start Node Exporter

The Docker Metrics endpoint will return information related to Docker. To collect metrics on the Docker Host it's required to run a Prometheus Node Exporter. Prometheus has many exporters that are designed to output metrics for a particular system, such as Postgres or MySQL.

#### Task
Launch the Node Exporter container. By mounting the host /proc and /sys directory, the container has accessed to the necessary information to report on.

```
docker run -d \
  -v "/proc:/host/proc" \
  -v "/sys:/host/sys" \
  -v "/:/rootfs" \
  --net="host" \
  --name=prometheus \
  quay.io/prometheus/node-exporter:v0.13.0 \
    -collector.procfs /host/proc \
    -collector.sysfs /host/sys \
    -collector.filesystem.ignored-mount-points "^/(sys|proc|dev|host|etc)($|/)"

# Node exporter metrics

# TYPE node_netstat_TcpExt_TCPAbortFailed untyped
node_netstat_TcpExt_TCPAbortFailed 0
# HELP node_netstat_TcpExt_TCPAbortOnClose Protocol TcpExt statistic TCPAbortOnClose.
# TYPE node_netstat_TcpExt_TCPAbortOnClose untyped
node_netstat_TcpExt_TCPAbortOnClose 1
# HELP node_netstat_TcpExt_TCPAbortOnData Protocol TcpExt statistic TCPAbortOnData.
# TYPE node_netstat_TcpExt_TCPAbortOnData untyped
node_netstat_TcpExt_TCPAbortOnData 3
# HELP node_netstat_TcpExt_TCPAbortOnLinger Protocol TcpExt statistic TCPAbortOnLinger.
# TYPE node_netstat_TcpExt_TCPAbortOnLinger untyped
node_netstat_TcpExt_TCPAbortOnLinger 0
# HELP node_netstat_TcpExt_TCPAbortOnMemory Protocol TcpExt statistic TCPAbortOnMemory.
# TYPE node_netstat_TcpExt_TCPAbortOnMemory untyped
node_netstat_TcpExt_TCPAbortOnMemory 0
# HELP node_netstat_TcpExt_TCPAbortOnTimeout Protocol TcpExt statistic TCPAbortOnTimeout.
# TYPE node_netstat_TcpExt_TCPAbortOnTimeout untyped
node_netstat_TcpExt_TCPAbortOnTimeout 0
# HELP node_netstat_TcpExt_TCPAckCompressed Protocol TcpExt statistic TCPAckCompressed.
# TYPE node_netstat_TcpExt_TCPAckCompressed untyped
node_netstat_TcpExt_TCPAckCompressed 11698
# HELP node_netstat_TcpExt_TCPAutoCorking Protocol TcpExt statistic TCPAutoCorking.
# TYPE node_netstat_TcpExt_TCPAutoCorking untyped
node_netstat_TcpExt_TCPAutoCorking 489
# HELP node_netstat_TcpExt_TCPBacklogCoalesce Protocol TcpExt statistic TCPBacklogCoalesce.
# TYPE node_netstat_TcpExt_TCPBacklogCoalesce untyped
node_netstat_TcpExt_TCPBacklogCoalesce 3850
# HELP node_netstat_TcpExt_TCPBacklogDrop Protocol TcpExt statistic TCPBacklogDrop.
# TYPE node_netstat_TcpExt_TCPBacklogDrop untyped
node_netstat_TcpExt_TCPBacklogDrop 0
# HELP node_netstat_TcpExt_TCPChallengeACK Protocol TcpExt statistic TCPChallengeACK.
# TYPE node_netstat_TcpExt_TCPChallengeACK untyped
node_netstat_TcpExt_TCPChallengeACK 1
# HELP node_netstat_TcpExt_TCPDSACKIgnoredDubious Protocol TcpExt statistic TCPDSACKIgnoredDubious.
# TYPE node_netstat_TcpExt_TCPDSACKIgnoredDubious untyped
node_netstat_TcpExt_TCPDSACKIgnoredDubious 0
# HELP node_netstat_TcpExt_TCPDSACKIgnoredNoUndo Protocol TcpExt statistic TCPDSACKIgnoredNoUndo.
# TYPE node_netstat_TcpExt_TCPDSACKIgnoredNoUndo untyped
node_netstat_TcpExt_TCPDSACKIgnoredNoUndo 1
# HELP node_netstat_TcpExt_TCPDSACKIgnoredOld Protocol TcpExt statistic TCPDSACKIgnoredOld.
# TYPE node_netstat_TcpExt_TCPDSACKIgnoredOld untyped
node_netstat_TcpExt_TCPDSACKIgnoredOld 0
# HELP node_netstat_TcpExt_TCPDSACKOfoRecv Protocol TcpExt statistic TCPDSACKOfoRecv.
# TYPE node_netstat_TcpExt_TCPDSACKOfoRecv untyped
node_netstat_TcpExt_TCPDSACKOfoRecv 0
# HELP node_netstat_TcpExt_TCPDSACKOfoSent Protocol TcpExt statistic TCPDSACKOfoSent.
# TYPE node_netstat_TcpExt_TCPDSACKOfoSent untyped
node_netstat_TcpExt_TCPDSACKOfoSent 0
# HELP node_netstat_TcpExt_TCPDSACKOldSent Protocol TcpExt statistic TCPDSACKOldSent.
# TYPE node_netstat_TcpExt_TCPDSACKOldSent untyped
node_netstat_TcpExt_TCPDSACKOldSent 3
# HELP node_netstat_TcpExt_TCPDSACKRecv Protocol TcpExt statistic TCPDSACKRecv.
# TYPE node_netstat_TcpExt_TCPDSACKRecv untyped
node_netstat_TcpExt_TCPDSACKRecv 1
# HELP node_netstat_TcpExt_TCPDSACKRecvSegs Protocol TcpExt statistic TCPDSACKRecvSegs.
# TYPE node_netstat_TcpExt_TCPDSACKRecvSegs untyped
node_netstat_TcpExt_TCPDSACKRecvSegs 1
# HELP node_netstat_TcpExt_TCPDSACKUndo Protocol TcpExt statistic TCPDSACKUndo.
# TYPE node_netstat_TcpExt_TCPDSACKUndo untyped
node_netstat_TcpExt_TCPDSACKUndo 0
# HELP node_netstat_TcpExt_TCPDeferAcceptDrop Protocol TcpExt statistic TCPDeferAcceptDrop.
# TYPE node_netstat_TcpExt_TCPDeferAcceptDrop untyped
node_netstat_TcpExt_TCPDeferAcceptDrop 0
# HELP node_netstat_TcpExt_TCPDelivered Protocol TcpExt statistic TCPDelivered.
# TYPE node_netstat_TcpExt_TCPDelivered untyped
node_netstat_TcpExt_TCPDelivered 13177
# HELP node_netstat_TcpExt_TCPDeliveredCE Protocol TcpExt statistic TCPDeliveredCE.
# TYPE node_netstat_TcpExt_TCPDeliveredCE untyped
node_netstat_TcpExt_TCPDeliveredCE 0
# HELP node_netstat_TcpExt_TCPFastOpenActive Protocol TcpExt statistic TCPFastOpenActive.
# TYPE node_netstat_TcpExt_TCPFastOpenActive untyped
node_netstat_TcpExt_TCPFastOpenActive 0
# HELP node_netstat_TcpExt_TCPFastOpenActiveFail Protocol TcpExt statistic TCPFastOpenActiveFail.
# TYPE node_netstat_TcpExt_TCPFastOpenActiveFail untyped
node_netstat_TcpExt_TCPFastOpenActiveFail 0
# HELP node_netstat_TcpExt_TCPFastOpenBlackhole Protocol TcpExt statistic TCPFastOpenBlackhole.
# TYPE node_netstat_TcpExt_TCPFastOpenBlackhole untyped
node_netstat_TcpExt_TCPFastOpenBlackhole 0
# HELP node_netstat_TcpExt_TCPFastOpenCookieReqd Protocol TcpExt statistic TCPFastOpenCookieReqd.
# TYPE node_netstat_TcpExt_TCPFastOpenCookieReqd untyped
node_netstat_TcpExt_TCPFastOpenCookieReqd 0
# HELP node_netstat_TcpExt_TCPFastOpenListenOverflow Protocol TcpExt statistic TCPFastOpenListenOverflow.
# TYPE node_netstat_TcpExt_TCPFastOpenListenOverflow untyped
node_netstat_TcpExt_TCPFastOpenListenOverflow 0
# HELP node_netstat_TcpExt_TCPFastOpenPassive Protocol TcpExt statistic TCPFastOpenPassive.
# TYPE node_netstat_TcpExt_TCPFastOpenPassive untyped
node_netstat_TcpExt_TCPFastOpenPassive 0
# HELP node_netstat_TcpExt_TCPFastOpenPassiveAltKey Protocol TcpExt statistic TCPFastOpenPassiveAltKey.
# TYPE node_netstat_TcpExt_TCPFastOpenPassiveAltKey untyped
node_netstat_TcpExt_TCPFastOpenPassiveAltKey 0
# HELP node_netstat_TcpExt_TCPFastOpenPassiveFail Protocol TcpExt statistic TCPFastOpenPassiveFail.
# TYPE node_netstat_TcpExt_TCPFastOpenPassiveFail untyped
node_netstat_TcpExt_TCPFastOpenPassiveFail 0
# HELP node_netstat_TcpExt_TCPFastRetrans Protocol TcpExt statistic TCPFastRetrans.
# TYPE node_netstat_TcpExt_TCPFastRetrans untyped
node_netstat_TcpExt_TCPFastRetrans 0
# HELP node_netstat_TcpExt_TCPFromZeroWindowAdv Protocol TcpExt statistic TCPFromZeroWindowAdv.
# TYPE node_netstat_TcpExt_TCPFromZeroWindowAdv untyped
node_netstat_TcpExt_TCPFromZeroWindowAdv 0
# HELP node_netstat_TcpExt_TCPFullUndo Protocol TcpExt statistic TCPFullUndo.
# TYPE node_netstat_TcpExt_TCPFullUndo untyped
node_netstat_TcpExt_TCPFullUndo 0
# HELP node_netstat_TcpExt_TCPHPAcks Protocol TcpExt statistic TCPHPAcks.
# TYPE node_netstat_TcpExt_TCPHPAcks untyped
node_netstat_TcpExt_TCPHPAcks 3395
# HELP node_netstat_TcpExt_TCPHPHits Protocol TcpExt statistic TCPHPHits.
# TYPE node_netstat_TcpExt_TCPHPHits untyped
node_netstat_TcpExt_TCPHPHits 94644
# HELP node_netstat_TcpExt_TCPHystartDelayCwnd Protocol TcpExt statistic TCPHystartDelayCwnd.
# TYPE node_netstat_TcpExt_TCPHystartDelayCwnd untyped
node_netstat_TcpExt_TCPHystartDelayCwnd 0
# HELP node_netstat_TcpExt_TCPHystartDelayDetect Protocol TcpExt statistic TCPHystartDelayDetect.
# TYPE node_netstat_TcpExt_TCPHystartDelayDetect untyped
node_netstat_TcpExt_TCPHystartDelayDetect 0
# HELP node_netstat_TcpExt_TCPHystartTrainCwnd Protocol TcpExt statistic TCPHystartTrainCwnd.
# TYPE node_netstat_TcpExt_TCPHystartTrainCwnd untyped
node_netstat_TcpExt_TCPHystartTrainCwnd 741
# HELP node_netstat_TcpExt_TCPHystartTrainDetect Protocol TcpExt statistic TCPHystartTrainDetect.
# TYPE node_netstat_TcpExt_TCPHystartTrainDetect untyped
node_netstat_TcpExt_TCPHystartTrainDetect 7
# HELP node_netstat_TcpExt_TCPKeepAlive Protocol TcpExt statistic TCPKeepAlive.
# TYPE node_netstat_TcpExt_TCPKeepAlive untyped
node_netstat_TcpExt_TCPKeepAlive 112
# HELP node_netstat_TcpExt_TCPLossFailures Protocol TcpExt statistic TCPLossFailures.
# TYPE node_netstat_TcpExt_TCPLossFailures untyped
node_netstat_TcpExt_TCPLossFailures 0
# HELP node_netstat_TcpExt_TCPLossProbeRecovery Protocol TcpExt statistic TCPLossProbeRecovery.
# TYPE node_netstat_TcpExt_TCPLossProbeRecovery untyped
node_netstat_TcpExt_TCPLossProbeRecovery 0
# HELP node_netstat_TcpExt_TCPLossProbes Protocol TcpExt statistic TCPLossProbes.
# TYPE node_netstat_TcpExt_TCPLossProbes untyped
node_netstat_TcpExt_TCPLossProbes 4
# HELP node_netstat_TcpExt_TCPLossUndo Protocol TcpExt statistic TCPLossUndo.
# TYPE node_netstat_TcpExt_TCPLossUndo untyped
node_netstat_TcpExt_TCPLossUndo 0
# HELP node_netstat_TcpExt_TCPLostRetransmit Protocol TcpExt statistic TCPLostRetransmit.
# TYPE node_netstat_TcpExt_TCPLostRetransmit untyped
node_netstat_TcpExt_TCPLostRetransmit 0
# HELP node_netstat_TcpExt_TCPMD5Failure Protocol TcpExt statistic TCPMD5Failure.
# TYPE node_netstat_TcpExt_TCPMD5Failure untyped
node_netstat_TcpExt_TCPMD5Failure 0
# HELP node_netstat_TcpExt_TCPMD5NotFound Protocol TcpExt statistic TCPMD5NotFound.
# TYPE node_netstat_TcpExt_TCPMD5NotFound untyped
node_netstat_TcpExt_TCPMD5NotFound 0
# HELP node_netstat_TcpExt_TCPMD5Unexpected Protocol TcpExt statistic TCPMD5Unexpected.
# TYPE node_netstat_TcpExt_TCPMD5Unexpected untyped
node_netstat_TcpExt_TCPMD5Unexpected 0
# HELP node_netstat_TcpExt_TCPMTUPFail Protocol TcpExt statistic TCPMTUPFail.
# TYPE node_netstat_TcpExt_TCPMTUPFail untyped
node_netstat_TcpExt_TCPMTUPFail 0
# HELP node_netstat_TcpExt_TCPMTUPSuccess Protocol TcpExt statistic TCPMTUPSuccess.
# TYPE node_netstat_TcpExt_TCPMTUPSuccess untyped
node_netstat_TcpExt_TCPMTUPSuccess 0
# HELP node_netstat_TcpExt_TCPMemoryPressures Protocol TcpExt statistic TCPMemoryPressures.
# TYPE node_netstat_TcpExt_TCPMemoryPressures untyped
node_netstat_TcpExt_TCPMemoryPressures 0
# HELP node_netstat_TcpExt_TCPMemoryPressuresChrono Protocol TcpExt statistic TCPMemoryPressuresChrono.
# TYPE node_netstat_TcpExt_TCPMemoryPressuresChrono untyped
node_netstat_TcpExt_TCPMemoryPressuresChrono 0
# HELP node_netstat_TcpExt_TCPMigrateReqFailure Protocol TcpExt statistic TCPMigrateReqFailure.
# TYPE node_netstat_TcpExt_TCPMigrateReqFailure untyped
node_netstat_TcpExt_TCPMigrateReqFailure 0
# HELP node_netstat_TcpExt_TCPMigrateReqSuccess Protocol TcpExt statistic TCPMigrateReqSuccess.
# TYPE node_netstat_TcpExt_TCPMigrateReqSuccess untyped
node_netstat_TcpExt_TCPMigrateReqSuccess 0
# HELP node_netstat_TcpExt_TCPMinTTLDrop Protocol TcpExt statistic TCPMinTTLDrop.
# TYPE node_netstat_TcpExt_TCPMinTTLDrop untyped
node_netstat_TcpExt_TCPMinTTLDrop 0
# HELP node_netstat_TcpExt_TCPOFODrop Protocol TcpExt statistic TCPOFODrop.
# TYPE node_netstat_TcpExt_TCPOFODrop untyped
node_netstat_TcpExt_TCPOFODrop 0
# HELP node_netstat_TcpExt_TCPOFOMerge Protocol TcpExt statistic TCPOFOMerge.
# TYPE node_netstat_TcpExt_TCPOFOMerge untyped
node_netstat_TcpExt_TCPOFOMerge 0
# HELP node_netstat_TcpExt_TCPOFOQueue Protocol TcpExt statistic TCPOFOQueue.
# TYPE node_netstat_TcpExt_TCPOFOQueue untyped
node_netstat_TcpExt_TCPOFOQueue 13123
# HELP node_netstat_TcpExt_TCPOrigDataSent Protocol TcpExt statistic TCPOrigDataSent.
# TYPE node_netstat_TcpExt_TCPOrigDataSent untyped
node_netstat_TcpExt_TCPOrigDataSent 13210
# HELP node_netstat_TcpExt_TCPPartialUndo Protocol TcpExt statistic TCPPartialUndo.
# TYPE node_netstat_TcpExt_TCPPartialUndo untyped
node_netstat_TcpExt_TCPPartialUndo 0
# HELP node_netstat_TcpExt_TCPPureAcks Protocol TcpExt statistic TCPPureAcks.
# TYPE node_netstat_TcpExt_TCPPureAcks untyped
node_netstat_TcpExt_TCPPureAcks 2741
# HELP node_netstat_TcpExt_TCPRcvCoalesce Protocol TcpExt statistic TCPRcvCoalesce.
# TYPE node_netstat_TcpExt_TCPRcvCoalesce untyped
node_netstat_TcpExt_TCPRcvCoalesce 86896
# HELP node_netstat_TcpExt_TCPRcvCollapsed Protocol TcpExt statistic TCPRcvCollapsed.
# TYPE node_netstat_TcpExt_TCPRcvCollapsed untyped
node_netstat_TcpExt_TCPRcvCollapsed 0
# HELP node_netstat_TcpExt_TCPRcvQDrop Protocol TcpExt statistic TCPRcvQDrop.
# TYPE node_netstat_TcpExt_TCPRcvQDrop untyped
node_netstat_TcpExt_TCPRcvQDrop 0
# HELP node_netstat_TcpExt_TCPRenoFailures Protocol TcpExt statistic TCPRenoFailures.
# TYPE node_netstat_TcpExt_TCPRenoFailures untyped
node_netstat_TcpExt_TCPRenoFailures 0
# HELP node_netstat_TcpExt_TCPRenoRecovery Protocol TcpExt statistic TCPRenoRecovery.
# TYPE node_netstat_TcpExt_TCPRenoRecovery untyped
node_netstat_TcpExt_TCPRenoRecovery 0
# HELP node_netstat_TcpExt_TCPRenoRecoveryFail Protocol TcpExt statistic TCPRenoRecoveryFail.
# TYPE node_netstat_TcpExt_TCPRenoRecoveryFail untyped
node_netstat_TcpExt_TCPRenoRecoveryFail 0
# HELP node_netstat_TcpExt_TCPRenoReorder Protocol TcpExt statistic TCPRenoReorder.
# TYPE node_netstat_TcpExt_TCPRenoReorder untyped
node_netstat_TcpExt_TCPRenoReorder 0
# HELP node_netstat_TcpExt_TCPReqQFullDoCookies Protocol TcpExt statistic TCPReqQFullDoCookies.
# TYPE node_netstat_TcpExt_TCPReqQFullDoCookies untyped
node_netstat_TcpExt_TCPReqQFullDoCookies 0
# HELP node_netstat_TcpExt_TCPReqQFullDrop Protocol TcpExt statistic TCPReqQFullDrop.
# TYPE node_netstat_TcpExt_TCPReqQFullDrop untyped
node_netstat_TcpExt_TCPReqQFullDrop 0
# HELP node_netstat_TcpExt_TCPRetransFail Protocol TcpExt statistic TCPRetransFail.
# TYPE node_netstat_TcpExt_TCPRetransFail untyped
node_netstat_TcpExt_TCPRetransFail 0
# HELP node_netstat_TcpExt_TCPSACKDiscard Protocol TcpExt statistic TCPSACKDiscard.
# TYPE node_netstat_TcpExt_TCPSACKDiscard untyped
node_netstat_TcpExt_TCPSACKDiscard 0
# HELP node_netstat_TcpExt_TCPSACKReneging Protocol TcpExt statistic TCPSACKReneging.
# TYPE node_netstat_TcpExt_TCPSACKReneging untyped
node_netstat_TcpExt_TCPSACKReneging 0
# HELP node_netstat_TcpExt_TCPSACKReorder Protocol TcpExt statistic TCPSACKReorder.
# TYPE node_netstat_TcpExt_TCPSACKReorder untyped
node_netstat_TcpExt_TCPSACKReorder 2
# HELP node_netstat_TcpExt_TCPSYNChallenge Protocol TcpExt statistic TCPSYNChallenge.
# TYPE node_netstat_TcpExt_TCPSYNChallenge untyped
node_netstat_TcpExt_TCPSYNChallenge 1
# HELP node_netstat_TcpExt_TCPSackFailures Protocol TcpExt statistic TCPSackFailures.
# TYPE node_netstat_TcpExt_TCPSackFailures untyped
node_netstat_TcpExt_TCPSackFailures 0
# HELP node_netstat_TcpExt_TCPSackMerged Protocol TcpExt statistic TCPSackMerged.
# TYPE node_netstat_TcpExt_TCPSackMerged untyped
node_netstat_TcpExt_TCPSackMerged 0
# HELP node_netstat_TcpExt_TCPSackRecovery Protocol TcpExt statistic TCPSackRecovery.
# TYPE node_netstat_TcpExt_TCPSackRecovery untyped
node_netstat_TcpExt_TCPSackRecovery 0
# HELP node_netstat_TcpExt_TCPSackRecoveryFail Protocol TcpExt statistic TCPSackRecoveryFail.
# TYPE node_netstat_TcpExt_TCPSackRecoveryFail untyped
node_netstat_TcpExt_TCPSackRecoveryFail 0
# HELP node_netstat_TcpExt_TCPSackShiftFallback Protocol TcpExt statistic TCPSackShiftFallback.
# TYPE node_netstat_TcpExt_TCPSackShiftFallback untyped
node_netstat_TcpExt_TCPSackShiftFallback 4
# HELP node_netstat_TcpExt_TCPSackShifted Protocol TcpExt statistic TCPSackShifted.
# TYPE node_netstat_TcpExt_TCPSackShifted untyped
node_netstat_TcpExt_TCPSackShifted 0
# HELP node_netstat_TcpExt_TCPSlowStartRetrans Protocol TcpExt statistic TCPSlowStartRetrans.
# TYPE node_netstat_TcpExt_TCPSlowStartRetrans untyped
node_netstat_TcpExt_TCPSlowStartRetrans 0
# HELP node_netstat_TcpExt_TCPSpuriousRTOs Protocol TcpExt statistic TCPSpuriousRTOs.
# TYPE node_netstat_TcpExt_TCPSpuriousRTOs untyped
node_netstat_TcpExt_TCPSpuriousRTOs 0
# HELP node_netstat_TcpExt_TCPSpuriousRtxHostQueues Protocol TcpExt statistic TCPSpuriousRtxHostQueues.
# TYPE node_netstat_TcpExt_TCPSpuriousRtxHostQueues untyped
node_netstat_TcpExt_TCPSpuriousRtxHostQueues 0
# HELP node_netstat_TcpExt_TCPSynRetrans Protocol TcpExt statistic TCPSynRetrans.
# TYPE node_netstat_TcpExt_TCPSynRetrans untyped
node_netstat_TcpExt_TCPSynRetrans 1
# HELP node_netstat_TcpExt_TCPTSReorder Protocol TcpExt statistic TCPTSReorder.
# TYPE node_netstat_TcpExt_TCPTSReorder untyped
node_netstat_TcpExt_TCPTSReorder 0
# HELP node_netstat_TcpExt_TCPTimeWaitOverflow Protocol TcpExt statistic TCPTimeWaitOverflow.
# TYPE node_netstat_TcpExt_TCPTimeWaitOverflow untyped
node_netstat_TcpExt_TCPTimeWaitOverflow 0
# HELP node_netstat_TcpExt_TCPTimeouts Protocol TcpExt statistic TCPTimeouts.
# TYPE node_netstat_TcpExt_TCPTimeouts untyped
node_netstat_TcpExt_TCPTimeouts 1
# HELP node_netstat_TcpExt_TCPToZeroWindowAdv Protocol TcpExt statistic TCPToZeroWindowAdv.
# TYPE node_netstat_TcpExt_TCPToZeroWindowAdv untyped
node_netstat_TcpExt_TCPToZeroWindowAdv 0
# HELP node_netstat_TcpExt_TCPWantZeroWindowAdv Protocol TcpExt statistic TCPWantZeroWindowAdv.
# TYPE node_netstat_TcpExt_TCPWantZeroWindowAdv untyped
node_netstat_TcpExt_TCPWantZeroWindowAdv 2
# HELP node_netstat_TcpExt_TCPWinProbe Protocol TcpExt statistic TCPWinProbe.
# TYPE node_netstat_TcpExt_TCPWinProbe untyped
node_netstat_TcpExt_TCPWinProbe 1
# HELP node_netstat_TcpExt_TCPWqueueTooBig Protocol TcpExt statistic TCPWqueueTooBig.
# TYPE node_netstat_TcpExt_TCPWqueueTooBig untyped
node_netstat_TcpExt_TCPWqueueTooBig 0
# HELP node_netstat_TcpExt_TCPZeroWindowDrop Protocol TcpExt statistic TCPZeroWindowDrop.
# TYPE node_netstat_TcpExt_TCPZeroWindowDrop untyped
node_netstat_TcpExt_TCPZeroWindowDrop 0
# HELP node_netstat_TcpExt_TW Protocol TcpExt statistic TW.
# TYPE node_netstat_TcpExt_TW untyped
node_netstat_TcpExt_TW 83
# HELP node_netstat_TcpExt_TWKilled Protocol TcpExt statistic TWKilled.
# TYPE node_netstat_TcpExt_TWKilled untyped
node_netstat_TcpExt_TWKilled 0
# HELP node_netstat_TcpExt_TWRecycled Protocol TcpExt statistic TWRecycled.
# TYPE node_netstat_TcpExt_TWRecycled untyped
node_netstat_TcpExt_TWRecycled 0
# HELP node_netstat_TcpExt_TcpDuplicateDataRehash Protocol TcpExt statistic TcpDuplicateDataRehash.
# TYPE node_netstat_TcpExt_TcpDuplicateDataRehash untyped
node_netstat_TcpExt_TcpDuplicateDataRehash 0
# HELP node_netstat_TcpExt_TcpTimeoutRehash Protocol TcpExt statistic TcpTimeoutRehash.
# TYPE node_netstat_TcpExt_TcpTimeoutRehash untyped
node_netstat_TcpExt_TcpTimeoutRehash 1
# HELP node_netstat_Tcp_ActiveOpens Protocol Tcp statistic ActiveOpens.
# TYPE node_netstat_Tcp_ActiveOpens untyped
node_netstat_Tcp_ActiveOpens 62
# HELP node_netstat_Tcp_AttemptFails Protocol Tcp statistic AttemptFails.
# TYPE node_netstat_Tcp_AttemptFails untyped
node_netstat_Tcp_AttemptFails 4
# HELP node_netstat_Tcp_CurrEstab Protocol Tcp statistic CurrEstab.
# TYPE node_netstat_Tcp_CurrEstab untyped
node_netstat_Tcp_CurrEstab 14
# HELP node_netstat_Tcp_EstabResets Protocol Tcp statistic EstabResets.
# TYPE node_netstat_Tcp_EstabResets untyped
node_netstat_Tcp_EstabResets 3
# HELP node_netstat_Tcp_InCsumErrors Protocol Tcp statistic InCsumErrors.
# TYPE node_netstat_Tcp_InCsumErrors untyped
node_netstat_Tcp_InCsumErrors 0
# HELP node_netstat_Tcp_InErrs Protocol Tcp statistic InErrs.
# TYPE node_netstat_Tcp_InErrs untyped
node_netstat_Tcp_InErrs 1
# HELP node_netstat_Tcp_InSegs Protocol Tcp statistic InSegs.
# TYPE node_netstat_Tcp_InSegs untyped
node_netstat_Tcp_InSegs 125067
# HELP node_netstat_Tcp_MaxConn Protocol Tcp statistic MaxConn.
# TYPE node_netstat_Tcp_MaxConn untyped
node_netstat_Tcp_MaxConn -1
# HELP node_netstat_Tcp_OutRsts Protocol Tcp statistic OutRsts.
# TYPE node_netstat_Tcp_OutRsts untyped
node_netstat_Tcp_OutRsts 18
# HELP node_netstat_Tcp_OutSegs Protocol Tcp statistic OutSegs.
# TYPE node_netstat_Tcp_OutSegs untyped
node_netstat_Tcp_OutSegs 37247
# HELP node_netstat_Tcp_PassiveOpens Protocol Tcp statistic PassiveOpens.
# TYPE node_netstat_Tcp_PassiveOpens untyped
node_netstat_Tcp_PassiveOpens 510
# HELP node_netstat_Tcp_RetransSegs Protocol Tcp statistic RetransSegs.
# TYPE node_netstat_Tcp_RetransSegs untyped
node_netstat_Tcp_RetransSegs 3
# HELP node_netstat_Tcp_RtoAlgorithm Protocol Tcp statistic RtoAlgorithm.
# TYPE node_netstat_Tcp_RtoAlgorithm untyped
node_netstat_Tcp_RtoAlgorithm 1
# HELP node_netstat_Tcp_RtoMax Protocol Tcp statistic RtoMax.
# TYPE node_netstat_Tcp_RtoMax untyped
node_netstat_Tcp_RtoMax 120000
# HELP node_netstat_Tcp_RtoMin Protocol Tcp statistic RtoMin.
# TYPE node_netstat_Tcp_RtoMin untyped
node_netstat_Tcp_RtoMin 200
# HELP node_netstat_UdpLite_IgnoredMulti Protocol UdpLite statistic IgnoredMulti.
# TYPE node_netstat_UdpLite_IgnoredMulti untyped
node_netstat_UdpLite_IgnoredMulti 0
# HELP node_netstat_UdpLite_InCsumErrors Protocol UdpLite statistic InCsumErrors.
# TYPE node_netstat_UdpLite_InCsumErrors untyped
node_netstat_UdpLite_InCsumErrors 0
# HELP node_netstat_UdpLite_InDatagrams Protocol UdpLite statistic InDatagrams.
# TYPE node_netstat_UdpLite_InDatagrams untyped
node_netstat_UdpLite_InDatagrams 0
# HELP node_netstat_UdpLite_InErrors Protocol UdpLite statistic InErrors.
# TYPE node_netstat_UdpLite_InErrors untyped
node_netstat_UdpLite_InErrors 0
# HELP node_netstat_UdpLite_MemErrors Protocol UdpLite statistic MemErrors.
# TYPE node_netstat_UdpLite_MemErrors untyped
node_netstat_UdpLite_MemErrors 0
# HELP node_netstat_UdpLite_NoPorts Protocol UdpLite statistic NoPorts.
# TYPE node_netstat_UdpLite_NoPorts untyped
node_netstat_UdpLite_NoPorts 0
# HELP node_netstat_UdpLite_OutDatagrams Protocol UdpLite statistic OutDatagrams.
# TYPE node_netstat_UdpLite_OutDatagrams untyped
node_netstat_UdpLite_OutDatagrams 0
# HELP node_netstat_UdpLite_RcvbufErrors Protocol UdpLite statistic RcvbufErrors.
# TYPE node_netstat_UdpLite_RcvbufErrors untyped
node_netstat_UdpLite_RcvbufErrors 0
# HELP node_netstat_UdpLite_SndbufErrors Protocol UdpLite statistic SndbufErrors.
# TYPE node_netstat_UdpLite_SndbufErrors untyped
node_netstat_UdpLite_SndbufErrors 0
# HELP node_netstat_Udp_IgnoredMulti Protocol Udp statistic IgnoredMulti.
# TYPE node_netstat_Udp_IgnoredMulti untyped
node_netstat_Udp_IgnoredMulti 0
# HELP node_netstat_Udp_InCsumErrors Protocol Udp statistic InCsumErrors.
# TYPE node_netstat_Udp_InCsumErrors untyped
node_netstat_Udp_InCsumErrors 0
# HELP node_netstat_Udp_InDatagrams Protocol Udp statistic InDatagrams.
# TYPE node_netstat_Udp_InDatagrams untyped
node_netstat_Udp_InDatagrams 961
# HELP node_netstat_Udp_InErrors Protocol Udp statistic InErrors.
# TYPE node_netstat_Udp_InErrors untyped
node_netstat_Udp_InErrors 0
# HELP node_netstat_Udp_MemErrors Protocol Udp statistic MemErrors.
# TYPE node_netstat_Udp_MemErrors untyped
node_netstat_Udp_MemErrors 0
# HELP node_netstat_Udp_NoPorts Protocol Udp statistic NoPorts.
# TYPE node_netstat_Udp_NoPorts untyped
node_netstat_Udp_NoPorts 0
# HELP node_netstat_Udp_OutDatagrams Protocol Udp statistic OutDatagrams.
# TYPE node_netstat_Udp_OutDatagrams untyped
node_netstat_Udp_OutDatagrams 1074
# HELP node_netstat_Udp_RcvbufErrors Protocol Udp statistic RcvbufErrors.
# TYPE node_netstat_Udp_RcvbufErrors untyped
node_netstat_Udp_RcvbufErrors 0
# HELP node_netstat_Udp_SndbufErrors Protocol Udp statistic SndbufErrors.
# TYPE node_netstat_Udp_SndbufErrors untyped
node_netstat_Udp_SndbufErrors 0
# HELP node_network_receive_bytes Network device statistic receive_bytes.
# TYPE node_network_receive_bytes gauge
node_network_receive_bytes{device="docker0"} 0
node_network_receive_bytes{device="ens3"} 3.32401879e+08
node_network_receive_bytes{device="lo"} 513990
# HELP node_network_receive_compressed Network device statistic receive_compressed.
# TYPE node_network_receive_compressed gauge
node_network_receive_compressed{device="docker0"} 0
node_network_receive_compressed{device="ens3"} 0
node_network_receive_compressed{device="lo"} 0
# HELP node_network_receive_drop Network device statistic receive_drop.
# TYPE node_network_receive_drop gauge
node_network_receive_drop{device="docker0"} 0
node_network_receive_drop{device="ens3"} 0
node_network_receive_drop{device="lo"} 0
# HELP node_network_receive_errs Network device statistic receive_errs.
# TYPE node_network_receive_errs gauge
node_network_receive_errs{device="docker0"} 0
node_network_receive_errs{device="ens3"} 0
node_network_receive_errs{device="lo"} 0
# HELP node_network_receive_fifo Network device statistic receive_fifo.
# TYPE node_network_receive_fifo gauge
node_network_receive_fifo{device="docker0"} 0
node_network_receive_fifo{device="ens3"} 0
node_network_receive_fifo{device="lo"} 0
# HELP node_network_receive_frame Network device statistic receive_frame.
# TYPE node_network_receive_frame gauge
node_network_receive_frame{device="docker0"} 0
node_network_receive_frame{device="ens3"} 0
node_network_receive_frame{device="lo"} 0
# HELP node_network_receive_multicast Network device statistic receive_multicast.
# TYPE node_network_receive_multicast gauge
node_network_receive_multicast{device="docker0"} 0
node_network_receive_multicast{device="ens3"} 0
node_network_receive_multicast{device="lo"} 0
# HELP node_network_receive_packets Network device statistic receive_packets.
# TYPE node_network_receive_packets gauge
node_network_receive_packets{device="docker0"} 0
node_network_receive_packets{device="ens3"} 231485
node_network_receive_packets{device="lo"} 525
# HELP node_network_transmit_bytes Network device statistic transmit_bytes.
# TYPE node_network_transmit_bytes gauge
node_network_transmit_bytes{device="docker0"} 0
node_network_transmit_bytes{device="ens3"} 1.3907474e+07
node_network_transmit_bytes{device="lo"} 513990
# HELP node_network_transmit_compressed Network device statistic transmit_compressed.
# TYPE node_network_transmit_compressed gauge
node_network_transmit_compressed{device="docker0"} 0
node_network_transmit_compressed{device="ens3"} 0
node_network_transmit_compressed{device="lo"} 0
# HELP node_network_transmit_drop Network device statistic transmit_drop.
# TYPE node_network_transmit_drop gauge
node_network_transmit_drop{device="docker0"} 0
node_network_transmit_drop{device="ens3"} 0
node_network_transmit_drop{device="lo"} 0
# HELP node_network_transmit_errs Network device statistic transmit_errs.
# TYPE node_network_transmit_errs gauge
node_network_transmit_errs{device="docker0"} 0
node_network_transmit_errs{device="ens3"} 0
node_network_transmit_errs{device="lo"} 0
# HELP node_network_transmit_fifo Network device statistic transmit_fifo.
# TYPE node_network_transmit_fifo gauge
node_network_transmit_fifo{device="docker0"} 0
node_network_transmit_fifo{device="ens3"} 0
node_network_transmit_fifo{device="lo"} 0
# HELP node_network_transmit_frame Network device statistic transmit_frame.
# TYPE node_network_transmit_frame gauge
node_network_transmit_frame{device="docker0"} 0
node_network_transmit_frame{device="ens3"} 0
node_network_transmit_frame{device="lo"} 0
# HELP node_network_transmit_multicast Network device statistic transmit_multicast.
# TYPE node_network_transmit_multicast gauge
node_network_transmit_multicast{device="docker0"} 0
node_network_transmit_multicast{device="ens3"} 0
node_network_transmit_multicast{device="lo"} 0
# HELP node_network_transmit_packets Network device statistic transmit_packets.
# TYPE node_network_transmit_packets gauge
node_network_transmit_packets{device="docker0"} 0
node_network_transmit_packets{device="ens3"} 31479
node_network_transmit_packets{device="lo"} 525
# HELP node_nf_conntrack_entries Number of currently allocated flow entries for connection tracking.
# TYPE node_nf_conntrack_entries gauge
node_nf_conntrack_entries 32
# HELP node_nf_conntrack_entries_limit Maximum size of connection tracking table.
# TYPE node_nf_conntrack_entries_limit gauge
node_nf_conntrack_entries_limit 65536
# HELP node_procs_blocked Number of processes blocked waiting for I/O to complete.
# TYPE node_procs_blocked gauge
node_procs_blocked 0
# HELP node_procs_running Number of processes in runnable state.
# TYPE node_procs_running gauge
node_procs_running 3
# HELP node_sockstat_FRAG_inuse Number of FRAG sockets in state inuse.
# TYPE node_sockstat_FRAG_inuse gauge
node_sockstat_FRAG_inuse 0
# HELP node_sockstat_FRAG_memory Number of FRAG sockets in state memory.
# TYPE node_sockstat_FRAG_memory gauge
node_sockstat_FRAG_memory 0
# HELP node_sockstat_RAW_inuse Number of RAW sockets in state inuse.
# TYPE node_sockstat_RAW_inuse gauge
node_sockstat_RAW_inuse 0
# HELP node_sockstat_TCP_alloc Number of TCP sockets in state alloc.
# TYPE node_sockstat_TCP_alloc gauge
node_sockstat_TCP_alloc 23
# HELP node_sockstat_TCP_inuse Number of TCP sockets in state inuse.
# TYPE node_sockstat_TCP_inuse gauge
node_sockstat_TCP_inuse 14
# HELP node_sockstat_TCP_mem Number of TCP sockets in state mem.
# TYPE node_sockstat_TCP_mem gauge
node_sockstat_TCP_mem 4
# HELP node_sockstat_TCP_mem_bytes Number of TCP sockets in state mem_bytes.
# TYPE node_sockstat_TCP_mem_bytes gauge
node_sockstat_TCP_mem_bytes 16384
# HELP node_sockstat_TCP_orphan Number of TCP sockets in state orphan.
# TYPE node_sockstat_TCP_orphan gauge
node_sockstat_TCP_orphan 0
# HELP node_sockstat_TCP_tw Number of TCP sockets in state tw.
# TYPE node_sockstat_TCP_tw gauge
node_sockstat_TCP_tw 9
# HELP node_sockstat_UDPLITE_inuse Number of UDPLITE sockets in state inuse.
# TYPE node_sockstat_UDPLITE_inuse gauge
node_sockstat_UDPLITE_inuse 0
# HELP node_sockstat_UDP_inuse Number of UDP sockets in state inuse.
# TYPE node_sockstat_UDP_inuse gauge
node_sockstat_UDP_inuse 6
# HELP node_sockstat_UDP_mem Number of UDP sockets in state mem.
# TYPE node_sockstat_UDP_mem gauge
node_sockstat_UDP_mem 2
# HELP node_sockstat_UDP_mem_bytes Number of UDP sockets in state mem_bytes.
# TYPE node_sockstat_UDP_mem_bytes gauge
node_sockstat_UDP_mem_bytes 8192
# HELP node_sockstat_sockets_used Number of sockets sockets in state used.
# TYPE node_sockstat_sockets_used gauge
node_sockstat_sockets_used 241
# HELP node_time System time in seconds since epoch (1970).
# TYPE node_time counter
node_time 1.776539165e+09
# HELP node_uname_info Labeled system information as provided by the uname system call.
# TYPE node_uname_info gauge
node_uname_info{domainname="(none)",machine="x86_64",nodename="host01",release="5.15.0-72-generic",sysname="Linux",version="#79-Ubuntu SMP Wed Apr 19 08:22:18 UTC 2023"} 1
# HELP node_vmstat_allocstall_dma /proc/vmstat information field allocstall_dma.
# TYPE node_vmstat_allocstall_dma untyped
node_vmstat_allocstall_dma 0
# HELP node_vmstat_allocstall_dma32 /proc/vmstat information field allocstall_dma32.
# TYPE node_vmstat_allocstall_dma32 untyped
node_vmstat_allocstall_dma32 0
# HELP node_vmstat_allocstall_movable /proc/vmstat information field allocstall_movable.
# TYPE node_vmstat_allocstall_movable untyped
node_vmstat_allocstall_movable 0
# HELP node_vmstat_allocstall_normal /proc/vmstat information field allocstall_normal.
# TYPE node_vmstat_allocstall_normal untyped
node_vmstat_allocstall_normal 0
# HELP node_vmstat_balloon_deflate /proc/vmstat information field balloon_deflate.
# TYPE node_vmstat_balloon_deflate untyped
node_vmstat_balloon_deflate 0
# HELP node_vmstat_balloon_inflate /proc/vmstat information field balloon_inflate.
# TYPE node_vmstat_balloon_inflate untyped
node_vmstat_balloon_inflate 0
# HELP node_vmstat_balloon_migrate /proc/vmstat information field balloon_migrate.
# TYPE node_vmstat_balloon_migrate untyped
node_vmstat_balloon_migrate 0
# HELP node_vmstat_compact_daemon_free_scanned /proc/vmstat information field compact_daemon_free_scanned.
# TYPE node_vmstat_compact_daemon_free_scanned untyped
node_vmstat_compact_daemon_free_scanned 0
# HELP node_vmstat_compact_daemon_migrate_scanned /proc/vmstat information field compact_daemon_migrate_scanned.
# TYPE node_vmstat_compact_daemon_migrate_scanned untyped
node_vmstat_compact_daemon_migrate_scanned 0
# HELP node_vmstat_compact_daemon_wake /proc/vmstat information field compact_daemon_wake.
# TYPE node_vmstat_compact_daemon_wake untyped
node_vmstat_compact_daemon_wake 2
# HELP node_vmstat_compact_fail /proc/vmstat information field compact_fail.
# TYPE node_vmstat_compact_fail untyped
node_vmstat_compact_fail 0
# HELP node_vmstat_compact_free_scanned /proc/vmstat information field compact_free_scanned.
# TYPE node_vmstat_compact_free_scanned untyped
node_vmstat_compact_free_scanned 0
# HELP node_vmstat_compact_isolated /proc/vmstat information field compact_isolated.
# TYPE node_vmstat_compact_isolated untyped
node_vmstat_compact_isolated 0
# HELP node_vmstat_compact_migrate_scanned /proc/vmstat information field compact_migrate_scanned.
# TYPE node_vmstat_compact_migrate_scanned untyped
node_vmstat_compact_migrate_scanned 0
# HELP node_vmstat_compact_stall /proc/vmstat information field compact_stall.
# TYPE node_vmstat_compact_stall untyped
node_vmstat_compact_stall 0
# HELP node_vmstat_compact_success /proc/vmstat information field compact_success.
# TYPE node_vmstat_compact_success untyped
node_vmstat_compact_success 0
# HELP node_vmstat_direct_map_level2_splits /proc/vmstat information field direct_map_level2_splits.
# TYPE node_vmstat_direct_map_level2_splits untyped
node_vmstat_direct_map_level2_splits 65
# HELP node_vmstat_direct_map_level3_splits /proc/vmstat information field direct_map_level3_splits.
# TYPE node_vmstat_direct_map_level3_splits untyped
node_vmstat_direct_map_level3_splits 0
# HELP node_vmstat_drop_pagecache /proc/vmstat information field drop_pagecache.
# TYPE node_vmstat_drop_pagecache untyped
node_vmstat_drop_pagecache 0
# HELP node_vmstat_drop_slab /proc/vmstat information field drop_slab.
# TYPE node_vmstat_drop_slab untyped
node_vmstat_drop_slab 0
# HELP node_vmstat_htlb_buddy_alloc_fail /proc/vmstat information field htlb_buddy_alloc_fail.
# TYPE node_vmstat_htlb_buddy_alloc_fail untyped
node_vmstat_htlb_buddy_alloc_fail 0
# HELP node_vmstat_htlb_buddy_alloc_success /proc/vmstat information field htlb_buddy_alloc_success.
# TYPE node_vmstat_htlb_buddy_alloc_success untyped
node_vmstat_htlb_buddy_alloc_success 0
# HELP node_vmstat_kswapd_high_wmark_hit_quickly /proc/vmstat information field kswapd_high_wmark_hit_quickly.
# TYPE node_vmstat_kswapd_high_wmark_hit_quickly untyped
node_vmstat_kswapd_high_wmark_hit_quickly 12
# HELP node_vmstat_kswapd_inodesteal /proc/vmstat information field kswapd_inodesteal.
# TYPE node_vmstat_kswapd_inodesteal untyped
node_vmstat_kswapd_inodesteal 0
# HELP node_vmstat_kswapd_low_wmark_hit_quickly /proc/vmstat information field kswapd_low_wmark_hit_quickly.
# TYPE node_vmstat_kswapd_low_wmark_hit_quickly untyped
node_vmstat_kswapd_low_wmark_hit_quickly 53
# HELP node_vmstat_nr_active_anon /proc/vmstat information field nr_active_anon.
# TYPE node_vmstat_nr_active_anon untyped
node_vmstat_nr_active_anon 280
# HELP node_vmstat_nr_active_file /proc/vmstat information field nr_active_file.
# TYPE node_vmstat_nr_active_file untyped
node_vmstat_nr_active_file 65683
# HELP node_vmstat_nr_anon_pages /proc/vmstat information field nr_anon_pages.
# TYPE node_vmstat_nr_anon_pages untyped
node_vmstat_nr_anon_pages 78881
# HELP node_vmstat_nr_anon_transparent_hugepages /proc/vmstat information field nr_anon_transparent_hugepages.
# TYPE node_vmstat_nr_anon_transparent_hugepages untyped
node_vmstat_nr_anon_transparent_hugepages 0
# HELP node_vmstat_nr_bounce /proc/vmstat information field nr_bounce.
# TYPE node_vmstat_nr_bounce untyped
node_vmstat_nr_bounce 0
# HELP node_vmstat_nr_dirtied /proc/vmstat information field nr_dirtied.
# TYPE node_vmstat_nr_dirtied untyped
node_vmstat_nr_dirtied 257511
# HELP node_vmstat_nr_dirty /proc/vmstat information field nr_dirty.
# TYPE node_vmstat_nr_dirty untyped
node_vmstat_nr_dirty 6
# HELP node_vmstat_nr_dirty_background_threshold /proc/vmstat information field nr_dirty_background_threshold.
# TYPE node_vmstat_nr_dirty_background_threshold untyped
node_vmstat_nr_dirty_background_threshold 22925
# HELP node_vmstat_nr_dirty_threshold /proc/vmstat information field nr_dirty_threshold.
# TYPE node_vmstat_nr_dirty_threshold untyped
node_vmstat_nr_dirty_threshold 45907
# HELP node_vmstat_nr_file_hugepages /proc/vmstat information field nr_file_hugepages.
# TYPE node_vmstat_nr_file_hugepages untyped
node_vmstat_nr_file_hugepages 0
# HELP node_vmstat_nr_file_pages /proc/vmstat information field nr_file_pages.
# TYPE node_vmstat_nr_file_pages untyped
node_vmstat_nr_file_pages 229847
# HELP node_vmstat_nr_file_pmdmapped /proc/vmstat information field nr_file_pmdmapped.
# TYPE node_vmstat_nr_file_pmdmapped untyped
node_vmstat_nr_file_pmdmapped 0
# HELP node_vmstat_nr_foll_pin_acquired /proc/vmstat information field nr_foll_pin_acquired.
# TYPE node_vmstat_nr_foll_pin_acquired untyped
node_vmstat_nr_foll_pin_acquired 0
# HELP node_vmstat_nr_foll_pin_released /proc/vmstat information field nr_foll_pin_released.
# TYPE node_vmstat_nr_foll_pin_released untyped
node_vmstat_nr_foll_pin_released 0
# HELP node_vmstat_nr_free_cma /proc/vmstat information field nr_free_cma.
# TYPE node_vmstat_nr_free_cma untyped
node_vmstat_nr_free_cma 0
# HELP node_vmstat_nr_free_pages /proc/vmstat information field nr_free_pages.
# TYPE node_vmstat_nr_free_pages untyped
node_vmstat_nr_free_pages 20496
# HELP node_vmstat_nr_inactive_anon /proc/vmstat information field nr_inactive_anon.
# TYPE node_vmstat_nr_inactive_anon untyped
node_vmstat_nr_inactive_anon 74177
# HELP node_vmstat_nr_inactive_file /proc/vmstat information field nr_inactive_file.
# TYPE node_vmstat_nr_inactive_file untyped
node_vmstat_nr_inactive_file 161612
# HELP node_vmstat_nr_isolated_anon /proc/vmstat information field nr_isolated_anon.
# TYPE node_vmstat_nr_isolated_anon untyped
node_vmstat_nr_isolated_anon 0
# HELP node_vmstat_nr_isolated_file /proc/vmstat information field nr_isolated_file.
# TYPE node_vmstat_nr_isolated_file untyped
node_vmstat_nr_isolated_file 0
# HELP node_vmstat_nr_kernel_misc_reclaimable /proc/vmstat information field nr_kernel_misc_reclaimable.
# TYPE node_vmstat_nr_kernel_misc_reclaimable untyped
node_vmstat_nr_kernel_misc_reclaimable 0
# HELP node_vmstat_nr_kernel_stack /proc/vmstat information field nr_kernel_stack.
# TYPE node_vmstat_nr_kernel_stack untyped
node_vmstat_nr_kernel_stack 4032
# HELP node_vmstat_nr_mapped /proc/vmstat information field nr_mapped.
# TYPE node_vmstat_nr_mapped untyped
node_vmstat_nr_mapped 67333
# HELP node_vmstat_nr_mlock /proc/vmstat information field nr_mlock.
# TYPE node_vmstat_nr_mlock untyped
node_vmstat_nr_mlock 6950
# HELP node_vmstat_nr_page_table_pages /proc/vmstat information field nr_page_table_pages.
# TYPE node_vmstat_nr_page_table_pages untyped
node_vmstat_nr_page_table_pages 2184
# HELP node_vmstat_nr_shmem /proc/vmstat information field nr_shmem.
# TYPE node_vmstat_nr_shmem untyped
node_vmstat_nr_shmem 286
# HELP node_vmstat_nr_shmem_hugepages /proc/vmstat information field nr_shmem_hugepages.
# TYPE node_vmstat_nr_shmem_hugepages untyped
node_vmstat_nr_shmem_hugepages 0
# HELP node_vmstat_nr_shmem_pmdmapped /proc/vmstat information field nr_shmem_pmdmapped.
# TYPE node_vmstat_nr_shmem_pmdmapped untyped
node_vmstat_nr_shmem_pmdmapped 0
# HELP node_vmstat_nr_slab_reclaimable /proc/vmstat information field nr_slab_reclaimable.
# TYPE node_vmstat_nr_slab_reclaimable untyped
node_vmstat_nr_slab_reclaimable 12489
# HELP node_vmstat_nr_slab_unreclaimable /proc/vmstat information field nr_slab_unreclaimable.
# TYPE node_vmstat_nr_slab_unreclaimable untyped
node_vmstat_nr_slab_unreclaimable 11457
# HELP node_vmstat_nr_swapcached /proc/vmstat information field nr_swapcached.
# TYPE node_vmstat_nr_swapcached untyped
node_vmstat_nr_swapcached 0
# HELP node_vmstat_nr_unevictable /proc/vmstat information field nr_unevictable.
# TYPE node_vmstat_nr_unevictable untyped
node_vmstat_nr_unevictable 6950
# HELP node_vmstat_nr_unstable /proc/vmstat information field nr_unstable.
# TYPE node_vmstat_nr_unstable untyped
node_vmstat_nr_unstable 0
# HELP node_vmstat_nr_vmscan_immediate_reclaim /proc/vmstat information field nr_vmscan_immediate_reclaim.
# TYPE node_vmstat_nr_vmscan_immediate_reclaim untyped
node_vmstat_nr_vmscan_immediate_reclaim 4
# HELP node_vmstat_nr_vmscan_write /proc/vmstat information field nr_vmscan_write.
# TYPE node_vmstat_nr_vmscan_write untyped
node_vmstat_nr_vmscan_write 0
# HELP node_vmstat_nr_writeback /proc/vmstat information field nr_writeback.
# TYPE node_vmstat_nr_writeback untyped
node_vmstat_nr_writeback 21
# HELP node_vmstat_nr_writeback_temp /proc/vmstat information field nr_writeback_temp.
# TYPE node_vmstat_nr_writeback_temp untyped
node_vmstat_nr_writeback_temp 0
# HELP node_vmstat_nr_written /proc/vmstat information field nr_written.
# TYPE node_vmstat_nr_written untyped
node_vmstat_nr_written 245994
# HELP node_vmstat_nr_zone_active_anon /proc/vmstat information field nr_zone_active_anon.
# TYPE node_vmstat_nr_zone_active_anon untyped
node_vmstat_nr_zone_active_anon 280
# HELP node_vmstat_nr_zone_active_file /proc/vmstat information field nr_zone_active_file.
# TYPE node_vmstat_nr_zone_active_file untyped
node_vmstat_nr_zone_active_file 65683
# HELP node_vmstat_nr_zone_inactive_anon /proc/vmstat information field nr_zone_inactive_anon.
# TYPE node_vmstat_nr_zone_inactive_anon untyped
node_vmstat_nr_zone_inactive_anon 74177
# HELP node_vmstat_nr_zone_inactive_file /proc/vmstat information field nr_zone_inactive_file.
# TYPE node_vmstat_nr_zone_inactive_file untyped
node_vmstat_nr_zone_inactive_file 161612
# HELP node_vmstat_nr_zone_unevictable /proc/vmstat information field nr_zone_unevictable.
# TYPE node_vmstat_nr_zone_unevictable untyped
node_vmstat_nr_zone_unevictable 6950
# HELP node_vmstat_nr_zone_write_pending /proc/vmstat information field nr_zone_write_pending.
# TYPE node_vmstat_nr_zone_write_pending untyped
node_vmstat_nr_zone_write_pending 18
# HELP node_vmstat_nr_zspages /proc/vmstat information field nr_zspages.
# TYPE node_vmstat_nr_zspages untyped
node_vmstat_nr_zspages 0
# HELP node_vmstat_numa_foreign /proc/vmstat information field numa_foreign.
# TYPE node_vmstat_numa_foreign untyped
node_vmstat_numa_foreign 0
# HELP node_vmstat_numa_hint_faults /proc/vmstat information field numa_hint_faults.
# TYPE node_vmstat_numa_hint_faults untyped
node_vmstat_numa_hint_faults 0
# HELP node_vmstat_numa_hint_faults_local /proc/vmstat information field numa_hint_faults_local.
# TYPE node_vmstat_numa_hint_faults_local untyped
node_vmstat_numa_hint_faults_local 0
# HELP node_vmstat_numa_hit /proc/vmstat information field numa_hit.
# TYPE node_vmstat_numa_hit untyped
node_vmstat_numa_hit 1.93139e+06
# HELP node_vmstat_numa_huge_pte_updates /proc/vmstat information field numa_huge_pte_updates.
# TYPE node_vmstat_numa_huge_pte_updates untyped
node_vmstat_numa_huge_pte_updates 0
# HELP node_vmstat_numa_interleave /proc/vmstat information field numa_interleave.
# TYPE node_vmstat_numa_interleave untyped
node_vmstat_numa_interleave 2612
# HELP node_vmstat_numa_local /proc/vmstat information field numa_local.
# TYPE node_vmstat_numa_local untyped
node_vmstat_numa_local 1.93139e+06
# HELP node_vmstat_numa_miss /proc/vmstat information field numa_miss.
# TYPE node_vmstat_numa_miss untyped
node_vmstat_numa_miss 0
# HELP node_vmstat_numa_other /proc/vmstat information field numa_other.
# TYPE node_vmstat_numa_other untyped
node_vmstat_numa_other 0
# HELP node_vmstat_numa_pages_migrated /proc/vmstat information field numa_pages_migrated.
# TYPE node_vmstat_numa_pages_migrated untyped
node_vmstat_numa_pages_migrated 0
# HELP node_vmstat_numa_pte_updates /proc/vmstat information field numa_pte_updates.
# TYPE node_vmstat_numa_pte_updates untyped
node_vmstat_numa_pte_updates 0
# HELP node_vmstat_oom_kill /proc/vmstat information field oom_kill.
# TYPE node_vmstat_oom_kill untyped
node_vmstat_oom_kill 0
# HELP node_vmstat_pageoutrun /proc/vmstat information field pageoutrun.
# TYPE node_vmstat_pageoutrun untyped
node_vmstat_pageoutrun 75
# HELP node_vmstat_pgactivate /proc/vmstat information field pgactivate.
# TYPE node_vmstat_pgactivate untyped
node_vmstat_pgactivate 104423
# HELP node_vmstat_pgalloc_dma /proc/vmstat information field pgalloc_dma.
# TYPE node_vmstat_pgalloc_dma untyped
node_vmstat_pgalloc_dma 9300
# HELP node_vmstat_pgalloc_dma32 /proc/vmstat information field pgalloc_dma32.
# TYPE node_vmstat_pgalloc_dma32 untyped
node_vmstat_pgalloc_dma32 1.938712e+06
# HELP node_vmstat_pgalloc_movable /proc/vmstat information field pgalloc_movable.
# TYPE node_vmstat_pgalloc_movable untyped
node_vmstat_pgalloc_movable 0
# HELP node_vmstat_pgalloc_normal /proc/vmstat information field pgalloc_normal.
# TYPE node_vmstat_pgalloc_normal untyped
node_vmstat_pgalloc_normal 0
# HELP node_vmstat_pgdeactivate /proc/vmstat information field pgdeactivate.
# TYPE node_vmstat_pgdeactivate untyped
node_vmstat_pgdeactivate 46892
# HELP node_vmstat_pgdemote_direct /proc/vmstat information field pgdemote_direct.
# TYPE node_vmstat_pgdemote_direct untyped
node_vmstat_pgdemote_direct 0
# HELP node_vmstat_pgdemote_kswapd /proc/vmstat information field pgdemote_kswapd.
# TYPE node_vmstat_pgdemote_kswapd untyped
node_vmstat_pgdemote_kswapd 0
# HELP node_vmstat_pgfault /proc/vmstat information field pgfault.
# TYPE node_vmstat_pgfault untyped
node_vmstat_pgfault 1.919629e+06
# HELP node_vmstat_pgfree /proc/vmstat information field pgfree.
# TYPE node_vmstat_pgfree untyped
node_vmstat_pgfree 1.981743e+06
# HELP node_vmstat_pginodesteal /proc/vmstat information field pginodesteal.
# TYPE node_vmstat_pginodesteal untyped
node_vmstat_pginodesteal 0
# HELP node_vmstat_pglazyfree /proc/vmstat information field pglazyfree.
# TYPE node_vmstat_pglazyfree untyped
node_vmstat_pglazyfree 0
# HELP node_vmstat_pglazyfreed /proc/vmstat information field pglazyfreed.
# TYPE node_vmstat_pglazyfreed untyped
node_vmstat_pglazyfreed 0
# HELP node_vmstat_pgmajfault /proc/vmstat information field pgmajfault.
# TYPE node_vmstat_pgmajfault untyped
node_vmstat_pgmajfault 3138
# HELP node_vmstat_pgmigrate_fail /proc/vmstat information field pgmigrate_fail.
# TYPE node_vmstat_pgmigrate_fail untyped
node_vmstat_pgmigrate_fail 0
# HELP node_vmstat_pgmigrate_success /proc/vmstat information field pgmigrate_success.
# TYPE node_vmstat_pgmigrate_success untyped
node_vmstat_pgmigrate_success 0
# HELP node_vmstat_pgpgin /proc/vmstat information field pgpgin.
# TYPE node_vmstat_pgpgin untyped
node_vmstat_pgpgin 711212
# HELP node_vmstat_pgpgout /proc/vmstat information field pgpgout.
# TYPE node_vmstat_pgpgout untyped
node_vmstat_pgpgout 1.036964e+06
# HELP node_vmstat_pgrefill /proc/vmstat information field pgrefill.
# TYPE node_vmstat_pgrefill untyped
node_vmstat_pgrefill 49498
# HELP node_vmstat_pgreuse /proc/vmstat information field pgreuse.
# TYPE node_vmstat_pgreuse untyped
node_vmstat_pgreuse 309660
# HELP node_vmstat_pgrotated /proc/vmstat information field pgrotated.
# TYPE node_vmstat_pgrotated untyped
node_vmstat_pgrotated 8
# HELP node_vmstat_pgscan_anon /proc/vmstat information field pgscan_anon.
# TYPE node_vmstat_pgscan_anon untyped
node_vmstat_pgscan_anon 0
# HELP node_vmstat_pgscan_direct /proc/vmstat information field pgscan_direct.
# TYPE node_vmstat_pgscan_direct untyped
node_vmstat_pgscan_direct 0
# HELP node_vmstat_pgscan_direct_throttle /proc/vmstat information field pgscan_direct_throttle.
# TYPE node_vmstat_pgscan_direct_throttle untyped
node_vmstat_pgscan_direct_throttle 0
# HELP node_vmstat_pgscan_file /proc/vmstat information field pgscan_file.
# TYPE node_vmstat_pgscan_file untyped
node_vmstat_pgscan_file 166553
# HELP node_vmstat_pgscan_kswapd /proc/vmstat information field pgscan_kswapd.
# TYPE node_vmstat_pgscan_kswapd untyped
node_vmstat_pgscan_kswapd 166553
# HELP node_vmstat_pgskip_dma /proc/vmstat information field pgskip_dma.
# TYPE node_vmstat_pgskip_dma untyped
node_vmstat_pgskip_dma 0
# HELP node_vmstat_pgskip_dma32 /proc/vmstat information field pgskip_dma32.
# TYPE node_vmstat_pgskip_dma32 untyped
node_vmstat_pgskip_dma32 0
# HELP node_vmstat_pgskip_movable /proc/vmstat information field pgskip_movable.
# TYPE node_vmstat_pgskip_movable untyped
node_vmstat_pgskip_movable 0
# HELP node_vmstat_pgskip_normal /proc/vmstat information field pgskip_normal.
# TYPE node_vmstat_pgskip_normal untyped
node_vmstat_pgskip_normal 0
# HELP node_vmstat_pgsteal_anon /proc/vmstat information field pgsteal_anon.
# TYPE node_vmstat_pgsteal_anon untyped
node_vmstat_pgsteal_anon 0
# HELP node_vmstat_pgsteal_direct /proc/vmstat information field pgsteal_direct.
# TYPE node_vmstat_pgsteal_direct untyped
node_vmstat_pgsteal_direct 0
# HELP node_vmstat_pgsteal_file /proc/vmstat information field pgsteal_file.
# TYPE node_vmstat_pgsteal_file untyped
node_vmstat_pgsteal_file 124937
# HELP node_vmstat_pgsteal_kswapd /proc/vmstat information field pgsteal_kswapd.
# TYPE node_vmstat_pgsteal_kswapd untyped
node_vmstat_pgsteal_kswapd 124937
# HELP node_vmstat_pswpin /proc/vmstat information field pswpin.
# TYPE node_vmstat_pswpin untyped
node_vmstat_pswpin 0
# HELP node_vmstat_pswpout /proc/vmstat information field pswpout.
# TYPE node_vmstat_pswpout untyped
node_vmstat_pswpout 0
# HELP node_vmstat_slabs_scanned /proc/vmstat information field slabs_scanned.
# TYPE node_vmstat_slabs_scanned untyped
node_vmstat_slabs_scanned 8704
# HELP node_vmstat_swap_ra /proc/vmstat information field swap_ra.
# TYPE node_vmstat_swap_ra untyped
node_vmstat_swap_ra 0
# HELP node_vmstat_swap_ra_hit /proc/vmstat information field swap_ra_hit.
# TYPE node_vmstat_swap_ra_hit untyped
node_vmstat_swap_ra_hit 0
# HELP node_vmstat_thp_collapse_alloc /proc/vmstat information field thp_collapse_alloc.
# TYPE node_vmstat_thp_collapse_alloc untyped
node_vmstat_thp_collapse_alloc 0
# HELP node_vmstat_thp_collapse_alloc_failed /proc/vmstat information field thp_collapse_alloc_failed.
# TYPE node_vmstat_thp_collapse_alloc_failed untyped
node_vmstat_thp_collapse_alloc_failed 0
# HELP node_vmstat_thp_deferred_split_page /proc/vmstat information field thp_deferred_split_page.
# TYPE node_vmstat_thp_deferred_split_page untyped
node_vmstat_thp_deferred_split_page 0
# HELP node_vmstat_thp_fault_alloc /proc/vmstat information field thp_fault_alloc.
# TYPE node_vmstat_thp_fault_alloc untyped
node_vmstat_thp_fault_alloc 0
# HELP node_vmstat_thp_fault_fallback /proc/vmstat information field thp_fault_fallback.
# TYPE node_vmstat_thp_fault_fallback untyped
node_vmstat_thp_fault_fallback 0
# HELP node_vmstat_thp_fault_fallback_charge /proc/vmstat information field thp_fault_fallback_charge.
# TYPE node_vmstat_thp_fault_fallback_charge untyped
node_vmstat_thp_fault_fallback_charge 0
# HELP node_vmstat_thp_file_alloc /proc/vmstat information field thp_file_alloc.
# TYPE node_vmstat_thp_file_alloc untyped
node_vmstat_thp_file_alloc 0
# HELP node_vmstat_thp_file_fallback /proc/vmstat information field thp_file_fallback.
# TYPE node_vmstat_thp_file_fallback untyped
node_vmstat_thp_file_fallback 0
# HELP node_vmstat_thp_file_fallback_charge /proc/vmstat information field thp_file_fallback_charge.
# TYPE node_vmstat_thp_file_fallback_charge untyped
node_vmstat_thp_file_fallback_charge 0
# HELP node_vmstat_thp_file_mapped /proc/vmstat information field thp_file_mapped.
# TYPE node_vmstat_thp_file_mapped untyped
node_vmstat_thp_file_mapped 0
# HELP node_vmstat_thp_migration_fail /proc/vmstat information field thp_migration_fail.
# TYPE node_vmstat_thp_migration_fail untyped
node_vmstat_thp_migration_fail 0
# HELP node_vmstat_thp_migration_split /proc/vmstat information field thp_migration_split.
# TYPE node_vmstat_thp_migration_split untyped
node_vmstat_thp_migration_split 0
# HELP node_vmstat_thp_migration_success /proc/vmstat information field thp_migration_success.
# TYPE node_vmstat_thp_migration_success untyped
node_vmstat_thp_migration_success 0
# HELP node_vmstat_thp_split_page /proc/vmstat information field thp_split_page.
# TYPE node_vmstat_thp_split_page untyped
node_vmstat_thp_split_page 0
# HELP node_vmstat_thp_split_page_failed /proc/vmstat information field thp_split_page_failed.
# TYPE node_vmstat_thp_split_page_failed untyped
node_vmstat_thp_split_page_failed 0
# HELP node_vmstat_thp_split_pmd /proc/vmstat information field thp_split_pmd.
# TYPE node_vmstat_thp_split_pmd untyped
node_vmstat_thp_split_pmd 0
# HELP node_vmstat_thp_split_pud /proc/vmstat information field thp_split_pud.
# TYPE node_vmstat_thp_split_pud untyped
node_vmstat_thp_split_pud 0
# HELP node_vmstat_thp_swpout /proc/vmstat information field thp_swpout.
# TYPE node_vmstat_thp_swpout untyped
node_vmstat_thp_swpout 0
# HELP node_vmstat_thp_swpout_fallback /proc/vmstat information field thp_swpout_fallback.
# TYPE node_vmstat_thp_swpout_fallback untyped
node_vmstat_thp_swpout_fallback 0
# HELP node_vmstat_thp_zero_page_alloc /proc/vmstat information field thp_zero_page_alloc.
# TYPE node_vmstat_thp_zero_page_alloc untyped
node_vmstat_thp_zero_page_alloc 0
# HELP node_vmstat_thp_zero_page_alloc_failed /proc/vmstat information field thp_zero_page_alloc_failed.
# TYPE node_vmstat_thp_zero_page_alloc_failed untyped
node_vmstat_thp_zero_page_alloc_failed 0
# HELP node_vmstat_unevictable_pgs_cleared /proc/vmstat information field unevictable_pgs_cleared.
# TYPE node_vmstat_unevictable_pgs_cleared untyped
node_vmstat_unevictable_pgs_cleared 0
# HELP node_vmstat_unevictable_pgs_culled /proc/vmstat information field unevictable_pgs_culled.
# TYPE node_vmstat_unevictable_pgs_culled untyped
node_vmstat_unevictable_pgs_culled 93133
# HELP node_vmstat_unevictable_pgs_mlocked /proc/vmstat information field unevictable_pgs_mlocked.
# TYPE node_vmstat_unevictable_pgs_mlocked untyped
node_vmstat_unevictable_pgs_mlocked 6957
# HELP node_vmstat_unevictable_pgs_munlocked /proc/vmstat information field unevictable_pgs_munlocked.
# TYPE node_vmstat_unevictable_pgs_munlocked untyped
node_vmstat_unevictable_pgs_munlocked 7
# HELP node_vmstat_unevictable_pgs_rescued /proc/vmstat information field unevictable_pgs_rescued.
# TYPE node_vmstat_unevictable_pgs_rescued untyped
node_vmstat_unevictable_pgs_rescued 7
# HELP node_vmstat_unevictable_pgs_scanned /proc/vmstat information field unevictable_pgs_scanned.
# TYPE node_vmstat_unevictable_pgs_scanned untyped
node_vmstat_unevictable_pgs_scanned 0
# HELP node_vmstat_unevictable_pgs_stranded /proc/vmstat information field unevictable_pgs_stranded.
# TYPE node_vmstat_unevictable_pgs_stranded untyped
node_vmstat_unevictable_pgs_stranded 0
# HELP node_vmstat_workingset_activate_anon /proc/vmstat information field workingset_activate_anon.
# TYPE node_vmstat_workingset_activate_anon untyped
node_vmstat_workingset_activate_anon 0
# HELP node_vmstat_workingset_activate_file /proc/vmstat information field workingset_activate_file.
# TYPE node_vmstat_workingset_activate_file untyped
node_vmstat_workingset_activate_file 24300
# HELP node_vmstat_workingset_nodereclaim /proc/vmstat information field workingset_nodereclaim.
# TYPE node_vmstat_workingset_nodereclaim untyped
node_vmstat_workingset_nodereclaim 512
# HELP node_vmstat_workingset_nodes /proc/vmstat information field workingset_nodes.
# TYPE node_vmstat_workingset_nodes untyped
node_vmstat_workingset_nodes 1033
# HELP node_vmstat_workingset_refault_anon /proc/vmstat information field workingset_refault_anon.
# TYPE node_vmstat_workingset_refault_anon untyped
node_vmstat_workingset_refault_anon 0
# HELP node_vmstat_workingset_refault_file /proc/vmstat information field workingset_refault_file.
# TYPE node_vmstat_workingset_refault_file untyped
node_vmstat_workingset_refault_file 28776
# HELP node_vmstat_workingset_restore_anon /proc/vmstat information field workingset_restore_anon.
# TYPE node_vmstat_workingset_restore_anon untyped
node_vmstat_workingset_restore_anon 0
# HELP node_vmstat_workingset_restore_file /proc/vmstat information field workingset_restore_file.
# TYPE node_vmstat_workingset_restore_file untyped
node_vmstat_workingset_restore_file 824
# HELP node_vmstat_zone_reclaim_failed /proc/vmstat information field zone_reclaim_failed.
# TYPE node_vmstat_zone_reclaim_failed untyped
node_vmstat_zone_reclaim_failed 0
# HELP process_cpu_seconds_total Total user and system CPU time spent in seconds.
# TYPE process_cpu_seconds_total counter
process_cpu_seconds_total 0.09
# HELP process_max_fds Maximum number of open file descriptors.
# TYPE process_max_fds gauge
process_max_fds 1.048576e+06
# HELP process_open_fds Number of open file descriptors.
# TYPE process_open_fds gauge
process_open_fds 8
# HELP process_resident_memory_bytes Resident memory size in bytes.
# TYPE process_resident_memory_bytes gauge
process_resident_memory_bytes 1.1771904e+07
# HELP process_start_time_seconds Start time of the process since unix epoch in seconds.
# TYPE process_start_time_seconds gauge
process_start_time_seconds 1.77653910151e+09
# HELP process_virtual_memory_bytes Virtual memory size in bytes.
# TYPE process_virtual_memory_bytes gauge
process_virtual_memory_bytes 1.8886656e+07
root@host01:~$ 

```

---

## Step 5 - View Metrics

With the containers launched, Prometheus will scrape and store the data based on the internals in the configuration.

#### **Dashboards**

The default Prometheus Dashboard reports internal information and provides a way to debug the metrics being collected. The dashboard can be viewed here

The dashboard will report the status of the scraping and the different targets at via the /targets page.

When running in production, you may wish to build a dashboard using Grafana, or a hosted Prometheus instance such as Weave Cortex.


#### **Query Prometheus**

To query the underlying metrics and create graphs, visit the graph page on the Dashboard at https://2886736901-9090-host08nc.environments.katacoda.com/graph

From here different metrics are queryable based on their name. This is done by adding a graph and entering a PromQL query or metrics label.

- For example, querying for 
```
engine_daemon_container_actions_seconds_sum
```
will show how many Docker actions are being performed. These actions are containers being started, deleted, created, committed, or changed.

<img width="3200" height="1896" alt="Image" src="https://github.com/user-attachments/assets/47a030c7-52b0-4356-9247-18515c58e9af" />



- The host metrics can be viewed too, for example, 

```
node_cpu

```
will output the Docker Hosts CPU information.

<img width="3200" height="1902" alt="Image" src="https://github.com/user-attachments/assets/107a6880-aa27-4c96-8384-0e2013184071" />


#### **Generate Metrics**

Running additional containers will result in changes to the metrics produced, which are viewable via the graphs and queries.






