### 驱动信息
驱动版本：550.100
支持设备：看https://www.nvidia.com/en-us/geforce/drivers/results/228548/
### 使用指南
1. 下载官方驱动https://cn.download.nvidia.com/XFree86/Linux-x86_64/550.100/NVIDIA-Linux-x86_64-550.100.run
2. 执行下面命令从驱动中解压nvidia-smi 依赖库和内核模块源码：
``` bash
chmod +x NVIDIA-Linux-x86_64-550.100.run
./NVIDIA-Linux-x86_64-550.100.run -x --tmpdir .
```
3. 创建目录归类相关程序，需要预留1G空间
``` bash
mkdir -p /volume2/@nvidia/bin
mkdir -p /volume2/@nvidia/lib
mkdir -p /volume2/@nvidia/module
```
4. 提取nvidia-smi需要的依赖库和可执行程序
``` bash
cd NVIDIA-Linux-x86_64-550.100/
cp -f libcudadebugger.so.550.100 /volume2/@nvidia/lib/
cp -f libcuda.so.550.100 /volume2/@nvidia/lib/
cp -f libEGL_nvidia.so.550.100 /volume2/@nvidia/lib/
cp -f libEGL.so.1.1.0 /volume2/@nvidia/lib/
cp -f libEGL.so.550.100 /volume2/@nvidia/lib/
cp -f libGLdispatch.so.0 /volume2/@nvidia/lib/
cp -f libGLESv1_CM_nvidia.so.550.100 /volume2/@nvidia/lib/
cp -f libGLESv1_CM.so.1.2.0 /volume2/@nvidia/lib/
cp -f libGLESv2_nvidia.so.550.100 /volume2/@nvidia/lib/
cp -f libGLESv2.so.2.1.0 /volume2/@nvidia/lib/
cp -f libGL.so.1.7.0 /volume2/@nvidia/lib/
cp -f libGLX_nvidia.so.550.100 /volume2/@nvidia/lib/
cp -f libglxserver_nvidia.so.550.100 /volume2/@nvidia/lib/
cp -f libGLX.so.0 /volume2/@nvidia/lib/
cp -f libnvcuvid.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-allocator.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-api.so.1 /volume2/@nvidia/lib/
cp -f libnvidia-cfg.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-eglcore.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-egl-gbm.so.1.1.1 /volume2/@nvidia/lib/
cp -f libnvidia-egl-wayland.so.1.1.13 /volume2/@nvidia/lib/
cp -f libnvidia-encode.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-fbc.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-glcore.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-glsi.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-glvkspirv.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-gpucomp.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-gtk2.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-gtk3.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-ml.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-ngx.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-nvvm.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-opencl.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-opticalflow.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-pkcs11-openssl3.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-pkcs11.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-ptxjitcompiler.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-rtcore.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-tls.so.550.100 /volume2/@nvidia/lib/
cp -f libnvidia-wayland-client.so.550.100 /volume2/@nvidia/lib/
cp -f libnvoptix.so.550.100 /volume2/@nvidia/lib/
cp -f libOpenCL.so.1.0.0 /volume2/@nvidia/lib/
cp -f libOpenGL.so.0 /volume2/@nvidia/lib/
cp -f libvdpau_nvidia.so.550.100 /volume2/@nvidia/lib/
cp -f nvidia_drv.so /volume2/@nvidia/lib/

cp -f nvidia-cuda-mps-control /volume2/@nvidia/bin/
cp -f nvidia-cuda-mps-server /volume2/@nvidia/bin/
cp -f nvidia-debugdump /volume2/@nvidia/bin/
cp -f nvidia-installer /volume2/@nvidia/bin/
cp -f nvidia-modprobe /volume2/@nvidia/bin/
cp -f nvidia-ngx-updater /volume2/@nvidia/bin/
cp -f nvidia-persistenced /volume2/@nvidia/bin/
cp -f nvidia-powerd /volume2/@nvidia/bin/
cp -f nvidia-settings /volume2/@nvidia/bin/
cp -f nvidia-smi /volume2/@nvidia/bin/
cp -f nvidia-xconfig /volume2/@nvidia/bin/
```
5. 加载内核模块
``` bash
sudo mv nvidia.ko /volume2/@nvidia/module/
sudo insmod /volume2/@nvidia/module/nvidia.ko
```
6. 下载ldconfig和ld.so.conf，并进行相关配置
``` bash
sudo chmod +x ldconfig
sudo mv ldconfig /sbin/ldconfig
sudo mv ld.so.conf /etc/ld.so.conf
sudo ldconfig
```
7. 建立软连接，便于执行命令
``` bash
sudo ln -s /volume2/@nvidia/bin/nvidia-smi /usr/bin
```
8. 运行nvidia-smi命令
``` bash
nvidia-smi
```
9. 开机自动加载驱动。在群晖中创建计划任务，触发动作选"开机"，执行权限root，脚本内容如下：
``` bash
sleep 5
/sbin/insmod /volume2/@nvidia/module/nvidia.ko
sleep 5
```

### 编译指南
参看：https://www.secfa.cn/2024/07/%e7%be%a4%e6%99%96%e9%a9%b1%e5%8a%a8%e7%bc%96%e8%af%91%e7%af%87%ef%bc%88%e5%9b%9b%ef%bc%89sa6400-nvidia-display-driver-nvidia-smi/

### Mini Runtime
该目录存放nvidia-smi 以及`运行该程序查看显卡信息时`需要的最小依赖库
使用方法：
``` bash
sudo mv nvidia.ko /usr/local/
sudo insmod nvidia.ko
sudo mv libnvidia-ml.so.550.100 /usr/lib/
sudo chmod +x nvidia-smi
sudo mv nvidia-smi /usr/bin/
sudo chmod +x ldconfig
sudo mv ldconfig /sbin/ldconfig
sudo echo "/usr/local/lib" > /etc/ld.so.conf
sudo echo "/usr/lib" > /etc/ld.so.conf
sudo echo "/usr/lib32" > /etc/ld.so.conf
sudo ldconfig
```