# 解决端口占用

1. `netstat -ano|findstr 8301` 端口号查询PID
2. `taskkill /f /t /im 9476` 终止



![image-20201012144329261](F:\GitFile\Typora\杂记\端口占用windows.assets\image-20201012144329261.png)

```shell
Microsoft Windows [版本 10.0.19041.546]
(c) 2020 Microsoft Corporation. 保留所有权利。

C:\Users\Administrator>netstat -ano|findstr 8301
  TCP    0.0.0.0:8301           0.0.0.0:0              LISTENING       9476
  TCP    [::]:8301              [::]:0                 LISTENING       9476

C:\Users\Administrator>
C:\Users\Administrator>taskkill /f /t /im 9476
成功: 已终止 PID 2828 (属于 PID 9476 子进程)的进程。
成功: 已终止 PID 9476 (属于 PID 9888 子进程)的进程。
```

