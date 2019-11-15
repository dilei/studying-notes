## dlv（GO语言调试利器 [github库](https://github.com/go-delve/delve)）

1. 安装
    ```
    go get -u github.com/go-delve/delve/cmd/dlv
    ```
1. 两种启动方式
    ```
    // 第一种
    dlv main.go 
    // 如果有参数，使用“--”和dlv的命令参数分割
    dlv debug cmd/main.go -- -config /root/config.yaml

    // 第二种
    // 启动服务，找到服务的进程id，attach到程序
    dlv attach 进程ID
    ```
3. 命令参数
    ```
    [root@ProdApiST5678 ~]# ps axu | grep main
    root       359  0.0  0.0 114896   952 pts/2    S+   19:46   0:00 grep --color=auto main
    root     29825  0.0  0.2 123868 14932 pts/0    Sl+  19:09   0:00 ./main -config=/root/gopath/src/prod-api/config/config_online.yaml
    [root@ProdApiST5678 ~]# dlv attach 29825
    Type 'help' for list of commands.
    (dlv) b /root/main.go:20  // main文件20行加断点
    (dlv) bp // 查看断点列表
    (dlv) clear 2 // 删除序号为2（上一个命令能看到断点序号）的断点
    (dlv) clearall // 删除所有断点
    (dlv) c // 向下执行到断点
    (dlv) l // 查看断点处的代码
    (dlv) p num // 打印num的值
    (dlv) n // 向下执行，遇到方法不进入
    (dlv) goroutines // 产看goroutines信息
    (dlv) goroutine 6 // 进入goroutine
    (dlv) bt // 当前线程调用栈信息
    (dlv) bt 7 // 调用栈深度
    (dlv) quit // 退出
    ```
