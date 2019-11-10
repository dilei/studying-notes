# pprof
go程序性能分析工具，主要有三种采样数据的方式:
- runtime/pprof: 使用命令行程序的数据采集
- net/http/pprof: 通过http程序的数据采集
- go test: 通常是使用前两种导出采集数据的文件，进行进一步的分析

本文使用第二种进行数据采集，net/http/pprof

## 开始
1. 需要你引入pprof
    ```go
      // 只需引用，它已经在init()函数中完成Handler的注册
      import _ "net/http/pprof"
    ```
1. 开始监听你的服务（首先你要启动你的服务）
    ```
    // 监听10s
    go tool pprof http://10.5.XX.XX:8088/debug/pprof/profile?seconds=10
    ```
    注意：seconds的值必须小于server的WriteTimeOut,否则直接报错
    > profile duration exceeds server's WriteTimeout

1. example:
    ```
    [root@ProdApiST ~]# go tool pprof http://10.5.XX.XX:8088/debug/pprof/profile?seconds=10
      Fetching profile over HTTP from http://10.5.XX.XX:8088/debug/pprof/profile?seconds=10
      Saved profile in /root/pprof/pprof.main.samples.cpu.002.pb.gz
      File: main
      Type: cpu
      Time: Nov 8, 2019 at 7:16pm (CST)
      Duration: 10.18s, Total samples = 7.60s (74.64%)
      Entering interactive mode (type "help" for commands, "o" for options)
      (pprof) top
      Showing nodes accounting for 3800ms, 50.00% of 7600ms total
      Dropped 329 nodes (cum <= 38ms)
      Showing top 10 nodes out of 135
            flat  flat%   sum%        cum   cum%
           630ms  8.29%  8.29%     1160ms 15.26%  reflect.Value.Index
           620ms  8.16% 16.45%     1630ms 21.45%  runtime.mallocgc
           410ms  5.39% 21.84%      500ms  6.58%  runtime.efaceeq
           380ms  5.00% 26.84%     2420ms 31.84%  reflect.packEface
           380ms  5.00% 31.84%     2830ms 37.24%  reflect.valueInterface
           380ms  5.00% 36.84%      630ms  8.29%  runtime.scanobject
           350ms  4.61% 41.45%     5080ms 66.84%  prod-api/libs.Contains
           240ms  3.16% 44.61%      350ms  4.61%  reflect.arrayAt
           210ms  2.76% 47.37%      220ms  2.89%  runtime.nextFreeFast
           200ms  2.63% 50.00%      200ms  2.63%  runtime.round
      (pprof) web > web.svg
    ```
1. 需要的工具
    ```sh
    // 生成svg的工具dot（centos为例安装）
    yum install graphviz
    ```
