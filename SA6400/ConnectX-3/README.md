### 驱动信息
驱动版本：4.9-7.1.0.0
支持设备：ConnectX-3、ConnectX-3 Pro
### 使用指南
1. 修改ARPL RR版引导，去除模块mlx4_core mlx4_en mlx4_ib
2. 删除/lib/modules/下的mlx4_core.ko、mlx4_en.ko、mlx4_ib.ko
``` bash
sudo rm -rf /lib/modules/mlx4_core.ko
sudo rm -rf /lib/modules/mlx4_en.ko
sudo rm -rf /lib/modules/mlx4_ib.ko
```
3. 将编译好的驱动mlx_compat.ko、mlx4_core.ko、mlx4_en.ko 复制到/lib/modules/
4. 在群晖中创建计划任务，触发动作选"开机"，执行权限root，脚本内容如下：
``` bash
sleep 10
/sbin/insmod /lib/modules/mlx_compat.ko
/sbin/insmod /lib/modules/mlx4_core.ko
sleep 5
/sbin/ifconfig eth2 up
/sbin/ifconfig eth3 up
```
其中eth2 和eth3 根据你的已有网卡和新增网卡来设置，比如我原先已经有两个网卡（eth0、eth1），我现在想要使用两个40G光口的CX314A,那就是需要额外启动eth2和eth3
同时这里加载mlx4_core.ko会同时自动加载mlx4_en.ko
### 编译指南
参看：https://www.secfa.cn/2024/07/%e7%be%a4%e6%99%96%e9%a9%b1%e5%8a%a8%e7%bc%96%e8%af%91%e7%af%87%ef%bc%88%e5%9b%9b%ef%bc%89sa6400-cx314a-connectx-3-pro/