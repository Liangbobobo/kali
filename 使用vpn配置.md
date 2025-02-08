# 1、查看手机ip
ip route | grep default

# 2、设置代理
export http_proxy="http://手机IP:10809"
export https_proxy="http://手机IP:10809"

# 3、查看当前IP地址，应该显示代理服务器的IP
curl ipinfo.io
curl -v https://www.google.com

# 添加iptables防火墙规则：
# 允许来自手机IP的连接
sudo iptables -A INPUT -p tcp -s 192.168.140.249 --dport 10808 -j ACCEPT
sudo iptables -A INPUT -p tcp -s 192.168.140.249 --dport 10809 -j ACCEPT


# 如果使用Firefox，可以直接在浏览器设置中配置：
打开 Settings/Preferences  
搜索 "proxy"  
点击 "Settings"  
选择 "Manual proxy configuration"  
填入代理信息：

     HTTP Proxy: 192.168.140.249
     Port: 10809
     选中 "Also use this proxy for HTTPS"


# 通过改变bashrc的方式

1、查看手机ip  
ip route | grep default

2、修改bash配置文件  
nano ~/.bashrc  
添加  
# 代理设置  
export http_proxy="http://192.168.43.1:10809"
export HTTP_PROXY="http://192.168.43.1:10809"
export https_proxy="http://192.168.43.1:10809"
export HTTPS_PROXY="http://192.168.43.1:10810"
export ALL_PROXY="socks5://192.168.43.1:10809"

# 便捷开关代理的别名命令  
alias proxy_on="export http_proxy='http://192.168.43.1:10809' && export https_proxy='http://192.168.43.1:10809' && export ALL_PROXY='socks5://192.168.43.1:10809'"
alias proxy_off="unset http_proxy && unset HTTP_PROXY && unset https_proxy && unset HTTPS_PROXY && unset ALL_PROXY"

3、使新的配置生效，执行：  
source ~/.bashrc

4、测试代理是否生效：  
curl -I https://www.google.com


5、快捷打开 关闭 代理命令  
proxy_on 命令开启代理
proxy_off 命令关闭代理