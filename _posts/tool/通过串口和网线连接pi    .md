

###通过串口和网线连接pi     

在没有键盘鼠标显示器，没有任何网络设备，甚至连电源和 micro USB 数据线都没有的情况下。可通过USB 转TTL对树莓派进行操作 ,__无需外接电源__。

![串口连接示意图](http://t1.aixinxi.net/o_1c2rur7okeovhdg1h4s7pg1sgma.jpg-w.jpg)   

波特率：115200

![](http://t1.aixinxi.net/o_1c2rvdoe011oenog1tet11ii59da.jpg-w.jpg) 



然后通过网线与笔记本和树莓派网口进行连接，注意设置笔记本WLAN共享。


###安装pacaur

网上多推荐使用 yaourt 工具，但是在Arch Arm是无法安装，而pacaur更强大. 

- 首先安装sudo git wget 
```
pacman -S sudo git wget 
```

- 获取[pacaur安装脚本](https://gist.github.com/rumpelsepp/d646750910be19332753)
```
$ wget https://gist.githubusercontent.com/rumpelsepp/d646750910be19332753/raw/bd1f3abd95694368a2933c6b94298abb3f9ce814/install-pacaur.sh -O install_pacaur.sh  
$ sh install_pacaur.sh
```



### 安装无线网卡       

![无线网卡型号](http://t1.aixinxi.net/o_1c2rvojpb7241i3hnd61rf6h8ja.jpg-w.jpg)

| Git Clone URL: | <https://aur.archlinux.org/8192cu-dkms.git> (read-only) |
| -------------- | ---------------------------------------- |
| Package Base:  | [8192cu-dkms](https://aur.archlinux.org/pkgbase/8192cu-dkms/) |

执行 

```
pacaur -S 8192cu-dkms
```



折这wifi自启动

参考[netctl（简体中文）](https://wiki.archlinux.org/index.php/Netctl_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 

执行

```
	sudo pacman -S wpa_actiond  
	
```

重启后执行

```
sudo wifi-menu
#记住配置文件为wifi-XX
systemctl enable netctl-auto@interface.service #interface一般为wlan0,可以使用ifcong确认
sudo netctl  enable (wifi-menu 生成的配置文件)
```

使用ifconfig 记住wlan0 ip地址，添加到xshell 等终端软件配置ssh连接。

### 重启

拔掉网线，USB 转ttl,重启，无线网卡闪烁说明wlan0 成功启动，可以直接使用SSH连接，大功告成。