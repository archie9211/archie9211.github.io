---
layout: post
author: Nageen
categories: [ how to ] 
featured: false
image: assets/images/tf-cuda-cudnn.png
---
Getting GPU working for tensorflow and CUDA is a difficult task for naive linux users. In this guide, you can easily understand the installation process. This guide is focussed on debian based distros, but it can be used on any linux distro with respective command for the task according to distro.

So, What do we need for working tensorflow and nvidia drivers on linux

> * A linux Machine, ofcourse.
> * Nvidia Drivers Installed.
> * Compatible tensorflow version Installed
> * Tensorflow compatible Cuda Version Installed.

1. Make sure You have nvidia Drivers installed <br>
I have nvidia geforce 940mx, So the latest drivers for me was <b>v430.14</b><br>
You can download and install Latest drivers from [here](https://www.nvidia.in/Download/index.aspx?lang=en-in)

2. Reboot and make sure nvidia drivers are installed successfully<br>

    * run {% ihighlight shell %} nvidia-smi {% endihighlight %} in terminal<br>
    It should give some output in tabular form like here :
    
    [![Image](https://i.imgur.com/blHMfTv.png)](https://i.imgur.com/blHMfTv.png)
    
    * make sure Nouveau drivers are disbaled ( These are automatically disabled after nvidia drivers installation)<br> to check: 
    run {% ihighlight shell %} lsmod | grep nouveau {% endihighlight %} in terminal
    
        * If there is no output
            * then its disabled and we are good to go.
        * else
            * Create a file at {% ihighlight shell %} /etc/modprobe.d/blacklist-nouveau.conf {% endihighlight %} with the following contents:
```conf
blacklist nouveau
options nouveau modeset=0
```
            #Regenerate the kernel initramfs:<br>
```shell
sudo update-initramfs -u # (Debian based)
sudo grub-mkconfig -o /boot/grub/grub.cfg # (Arch linux based)
```


3. Download cuda runfile from [this link](https://developer.nvidia.com/cuda-toolkit) (in my case I used cuda 10.0,you can check latest cuda version supported by tf [here](https://www.tensorflow.org/install/gpu))<br> 
            as shown here :<br>
            [![](https://i.imgur.com/2RwjtKA.png)](https://i.imgur.com/2RwjtKA.png)
4. Installing CUDA <br>
    open terminal in the file location folder and run<br>
    * {% ihighlight shell %} sudo sh cuda_10.0.130_410.48_linux.run # change the name of file accordingly {% endihighlight %}        
    * accept the terms and conditons by typing accept when prompt asks for it
    * when a selection menu comes up select everything accept drivers
    * and install
    * wait for installation (might take 5-10 mins)

5. Installing cuDNN 7.5.0 (compatible with cuda 10.0,Note : 10.1 was not supported at that time)<br>
    * Download from https://developer.nvidia.com/cudnn <br>
    NOTE: you will need account to download this file, so create a account if not already
```shell
# select : "cuDNN Library for Linux"
# extract the downloaded file
tar -zxvf cudnn-10.0-linux-x64-v7.5.0.56.tgz
#Move the unpacked contents to your CUDA directory
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda-10.0/lib64/
sudo cp  cuda/include/cudnn.h /usr/local/cuda-10.0/include/
# Give read access to all users
sudo chmod a+r /usr/local/cuda-10.0/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```
6. Install libcupti-dev <br>
```shell
sudo apt-get install libcupti-dev nvidia-modprobe
```
7. Post Install actions (define paths in ~/.bashrc or ~/.zshrc (for zsh users) )<br>
```shell
export PATH=/usr/local/cuda-10.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```
8. Finally install tensorflow-gpu using pip<br>
```shell
pip install --upgrade tensorflow-gpu
```
9. Check the status of tf in python3<br>
    ```python
    from tensorflow.python.client import device_lib
    device_lib.list_local_devices()
    ```
    Output should be something like :
```bash
[name: "/device:CPU:0"
device_type: "CPU"
memory_limit: 268435456
locality {
}
incarnation: 2919472738789042941
, name: "/device:XLA_GPU:0"
device_type: "XLA_GPU"
memory_limit: 17179869184
locality {
}
incarnation: 17258321204030932820
physical_device_desc: "device: XLA_GPU device"
, name: "/device:XLA_CPU:0"
device_type: "XLA_CPU"
memory_limit: 17179869184
locality {
}
incarnation: 1501423454042607117
physical_device_desc: "device: XLA_CPU device"
, name: "/device:GPU:0"
device_type: "GPU"
memory_limit: 1649016832
locality {
    bus_id: 1
    links {
    }
}
incarnation: 1732355850559614807
physical_device_desc: "device: 0, name: GeForce 940MX, pci bus id: 0000:01:00.0, compute capability: 5.0"
]
```
And you will have a Working GPU with tensorflow. Happy Coding.
