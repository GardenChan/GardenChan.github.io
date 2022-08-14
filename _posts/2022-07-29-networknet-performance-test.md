---
layout: post
title: "常用的网络测试用具"
subtitle: "轻松测试国内、国外、跨境网络性能，包括带宽、延迟等"
date: 2022-07-29
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [计算机网络,测试]
---

# 一、带宽压测 iperf
## Server-数据接收端

``` shell
# server端和client端 工具安装
sudo apt remove iperf3 libiperf0
sudo apt install libsctp1
wget https://iperf.fr/download/ubuntu/libiperf0_3.9-1_amd64.deb
wget https://iperf.fr/download/ubuntu/iperf3_3.9-1_amd64.deb
sudo dpkg -i libiperf0_3.9-1_amd64.deb iperf3_3.9-1_amd64.deb
rm libiperf0_3.9-1_amd64.deb iperf3_3.9-1_amd64.deb
# 数据接收端 server
iperf3 -s
```

## Client-数据发送端

``` shell
# server端和client端 工具安装
sudo apt remove iperf3 libiperf0
sudo apt install libsctp1
wget https://iperf.fr/download/ubuntu/libiperf0_3.9-1_amd64.deb
wget https://iperf.fr/download/ubuntu/iperf3_3.9-1_amd64.deb
sudo dpkg -i libiperf0_3.9-1_amd64.deb iperf3_3.9-1_amd64.deb
rm libiperf0_3.9-1_amd64.deb iperf3_3.9-1_amd64.deb

# 数据发送端 client
# -c 设置为客服端
# server-ip
# -t 测试时长
# -i 日志输出间隔时间
# -u UDP
# -b 目标带宽
iperf3 -c <server-ip> -t 1800 -i 1 -u -b 50m
```

# 二、HTTP压测 wrk

```shell 
git clone https://github.com/wg/wrk
cd wrk
make
# 命令介绍: wrk <选项> <被测HTTP服务的URL>
# Options:
# -c, --connections 跟服务器建立并保持的TCP连接数量 (请求并发数量)
# -d, --duration 压测时间
# -t, --threads 使用多少个线程进行压测 一般设置成cpu的核数的2或者4倍
# --timeout 或 -T：超时的时间
# --latency：显示延迟统计

# 举例
wrk -t8 -c200 -d30s --latency http://www.bing.com
wrk -t10 -c30 -d2s -T5s --latency http://www.baidu.com
```

# 三、上传和下载带宽测试
## Speedtest 测速

```shell 
# 使用Speedtest工具测试主机的上传和下载带宽
wget https://raw.github.com/sivel/speedtest-cli/master/speedtest.py
python speedtest.py --share
```

## Superspeed 测速

```shell 
# Superspeed工具针对国内三大运营商的节点进行测速
bash <(curl -Lso- https://git.io/superspeed)  
bash <(curl -Lso- https://git.io/superspeed_uxh) # 使用这个版本
```

# 四、综合性能测试

``` shell
wget -qO- bench.sh | bash
```

# 五、PING 延迟测速
## 全国各地-->目标机器
全国各地到目标机器的 PING 延迟测试，使用 http://ipip.net 的在线工具测试工具 https://tools.ipip.net/ping.php ，输入目标机器的 IP，测试全国各地区到目标机器的PING延迟。

## 目标机器-->国内三大运营商

``` shell
#目标机器到国内电信、联通、移动线路
# 使用 mping 脚本一键测试目标机器到国内电信、联通、移动线路的回程Ping
wget https://raw.githubusercontent.com/helloxz/mping/master/mping.sh
bash mping.sh
```

# 六、IP 路由

``` shell
安装 besttrace
wget https://cdn.ipip.net/17mon/besttrace4linux.zip
unzip besttrace4linux.zip
chmod +x besttrace

在目标云服务器上测试到目标 IP 的寻址路由
./besttrace <ip>
```

