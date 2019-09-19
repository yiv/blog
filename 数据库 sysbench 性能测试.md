# 数据库 sysbench 性能测试

## MySQL

#### 准备 1 万行数据

```bash
sysbench ./oltp_read_only.lua --db-driver=mysql --mysql-host=10.72.17.30 --mysql-port=13306 --mysql-user=root --mysql-password=123@mysql prepare
```



#### 准备 1000 万行数据

```bash
sysbench ./oltp_read_only.lua --db-driver=mysql --mysql-host=10.72.17.30 --mysql-port=13306 --mysql-user=root --mysql-password=123@mysql --table-size=100000000 prepare
```






#### 测试 - 读

```bash
sysbench ./oltp_read_only.lua --num-threads=16 --db-driver=mysql --mysql-host=10.72.17.30 --mysql-port=13306 --mysql-user=root --mysql-password=123@mysql run 

sysbench 1.0.17 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 16
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            174944
        write:                           0
        other:                           24992
        total:                           199936
    transactions:                        12496  (1247.85 per sec.)
    queries:                             199936 (19965.64 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.0126s
    total number of events:              12496

Latency (ms):
         min:                                    4.84
         avg:                                   12.81
         max:                                   52.98
         95th percentile:                       22.69
         sum:                               160064.82

Threads fairness:
    events (avg/stddev):           781.0000/34.47
    execution time (avg/stddev):   10.0041/0.00

```

#### 测试 - 写

```bash
sysbench ./oltp_write_only.lua --num-threads=16 --db-driver=mysql --mysql-host=10.72.17.30 --mysql-port=13306 --mysql-user=root --mysql-password=123@mysql run 

sysbench 1.0.17 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 16
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            0
        write:                           92156
        other:                           46088
        total:                           138244
    transactions:                        23026  (2300.82 per sec.)
    queries:                             138244 (13813.69 per sec.)
    ignored errors:                      36     (3.60 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.0062s
    total number of events:              23026

Latency (ms):
         min:                                    2.06
         avg:                                    6.95
         max:                                   65.13
         95th percentile:                       11.87
         sum:                               159981.47

Threads fairness:
    events (avg/stddev):           1439.1250/22.65
    execution time (avg/stddev):   9.9988/0.00

```

#### 测试 - 读写

```bash
sysbench ./oltp_read_write.lua --num-threads=16 --db-driver=mysql --mysql-host=10.72.17.30 --mysql-port=13306 --mysql-user=root --mysql-password=123@mysql run 

sysbench 1.0.17 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 16
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            115570
        write:                           33011
        other:                           16507
        total:                           165088
    transactions:                        8252   (823.78 per sec.)
    queries:                             165088 (16480.29 per sec.)
    ignored errors:                      3      (0.30 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.0158s
    total number of events:              8252

Latency (ms):
         min:                                    6.83
         avg:                                   19.40
         max:                                  110.61
         95th percentile:                       31.94
         sum:                               160083.48

Threads fairness:
    events (avg/stddev):           515.7500/10.37
    execution time (avg/stddev):   10.0052/0.00

```
#### 测试 - 插入

```bash
sysbench ./oltp_insert.lua --num-threads=16 --db-driver=mysql --mysql-host=10.72.17.30 --mysql-port=13306 --mysql-user=root --mysql-password=123@mysql run 

sysbench 1.0.17 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 16
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            0
        write:                           33625
        other:                           0
        total:                           33625
    transactions:                        33625  (3360.83 per sec.)
    queries:                             33625  (3360.83 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.0037s
    total number of events:              33625

Latency (ms):
         min:                                    1.05
         avg:                                    4.76
         max:                                   30.27
         95th percentile:                        7.84
         sum:                               159895.97

Threads fairness:
    events (avg/stddev):           2101.5625/4.40
    execution time (avg/stddev):   9.9935/0.00

```


## CockroachDB（ 3 节点，未负载均衡）

#### 准备 1 万行数据
```bash
sysbench ./oltp_read_only.lua --db-driver=pgsql --pgsql-host=10.72.17.30 --pgsql-port=26257 --pgsql-user=root prepare
```

#### 测试 - 读

```bash

sysbench ./oltp_read_only.lua --num-threads=16 --db-driver=pgsql --pgsql-host=10.72.17.30 --pgsql-port=26257 --pgsql-user=root run


sysbench 1.0.17 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 16
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            116746
        write:                           0
        other:                           16678
        total:                           133424
    transactions:                        8339   (832.75 per sec.)
    queries:                             133424 (13324.01 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.0124s
    total number of events:              8339

Latency (ms):
         min:                                   10.27
         avg:                                   19.20
         max:                                   75.52
         95th percentile:                       27.17
         sum:                               160095.74

Threads fairness:
    events (avg/stddev):           521.1875/7.21
    execution time (avg/stddev):   10.0060/0.00
```

#### 测试 - 写

```bash
sysbench ./oltp_write_only.lua --num-threads=16 --db-driver=pgsql --pgsql-host=10.72.17.30 --pgsql-port=26257 --pgsql-user=root run 

sysbench 1.0.17 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 16
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            0
        write:                           10431
        other:                           8342
        total:                           18773
    transactions:                        2728   (265.06 per sec.)
    queries:                             18773  (1824.07 per sec.)
    ignored errors:                      486    (47.22 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.2887s
    total number of events:              2728

Latency (ms):
         min:                                    9.78
         avg:                                   59.50
         max:                                  722.03
         95th percentile:                      227.40
         sum:                               162319.57

Threads fairness:
    events (avg/stddev):           170.5000/16.57
    execution time (avg/stddev):   10.1450/0.08

```

