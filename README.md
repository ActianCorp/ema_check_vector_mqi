# ema_check_vector_mqi
##Command line utility for  running a series of health-status checks across a Vector-H cluster
###(part of the Actian Enterprise Monitoring Application - EMA)

####Overview
This Linux BASH script can be used to check the health status of a number of metrics in a Vector-H cluster.

This application was initially developed as part of Actian Services' Enterprise Monitoring Appliance (EMA) - a solution developed to provide monitoring and alerting of Actian's products and based on the Nagios framework.

Although designed to be an integral part of the EMA, where montoring and alerting are provided, along with historic performance data capture and integration with other components of EMA, this application can also be used in isolation to check the health status of a cluster on an ad-hoc basis.

To support this application, a couple of other scripts are also required (mqi and ema_common_library_functions).  These are also provided within this project, and need to be present in the same directory as the ema_check_verctor_mqi application.

This application was initially developed to provide a mechanism by which commands could be invoked from a single node in a Vector in Hadoop (aka Vector-H or Vortex) or Matrix (aka ParAccel or PADB) cluster.  The results of which would then be picked up within the Actian Services' Enterprise Monitoring Appliance (EMA) - a solution developed to provide monitoring and alerting of Actian's products and based on the Nagios framework.

####Usage
```
./ema_check_vector_mqi

Usage:
  ema_check_vector_mqi
   -s|--system       {II_SYSTEM}
   -d|--database     {database name}
   -t|--test
      1 - Mounted disk space
      2 - LOG file size
      6 - SMARTCTL (Dell & Cisco) disk status
      7 - Network errors
      8 - SWAP usage
      9 - hard ulimit file descriptors
     10 - soft ulimit file descriptors
     11 - max files limits
     12 - per process inuse file descriptors
     13 - total inuse file descriptors
   -w|--warning        <WARNING (percent)>
   -c|--critical       <CRITICAL (percent)>
   -W|--WARNING        <WARNING (value)>
   -C|--CRITICAL       <CRITICAL (value)>

  ema_check_vector_mqi --help
  ema_check_vector_mqi --version
```

####Examples

The following list provides examples on how to invoke the ema_check_vector_mqi application. 

In these examples, the value of II_SYSTEM has been hard-coded (/opt/Actian/VectorX1) - although this could equally have been the variable $II_SYSTEM, had this been previously set.

Note that some of the "test" operate with thresholds being set using percentages (for warning and critical) whereas others are based on values.  To differentiate between the two, one must use the lower-case variant for percentages and the upper-case for values.

This is not ideal, and may in future be changed to accept a single pair.

```
./ema_check_vector_mqi --system /opt/Actian/VectorX1 --test 1 --warning 70 --critical 90
./ema_check_vector_mqi --system /opt/Actian/VectorX1 --test 2 --WARNING 1073741824 --CRITICAL 2147483648
./ema_check_vector_mqi --system /opt/Actian/VectorX1 --test 6
./ema_check_vector_mqi --system /opt/Actian/VectorX1 --test 7 --warning 10 --critical 20
./ema_check_vector_mqi --system /opt/Actian/VectorX1 --test 8 --warning 10 --critical 20
./ema_check_vector_mqi --system /opt/Actian/VectorX1 --test 9 --WARNING 10000 --CRITICAL 10000
./ema_check_vector_mqi --system /opt/Actian/VectorX1 --test 10 --WARNING 10000 --CRITICAL 10000
./ema_check_vector_mqi --system /opt/Actian/VectorX1 --test 11 --WARNING 10000 --CRITICAL 10000
./ema_check_vector_mqi --system /opt/Actian/VectorX1 --test 12 --warning 20 --critical 30
./ema_check_vector_mqi --system /opt/Actian/VectorX1 --test 13 --warning 20 --critical 30
```

####Sample output

The following section shows the output from executing each of the individual "tests" on one of our Vector-H 4.2.1 (Patch 16902) CentOS 6.4 clusters running Hortonworks.

Note that for each of the tests, three outputs are generated.

The first (and "invisible") output is the return (or exit) code of the application.  This has been designed to provide EMA with an overall health-status of running the application.

These return codes can be defined as:

```
0 - No errors were found
1 - One or more warning thresholds were breached
2 - One or more critical thresholds were breached
```

The second output is a single (albeit possiblly long) line.  This give a summary of the execution of the "test" and can be further broken into two components (separated by a single pipe "|" character):

```
1. The Standard message (which is used in EMA to give an overview status to the user)
2. Performance data and parameter values (again used by EMA to record historic data for long-term reporting)
```

The third (optional) component comprises of zero, one or more lines of detailed information pertaining the the test that has just been executed.


