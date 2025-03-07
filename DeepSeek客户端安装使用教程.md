# A10 deepseek安装

用户名：18010750271

密码：Xzh.201333

代理：ssh -f -N -D 0.0.0.0:1081 administrator@172.29.234.17

密码：Nlx135@$^

## 0 、更新apt源

```bash
apt update 
sed -i 's|http://172.29.12.35/ubuntu|https://mirrors.aliyun.com/ubuntu|g' /etc/apt/sources.list
apt-get install build-essential
```

## 1、cudatoolkit安装

#### 1、cudatoolkit安装准备

1.**强制禁用 Nouveau 驱动**

```BASH
# 编辑黑名单配置
nano /etc/modprobe.d/blacklist-nouveau.conf
# 添加以下内容：
blacklist nouveau
options nouveau modeset=0
```

2.**手动重建 initramfs**

```bash
# 先备份原initramfs
cp /boot/initrd.img-$(uname -r) /boot/initrd.img-$(uname -r).bak
# 强制重建
update-initramfs -u -k all
```

3.**彻底重启系统**

```BASH
sudo reboot
```

4.**验证 Nouveau 状态**

```BASH
# 重启后检查是否加载
lsmod | grep -i nouveau
# 无输出表示禁用成功
```

## 

#### 2、安装cudatoolkit

已包含nvidia驱动

[CUDA Toolkit 12.4 Downloads | NVIDIA Developer](https://developer.nvidia.com/cuda-12-4-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=runfile_local)

```bash
wget https://developer.download.nvidia.com/compute/cuda/12.4.0/local_installers/cuda_12.4.0_550.54.14_linux.run
sudo sh cuda_12.4.0_550.54.14_linux.run
```



![image-20250307135028685](C:\Users\13556\AppData\Roaming\Typora\typora-user-images\image-20250307135028685.png)

## 2、miniconda配置

### 1、miniconda下载

[Download Now | Anaconda](https://www.anaconda.com/download/success)

https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

![image-20250307140353615](C:\Users\13556\AppData\Roaming\Typora\typora-user-images\image-20250307140353615.png)



### 2、conda环境配置

**安装miniconda**

```bash
bash Miniconda3-latest-Linux-x86_64.sh
```

**手动添加 Conda 到环境变量**

**Linux/macOS**

1. 打开终端，编辑 Shell 配置文件（如 `~/.bashrc`、`~/.zshrc` 或 `~/.bash_profile`）：

   ```bash
   nano ~/.bashrc  # 根据你的 Shell 选择配置文件
   ```

2. 在文件末尾添加以下内容（替换 `/root/miniconda3` 为你的实际路径）：

   ```bash
   export PATH="/root/miniconda3/bin:$PATH"
   ```

3. 保存文件（`Ctrl+O` → 回车 → `Ctrl+X`），然后更新配置：

   ```bash
   source ~/.bashrc  # 与步骤1中选择的配置文件一致
   ```

**初始化conda**

1. **运行初始化命令**：

   ```bash
   conda init
   ```

   然后根据你当前使用的Shell类型（如 `bash`、`zsh` 等），可能需要指定对应的初始化，例如：

   ```bash
   conda init bash
   ```

2.**重新加载Shell配置**：

   - 如果是bash

     ```bash
     source ~/.bashrc
     ```

3.**再尝试激活环境：**

```bash
conda create -n vllm
```

```bash
conda activate vllm
```



## 3、pytorch安装

[PyTorch](https://pytorch.org/)

**添加清华源的pytorch**

```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
```

```bash
pip3 install torch torchvision torchaudio -f https://mirrors.aliyun.com/pytorch-wheels/cu121
```

![image-20250307135410229](C:\Users\13556\AppData\Roaming\Typora\typora-user-images\image-20250307135410229.png)

## 4、vllm安装

```bash
pip install vllm
```



## 5、模型下载

```bash
pip install modelscope 
modelscope download --model tclf90/deepseek-r1-distill-qwen-32b-gptq-int4 --local_dir /data/deepseek-32b
```

