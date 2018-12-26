# Cool-Ubuntu-For-DL
Setup for a deep learning server.

## Boot with legacy mode
Press E with boot ubuntu live CD, you will see the options.

## Disable nouveau when boot if you are seeing nouveau errors.
* Press 'e'
* Add nouveau.modeset=0 to the end of the linux line, before ----
* Press F10 to boot.

## Fix system lead.
(Using boot-repair in UEFI boot mode)[http://blog.csdn.net/piaocoder/article/details/50589667]

## Before start
I was planning to write a script for install all the depandences. 
But I found that I can not keep up with the updates of all the depandences. 
However, their official sites updates regularly and are getting better and better for researchers. 
Thus, I am writing this to give some basic procedures I would follow.
If you have anything that will facilitates researchers, please feel free to open up a pull request!

First, you should be aware that after you have installed the system and installed the SSH service with
```
sudo apt-get install openssh-server
```
You can login remotely and do following steps all from a remote command line (including driver installation).
And this is clearly a better choice since you have every access to your daily staff and can drink a cup of caffe while do following steps.
And note that using wget to download things you need from a command line would be a good chioce.

## Driver and Cuda installation
* Personally, install cude.deb directly is the best choice and it install drivers automatically. CUDA [official download site](https://developer.nvidia.com/cuda-downloads).
* If above fails, install nvidia driver from the ubuntu 'additional drivers' (type win and search 'additional drivers', you will see it).
* If still fails, install driver with a run file from [official Nvidia Driver site](http://www.nvidia.com/Download/index.aspx?).
* Note that block third-party drivers should not be a hard request from above procedure.
* For cudnn, I prefer follow the [official site](https://developer.nvidia.com/cudnn) to install the three deb files.

## apt-get install
I tried to list every thing we need, but that is hard and it is better we just install things when we need it.

## Anacoda installation
* It should not require a root permission, these I would not install it on a server. Indivitual user should install it theirselves with ```wget``` and ```bash```. Click to [official download site](https://www.anaconda.com/download/#linux).

## Tensorflow installation
I have no commants on this, since I have been away from this platform for long. See [Tensorflow offcial site](https://www.tensorflow.org/install/install_linux).

## Pytorch installation
* It should not require a root permission, these I would not install it on a server. Indivitual user should install it theirselves. See [Pytorch offcial site](http://pytorch.org/).

## OpenCV installation
There is a very cool script to install it, see [here](https://github.com/jayrambhia/Install-OpenCV).
OpenCV with FFMPEG is supported from above script.
However, it seems to be a common case when opencv with FFMPEG fails.
Thus, I highly recommand reading your video files with imageio or some other easy-installed packages, and processing pictures with the opencv installed with conda or pip:
```
pip install opencv-python
```

## Vim
There is a very cool scrip to install a vim with many useful plugins, [here](https://github.com/ma6174/vim-deprecated).

## Some default confit for Git
```
git config --global user.email "YuhangSong.China@gmail.com"
git config --global user.name "YuhangSong"
git config --global push.default "current"
git config --global pull.default "current" 
git config --global credential.helper "cache --timeout=36000000000000000"

git config --local user.email "YuhangSong.China@gmail.com"
git config --local user.name "YuhangSong"
git config --local push.default "current"
git config --local pull.default "current" 
git config --local credential.helper "cache --timeout=36000000000000000"
```

## [Wallpapers](https://github.com/YuhangSong/Pictures)

## SSH personal config
Add ssh config files from [here](https://github.com/YuhangSong/my_ssh) (This is private, you wounldn't need it).

## Atom Settings
Async atom settings from [here](https://github.com/YuhangSong/atom) (This is private, you wounldn't need it).

## [Theme](https://github.com/YuhangSong/.theme)
If you are using your personal computer, you may want to make it beautiful.

## Backup and Restore

### Backup
Use following command from a liveCD to backup the system.
```
sudo su
SYS_ROOT=/media/ubuntu/xxxx
tar --warning='no-file-ignored' --warning='no-file-changed' -cvpzf $SYS_ROOT/backup.tgz --exclude=$SYS_ROOT/backup.tgz --exclude=$SYS_ROOT/proc --exclude=$SYS_ROOT/lost+found --exclude=$SYS_ROOT/mnt --exclude=$SYS_ROOT/sys $SYS_ROOT/
```
```xxxx``` should replaced.

### Restore
Use following command from a liveCD to restore the system.
```
SYS_ROOT=/media/ubuntu/xxxx
tar xvpfz $SYS_ROOT/backup.tgz -C $SYS_ROOT/
mkdir $SYS_ROOT/proc
mkdir $SYS_ROOT/lost+found 
mkdir $SYS_ROOT/mnt 
mkdir $SYS_ROOT/sys
```
```xxxx``` should replaced.

## Forward across a firewall
[Natapp](https://natapp.cn/) is a good choice.

## Auto login a BUAA account
Thank to Daqian Cheng, we have a [auto login script](https://github.com/DaqianCheng/BUAA_Login).

## Boot service
You may need a boot service for above two service. Try use tmux like following:
```sudo vim /etc/rc.local```
add before ```exit 0```
```
tmux new-session -s boot_service_buaa_login -d
tmux send-keys -t boot_service_buaa_login '/home/yuhangsong/BUAA_Login/BUAA_Login.sh' Enter
tmux new-session -s boot_service_ssh_forwarding -d
tmux send-keys -t boot_service_ssh_forwarding '/home/yuhangsong/natapp -authtoken=xxxxx' Enter
```
Add [personal boot settings](https://github.com/YuhangSong/boot_service) (This is private, you wounldn't need it).