#####Test: 1 - Mounted disk space

```
File Systems OK: 95 File Systems Warning: 1  (MQI)|mounted_file_systems: file_systems_ok=95 file_systems_warn=1 file_systems_crit=0 PARAMETERS: TEST=1 WARNING_PERCENT=70 CRITICAL_PERCENT=90

Node: 172.16.68.2:
   OK: /dev/md0 (/) Capacity: 112G Used: 24G (23%) Free: 83G
   OK: /dev/sda3 (/boot) Capacity: 93M Used: 32M (37%) Free: 56M
   OK: /dev/sda2 (/mnt/d0) Capacity: 63G Used: 8.2G (14%) Free: 52G
   OK: /dev/sdb2 (/mnt/d1) Capacity: 63G Used: 3.8G (7%) Free: 56G
   OK: /dev/sdc2 (/mnt/d2) Capacity: 63G Used: 3.8G (7%) Free: 56G
   OK: /dev/sdd2 (/mnt/d3) Capacity: 63G Used: 3.8G (7%) Free: 56G
   OK: /dev/sde2 (/mnt/d4) Capacity: 63G Used: 3.8G (7%) Free: 56G
   OK: /dev/sdf2 (/mnt/d5) Capacity: 63G Used: 3.8G (7%) Free: 56G
   OK: /dev/sdg2 (/mnt/d6) Capacity: 63G Used: 3.8G (7%) Free: 56G
   OK: /dev/sdh2 (/mnt/d7) Capacity: 63G Used: 3.8G (7%) Free: 56G
   OK: /dev/mapper/vg01-lv01 (/mnt/vh) Capacity: 377G Used: 91G (24%) Free: 287G
   OK: /dev/sdk1 (/sdk1) Capacity: 244G Used: 131G (57%) Free: 101G
   OK: /dev/sdl1 (/sdl1) Capacity: 244G Used: 132G (57%) Free: 100G
   OK: /dev/sdm1 (/sdm1) Capacity: 244G Used: 117G (51%) Free: 115G
   OK: /dev/sdn1 (/sdn1) Capacity: 244G Used: 110G (48%) Free: 122G
   OK: /dev/sdo1 (/sdo1) Capacity: 244G Used: 90G (39%) Free: 142G
   OK: /dev/sdp1 (/sdp1) Capacity: 244G Used: 92G (40%) Free: 140G

<<< SNIP >>>

Node: 172.16.68.5:
   OK: /dev/md0 (/) Capacity: 112G Used: 42G (40%) Free: 65G
   OK: /dev/sda3 (/boot) Capacity: 93M Used: 32M (37%) Free: 56M
   OK: /dev/sda2 (/mnt/d0) Capacity: 63G Used: 4.9G (9%) Free: 55G
   OK: /dev/sdb2 (/mnt/d1) Capacity: 63G Used: 4.8G (9%) Free: 55G
   OK: /dev/sdc2 (/mnt/d2) Capacity: 63G Used: 4.8G (9%) Free: 55G
   OK: /dev/sdd2 (/mnt/d3) Capacity: 63G Used: 4.8G (8%) Free: 55G
   OK: /dev/sde2 (/mnt/d4) Capacity: 63G Used: 5.0G (9%) Free: 55G
   OK: /dev/sdf2 (/mnt/d5) Capacity: 63G Used: 4.9G (9%) Free: 55G
   OK: /dev/sdg2 (/mnt/d6) Capacity: 63G Used: 4.8G (8%) Free: 55G
   OK: /dev/sdh2 (/mnt/d7) Capacity: 63G Used: 4.9G (9%) Free: 55G
   WARNING: /dev/mapper/vg01-lv01 (/mnt/vh) Capacity: 377G Used: 282G (75%) Free: 96G
   OK: /dev/sdk1 (/sdk1) Capacity: 244G Used: 94G (41%) Free: 138G
   OK: /dev/sdl1 (/sdl1) Capacity: 244G Used: 93G (41%) Free: 139G
   OK: /dev/sdm1 (/sdm1) Capacity: 244G Used: 93G (41%) Free: 139G
   OK: /dev/sdn1 (/sdn1) Capacity: 244G Used: 94G (41%) Free: 138G
   OK: /dev/sdo1 (/sdo1) Capacity: 244G Used: 95G (41%) Free: 137G
   OK: /dev/sdp1 (/sdp1) Capacity: 244G Used: 93G (41%) Free: 139G

<<< SNIP >>>
```

#####Test: 2 - LOG file size