#### 测试 - 读写

```bash
sysbench ./oltp_read_write.lua --num-threads=16 --db-driver=pgsql --pgsql-host=10.72.17.30 --pgsql-port=26257 --pgsql-user=root run 

sysbench 1.0.17 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 16
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            15274
        write:                           3732
        other:                           1823
        total:                           20829
    transactions:                        135    (12.38 per sec.)
    queries:                             20829  (1910.40 per sec.)
    ignored errors:                      956    (87.68 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.9010s
    total number of events:              135

Latency (ms):
         min:                                   26.67
         avg:                                 1246.13
         max:                                 7072.39
         95th percentile:                     3208.88
         sum:                               168227.64

Threads fairness:
    events (avg/stddev):           8.4375/2.74
    execution time (avg/stddev):   10.5142/0.28

```
#### 测试 - 插入

```bash
sysbench ./oltp_insert.lua --num-threads=16 --db-driver=pgsql --pgsql-host=10.72.17.30 --pgsql-port=26257 --pgsql-user=root run

sysbench 1.0.17 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 16
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            0
        write:                           21647
        other:                           0
        total:                           21647
    transactions:                        21647  (2163.40 per sec.)
    queries:                             21647  (2163.40 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.0047s
    total number of events:              21647

Latency (ms):
         min:                                    3.56
         avg:                                    7.39
         max:                                   28.54
         95th percentile:                       11.04
         sum:                               159940.97

Threads fairness:
    events (avg/stddev):           1352.9375/2.97
    execution time (avg/stddev):   9.9963/0.00

```



## CockroachDB（ 3 节点，负载均衡）

#### 准备 1 万行数据
```bash
sysbench ./oltp_read_only.lua --db-driver=pgsql --pgsql-host=10.72.17.30 --pgsql-port=26256 --pgsql-user=root prepare
```
#### 准备 1000 万行数据
```bash
sysbench ./oltp_read_only.lua --db-driver=pgsql --pgsql-host=10.72.17.30 --pgsql-port=26256 --pgsql-user=root --table-size=100000000 prepare
```
#### 测试 - 读

```bash

sysbench ./oltp_read_only.lua --num-threads=16 --db-driver=pgsql --pgsql-host=10.72.17.30 --pgsql-port=26256 --pgsql-user=root run

sysbench 1.0.17 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 16
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            67732
        write:                           0
        other:                           9676
        total:                           77408
    transactions:                        4838   (482.57 per sec.)
    queries:                             77408  (7721.15 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.0239s
    total number of events:              4838

Latency (ms):
         min:                                   10.85
         avg:                                   33.11
         max:                                   88.13
         95th percentile:                       55.82
         sum:                               160192.37

Threads fairness:
    events (avg/stddev):           302.3750/113.64
    execution time (avg/stddev):   10.0120/0.01


```

#### 测试 - 写

```bash
sysbench ./oltp_write_only.lua --num-threads=16 --db-driver=pgsql --pgsql-host=10.72.17.30 --pgsql-port=26256 --pgsql-user=root run 

sysbench 1.0.17 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 16
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            0
        write:                           6980
        other:                           5764
        total:                           12744
    transactions:                        1858   (183.97 per sec.)
    queries:                             12744  (1261.87 per sec.)
    ignored errors:                      324    (32.08 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.0977s
    total number of events:              1858

Latency (ms):
         min:                                   15.68
         avg:                                   86.48
         max:                                  898.92
         95th percentile:                      303.33
         sum:                               160683.95

Threads fairness:
    events (avg/stddev):           116.1250/15.02
    execution time (avg/stddev):   10.0427/0.03


```

#### 测试 - 读写

```bash
sysbench ./oltp_read_write.lua --num-threads=16 --db-driver=pgsql --pgsql-host=10.72.17.30 --pgsql-port=26256 --pgsql-user=root run 

sysbench 1.0.17 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 16
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            12339
        write:                           2912
        other:                           1513
        total:                           16764
    transactions:                        114    (10.10 per sec.)
    queries:                             16764  (1485.86 per sec.)
    ignored errors:                      773    (68.51 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          11.2799s
    total number of events:              114

Latency (ms):
         min:                                   54.36
         avg:                                 1501.53
         max:                                 6015.74
         95th percentile:                     3982.86
         sum:                               171174.90

Threads fairness:
    events (avg/stddev):           7.1250/2.47
    execution time (avg/stddev):   10.6984/0.39


```
#### 测试 - 插入

```bash
sysbench ./oltp_insert.lua --num-threads=16 --db-driver=pgsql --pgsql-host=10.72.17.30 --pgsql-port=26256 --pgsql-user=root run

sysbench 1.0.17 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 16
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            0
        write:                           16311
        other:                           0
        total:                           16311
    transactions:                        16311  (1629.87 per sec.)
    queries:                             16311  (1629.87 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.0060s
    total number of events:              16311

Latency (ms):
         min:                                    5.12
         avg:                                    9.81
         max:                                   33.03
         95th percentile:                       15.83
         sum:                               159931.66

Threads fairness:
    events (avg/stddev):           1019.4375/23.68
    execution time (avg/stddev):   9.9957/0.00


```