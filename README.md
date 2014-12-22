tcpcopy_guide
=============

handbook


v0.9.9

编译安装：
wget https://github.com/wangbin579/tcpcopy/archive/0.9.9.zip
unzip 0.9.9.zip
cd tcpcopy-0.9.9/
./autogen.sh
./configure
  或 ./configure --enable-advanced --enable-pcap   route模式与iptables模式不能共存
make && make install

辅助机（设IP 192.168.13.202）
intercept -i eth1 -F'tcp and src port 80' -d

测试、辅助机（设IP 192.168.13.201）
iptables -t filter -I OUTPUT -p tcp --sport 80 -j QUEUE
intercept &
  或 route add -host 192.168.11.5 gw 192.168.13.202
    导入到测试机的客户端使用辅助机器路由
tail -f /var/log/nginx/access.log

业务机（设IP 192.168.13.200）
tcpcopy -x 192.168.13.200:80-192.168.13.201:80 &
  或 tcpcopy -x 192.168.13.200:80-192.168.13.201:80 -s 192.168.13.202 -i eth1 -d
tail -f /var/log/nginx/access.log

清除iptables：
iptables -P INPUT ACCEPT
iptables -F


------------------------------------
注意公用数据库可能导致的数据重复问题。
