# Arch折腾记录

#### 安装openssh服务

```shell
pacman -S openssh //安装openssh服务
systemctl start sshd.service //启动openssh服务
systemctl enable sshd.service //设置开机启动ssh服务
vim /etc/ssh/sshd_config //编辑sshd配置文件
  # Authentication:   //运行root用户进行ssh登入
  LoginGraceTime 2m  //将注释#号去掉
  #PermitRootLogin prohibit-password
  PermitRootLogin yes   //添加一行
  StrictModes yes      //将注释#号去掉
```



#### 软件源

```shell
vim /etc/pacman.conf

# aur
## 莞工 GNU/Linux 协会 开源软件镜像站 (广东东莞) (ipv4, https)
## Added: 2018-11-03
[archlinuxcn]
Server = https://mirrors.dgut.edu.cn/archlinuxcn/$arch

#更新软件包缓存：
sudo pacman -Syy

sudo pacman -S yay
yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save

sudo pacman -Syu
sudo pacman -S archlinuxcn-keyring
```

