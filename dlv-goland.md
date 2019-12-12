## goland连接dlv进行远程调试
有两种方式启动
### 第一种，直接让dlv编译你的程序
- 如果有启动参数，需要使用“--”放在后面(比如我的参数 -c /root/gopath/src/prod-api/configs/config_online.yaml -l info）
```
dlv debug --headless --listen=:2345 --api-version=2 --accept-multiclient -- -c /root/gopath/src/prod-api/configs/config_online.yaml -l info
```
- 配置goland远程调试配置
```
在 GoLand 中点击菜单栏 Run -> EditConfigurations -> + -> GoRemote, 填上远程的ip和端口
```
- 在goland中设置断点
- 访问你的网页，即可进入调试
### 第二种，让dlv运行你的可执行程序
- 运行
```
dlv --listen=:2345 --headless=true --api-version=2 --accept-multiclient exec ./main -- -c /root/gopath/src/prod-api/configs/config_online.yaml -l info
```
- 其余步骤同上