```
OK: LOG file is currently 815.1 MB (MQI)|log_file_size: log_file_size=854727439 PARAMETERS: TEST=2 DATABASE= II_SYSTEM=/opt/Actian/VectorX1 WARNING_VALUE=1073741824 CRITICAL_VALUE=2147483648

Location of LOG file: hdfs://padb-cluster:8020/Actian/VectorX1/ingres/data/vectorwise/xerox/CBM
```

#####Test: 6 - SMARTCTL (Dell & Cisco) disk status

```
Disks OK: 96  (MQI)|smartctl_disks: disk_ok=96 disk_warn=0 disk_crit=0 PARAMETERS: TEST=6

OK: 172.16.68.2 /dev/sda
OK: 172.16.68.2 /dev/sdb
OK: 172.16.68.2 /dev/sdd
OK: 172.16.68.2 /dev/sdf
OK: 172.16.68.2 /dev/sdc
OK: 172.16.68.2 /dev/sde
OK: 172.16.68.2 /dev/sdi
<<< SNIP >>>
```

#####Test: 7 - Network errors

```
Links OK: 20  (MQI)|link_status: nic_ok=20 nic_warn=0 nic_crit=0 PARAMETERS: TEST=7 WARNING_PERCENT=10 CRITICAL_PERCENT=20
OK: Node: 172.16.68.2 NIC: eth0 TX Packets: 81378768  TX Errors: 0 (0%) RX Packets: 925097508 RX Errors: 0 (0%) Collisions: 0
OK: Node: 172.16.68.2 NIC: eth2 TX Packets: 784044  TX Errors: 0 (0%) RX Packets: 773246 RX Errors: 0 (0%) Collisions: 0
OK: Node: 172.16.68.2 NIC: lo TX Packets: 939185556  TX Errors: 0 (0%) RX Packets: 939185556 RX Errors: 0 (0%) Collisions: 0
OK: Node: 172.16.68.3 NIC: eth0 TX Packets: 172719404  TX Errors: 0 (0%) RX Packets: 47349156 RX Errors: 0 (0%) Collisions: 0
OK: Node: 172.16.68.3 NIC: eth2 TX Packets: 755175  TX Errors: 0 (0%) RX Packets: 760321 RX Errors: 0 (0%) Collisions: 0
OK: Node: 172.16.68.3 NIC: lo TX Packets: 5901368  TX Errors: 0 (0%) RX Packets: 5901368 RX Errors: 0 (0%) Collisions: 0
OK: Node: 172.16.68.4 NIC: eth0 TX Packets: 119266578  TX Errors: 0 (0%) RX Packets: 41265655 RX Errors: 0 (0%) Collisions: 0
<<< SNIP >>>
```

#####Test: 8 - SWAP usage

```
SWAP OK: 6  (MQI)|swap_usage: swap_ok=6 swap_warn=0 swap_crit=0 PARAMETERS: TEST=8 WARNING_PERCENT=10 CRITICAL_PERCENT=20
OK: Node: 172.16.68.2 SWAP(GB) Total: 181 Used: 0 (0%) Free: 181
OK: Node: 172.16.68.3 SWAP(GB) Total: 181 Used: 0 (0%) Free: 181
OK: Node: 172.16.68.4 SWAP(GB) Total: 181 Used: 0 (0%) Free: 181
OK: Node: 172.16.68.5 SWAP(GB) Total: 181 Used: 0 (0%) Free: 181
OK: Node: 172.16.68.6 SWAP(GB) Total: 181 Used: 0 (0%) Free: 181
OK: Node: 172.16.68.7 SWAP(GB) Total: 3 Used: 0 (0%) Free: 3
```

#####Test: 9 - hard ulimit file descriptors

```
Hard File Descriptor ULIMIT OK: 5 Hard File Descriptor ULIMIT Critical: 1  (MQI)|hard_fd_ulimit: fd_ulimit_ok=5 fd_ulimit_warn=0 fd_ulimit_crit=1 PARAMETERS: TEST=9 WARNING_VALUE=10000 CRITICAL_VALUE=10000
OK: Node: 172.16.68.2 Hard File Descriptor Limit: 65000 is fine (required value of 10000)
OK: Node: 172.16.68.3 Hard File Descriptor Limit: 65000 is fine (required value of 10000)
OK: Node: 172.16.68.4 Hard File Descriptor Limit: 65000 is fine (required value of 10000)
OK: Node: 172.16.68.5 Hard File Descriptor Limit: 65000 is fine (required value of 10000)
OK: Node: 172.16.68.6 Hard File Descriptor Limit: 65000 is fine (required value of 10000)
CRITICAL: Node: 172.16.68.7 Hard File Descriptor Limit: 4096 is less than the required value of 10000
```

#####Test: 10 - soft ulimit file descriptors

