## 1.查看环境的cuda版本
- 查看系统的cuda版本：nvcc -V
- 查看当前环境的：conda list | grep cuda
## 2.cuda应用的整体架构
![](https://cdn.nlark.com/yuque/0/2024/jpeg/34805283/1715057614467-52af437e-5744-4d32-901e-6ea3435c1c16.jpeg)
## 3.一条龙流程
### 3.1查看os和gcc版本以及显卡信息

- `cat /etc/*release`
- `gcc --version`
- `lspci | grep -i nvidia-smi`
### 3.2查看cuda版本

- `nvidia-smi`
- 显示cuda version: 11.6
   - 这个不是`cuda toolkit`的版本，而是`cuda user-mode driver`版本
#### 3.2.1补充解释说明

- **CUDA有两个基本的API**
   - `runtime`和`driver API`他们有一个对应的关系
   - `driver API`是由GPU的驱动安装程序安装的（`libcuda.so`）
   - `runtime API`是通过CUDA toolkit安装程序安装的（当然，有时候也是和GPU驱动捆绑安装的，`libcudart.so`）
- `NVIDIA-SMI`
   - 这个工具是由GPU驱动程序安装的，并不是toolkit安装的。
   - 这与`CUDA runtime`的版本无关
   - 要弄清楚`nvidia-smi`的输出版本与安装的`cuda runtime`的版本并没有关系！！！
- `nvcc`
   - 这个CUDA程序的编译工具是由CUDA toolkit安装，它标识的版本是CUDA运行时的版本。
   - 它并不知道GPU驱动的版本信息，即使你没有安装GPU驱动它也不知道。
- `nvcc -V`显示的版本不是预期的版本
   - 重新按安装步骤来
> [https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#post-installation-actions](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#post-installation-actions)

- 向下兼容
   - 一般情况下，`nvidia-smi`报告的cuda版本号在数值上高于或者等于`nvcc -V`的版本号
   - 这是是定义的兼容路径（`newer drivers/driver API support "older" CUDA toolkits/runtime API`）
- `nvcc -V`没有输出任何提示或者（`Command not found`）
   - 可能由于CUDA没有被正确地安装
      - 使用`find`或者`locate`找到`nvcc`的可执行路径，假设只有一个路径，那么可以使用该路径来修复 PATH 环境变量
   - 同样有可能压根就没安装，这种情况可能是`nvcc`是由CUDA toolki安装的，而不是GPU驱动程序单独安装的
- 在`docker`中
   - `nvidia-smi`标志系统的驱动版本
   - `nvcc --version`是docker容器内的版本信息
- `Ananconda`中
   - 你也会发现版本不匹配的问题
   - 由Ananconda安装的旧版本的CUDA toolkit可以使用最新的`nvidia-smi`的版本
- `nvidia-smi`的显示版本低于`nvcc`的显示版本
   - 可能有问题了
   - 向下兼容不符合
   - 建议更新GPU的驱动版本
   - 具体的报错的问题
      - 比如现在我们的`nvidia-smi`显示的版本信息是10.1，而`nvcc`的版本信息是10.2，这个时候cuda尝试编译使用10.2nvcc在10.1的驱动上编译，会报错""RuntimeError: CUDA error: no kernel image is available for execution on the device"

## 3.3 cuda
### 3.3.1内核的一些模块需要添加进去
```
nvidia.ko
nvidia-modeset.ko
nvidia-uvm.ko
nvidia-drm.ko
nvidia-peermem.ko
```
### 3.3.2关于版本号的解释
```python
nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2022 NVIDIA Corporation
Built on Tue_Mar__8_18:18:20_PST_2022
Cuda compilation tools, release 11.6, V11.6.124
Build cuda_11.6.r11.6/compiler.31057947_0
```

- nvidia-smi显示的510.108.03 是最下面GPU kernal-mode driver (nvidia.so)的版本
- nvidia-smi显示的11.6 是中间`CUDA user-mode driver (libcuda.so)`的版本
   - 而且版本号是11/12这种数字, 但是却不是和cuda toolkit强关联的. 
   - 属于driver层, 是随着driver一起安装的.
- nvcc显示的11.6 是最上面cuda toolkit的版本
## 4.版本兼容问题

- 分层，层与层之间就会有兼容的问题
   - 卡和driver
   - nvidia-driver和cuda-toolkit
   - cuda和pytorch
- 显卡驱动兼容性问题
   - [https://www.nvidia.com/download/index.aspx?lang=en-us](https://www.nvidia.com/download/index.aspx?lang=en-us)
- driver提供三种兼容能力, 下面细说
   - Backward Compatibility
   - Minor Version Compatibility
   - Forward Compatibility
   - ![image.png](https://cdn.nlark.com/yuque/0/2024/png/34805283/1715058254767-da846c15-27d6-4431-b7aa-07659b621ecd.png#averageHue=%23f7f7f5&clientId=uc517047e-a8d4-4&from=paste&height=169&id=u2515e1a0&originHeight=211&originWidth=607&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=57832&status=done&style=none&taskId=u09572ed6-e29f-4ac7-af85-7f1cb326b97&title=&width=485.6)
- driver的此要兼容
   - driver可以向前兼容未来的版本
   - ![image.png](https://cdn.nlark.com/yuque/0/2024/png/34805283/1715058324029-e1f52211-b862-49b1-ad33-3aced393a2ad.png#averageHue=%23f9f9f9&clientId=uc517047e-a8d4-4&from=paste&height=162&id=u913f926c&originHeight=202&originWidth=720&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=33862&status=done&style=none&taskId=u219d11eb-9078-4a7c-99f2-52439cdd4b7&title=&width=576)
- 两种driver
   - 之前文档指出, 这里涉及到3个东西的兼容性, 他们从底到上分别是
      - GPU kernal-mode driver
      - CUDA user-mode driver
      - CUDA runtime
   - 中间的驱动是随着kernel-model绑定的驱动
      - 比如CUDA user-mode driver是12.0版本(kernal driver525对应的), 那么在他上面运行
         - 11.x版本的cuda runtime/cuda toolkit, 都没问题, 向后兼容.
         - 12.3/12.x版本的cuda runtime/cuda toolkit