```
Soft File Descriptor ULIMIT OK: 4 Soft File Descriptor ULIMIT Critical: 2  (MQI)|hard_fd_ulimit: fd_ulimit_ok=4 fd_ulimit_warn=0 fd_ulimit_crit=2 PARAMETERS: TEST=10 WARNING_VALUE=10000 CRITICAL_VALUE=10000
OK: Node: 172.16.68.2 Soft File Descriptor Limit: 65000 is fine (required value of 10000)
OK: Node: 172.16.68.3 Soft File Descriptor Limit: 65000 is fine (required value of 10000)
OK: Node: 172.16.68.4 Soft File Descriptor Limit: 65000 is fine (required value of 10000)
CRITICAL: Node: 172.16.68.5 Soft File Descriptor Limit: 8192 is less than the required value of 10000
OK: Node: 172.16.68.6 Soft File Descriptor Limit: 65000 is fine (required value of 10000)
CRITICAL: Node: 172.16.68.7 Soft File Descriptor Limit: 1024 is less than the required value of 10000
```

#####Test: 11 - max files limits

```
Max Files Limit OK: 6  (MQI)|max_files_limit: max_files_limit_ok=6 max_files_limit_warn=0 max_files_limit_crit=0 PARAMETERS: TEST=11 WARNING_VALUE=10000 CRITICAL_VALUE=10000
OK: Node: 172.16.68.2 Max Files Limit: 19709147 is fine (required value of 10000)
OK: Node: 172.16.68.3 Max Files Limit: 19709147 is fine (required value of 10000)
OK: Node: 172.16.68.4 Max Files Limit: 19709146 is fine (required value of 10000)
OK: Node: 172.16.68.5 Max Files Limit: 19709147 is fine (required value of 10000)
OK: Node: 172.16.68.6 Max Files Limit: 19709146 is fine (required value of 10000)
OK: Node: 172.16.68.7 Max Files Limit: 19709105 is fine (required value of 10000)
```

#####Test: 12 - per process inuse file descriptors

```
File Descriptor usage OK: 39 File Descriptor usage Warning: 1 File Descriptor usage Critical: 1  (MQI)|process_fd_usage: fd_ok=39 fd_warn=1 fd_crit=1 PARAMETERS: TEST=12 WARNING_PERCENT=20 CRITICAL_PERCENT=30

Node: 172.16.68.2:
   OK: PID 33361 currently has 6 file descriptors 0% (soft limit is 65000 hard limit is 65000)
   OK: PID 44755 currently has 10 file descriptors 0% (soft limit is 65000 hard limit is 65000)
   OK: PID 44773 currently has 1714 file descriptors 2% (soft limit is 65000 hard limit is 65000)

<<< SNIP >>>

Node: 172.16.68.5:
   OK: PID 13102 currently has 6 file descriptors 0% (soft limit is 8192 hard limit is 65000)
   OK: PID 13116 currently has 5 file descriptors 0% (soft limit is 8192 hard limit is 65000)
   OK: PID 17741 currently has 5 file descriptors 0% (soft limit is 8192 hard limit is 65000)
   OK: PID 44921 currently has 29 file descriptors 0% (soft limit is 8192 hard limit is 65000)
   OK: PID 44922 currently has 15 file descriptors 0% (soft limit is 8192 hard limit is 65000)
   OK: PID 44923 currently has 8 file descriptors 0% (soft limit is 8192 hard limit is 65000)
   OK: PID 44924 currently has 8 file descriptors 0% (soft limit is 8192 hard limit is 65000)
   OK: PID 44925 currently has 8 file descriptors 0% (soft limit is 8192 hard limit is 65000)
   OK: PID 44926 currently has 8 file descriptors 0% (soft limit is 8192 hard limit is 65000)
   OK: PID 44927 currently has 8 file descriptors 0% (soft limit is 8192 hard limit is 65000)
   WARNING: PID 44931 currently has 1732 file descriptors 21% (soft limit is 8192 hard limit is 65000)
   OK: PID 47808 currently has 5 file descriptors 0% (soft limit is 8192 hard limit is 65000)

<<< SNIP >>>
```

#####Test: 13 - total inuse file descriptors

```
Total File Descriptor usage OK: 6  (MQI)|total_fd_usage: fd_ok=6 fd_warn=0 fd_crit=0 PARAMETERS: TEST=13 WARNING_PERCENT=20 CRITICAL_PERCENT=30

Node: 172.16.68.2:
   OK: 9856 file descriptors are in use 0% (file max limit is 19709147)

Node: 172.16.68.3:
   OK: 4864 file descriptors are in use 0% (file max limit is 19709147)

Node: 172.16.68.4:
   OK: 4288 file descriptors are in use 0% (file max limit is 19709146)

<<< SNIP >>>
```
