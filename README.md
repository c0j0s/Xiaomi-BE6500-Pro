# 小米路由器 BE6500Pro 折腾手册
## 首先
1. 备份，备份，备份！

## 固件刷机
1. 下载小米路由器修复工具（MIWIFIRepairTool.x86.zip）
1. 下载 `1.0.46` 版本固件 (miwifi_rd08_firmware_076b5_1.0.46.bin)
1. 关闭杀毒软件，防火墙 (尤其是公共网络防火墙!)
1. 跟着提示刷机

## 开启ssh
2025年2月，已下教程依旧有效。
```
https://www.gaicas.com/xiaomi-be6500-pro.html
```
### SSH登入错误
    ```
    SSH No Matching Host Key Type Found
    ```
1. 修改ssh设置， 建议用Linux/macos/WSL.
    ```
    nano ~/.ssh/config
    ```
    写入文件
    ```
    Host *
    HostKeyAlgorithms +ssh-rsa
    PubkeyAcceptedKeyTypes +ssh-rsa
    ```
1. 登入ssh
    ```
    ssh root@192.168.31.1
    ...: yes
    ...: 输入SSH秘钥 (https://www.gaicas.com/miwifi-ssh-key.html)
    .
    .
    .
    BusyBox v1.25.1 (2023-10-24 08:14:11 UTC) built-in shell (ash)

    -----------------------------------------------------
        Welcome to XiaoQiang!
    -----------------------------------------------------
    $$$$$$\  $$$$$$$\  $$$$$$$$\      $$\      $$\        $$$$$$\  $$\   $$\
    $$  __$$\ $$  __$$\ $$  _____|     $$ |     $$ |      $$  __$$\ $$ | $$  |
    $$ /  $$ |$$ |  $$ |$$ |           $$ |     $$ |      $$ /  $$ |$$ |$$  /
    $$$$$$$$ |$$$$$$$  |$$$$$\         $$ |     $$ |      $$ |  $$ |$$$$$  /
    $$  __$$ |$$  __$$< $$  __|        $$ |     $$ |      $$ |  $$ |$$  $$<
    $$ |  $$ |$$ |  $$ |$$ |           $$ |     $$ |      $$ |  $$ |$$ |\$$\
    $$ |  $$ |$$ |  $$ |$$$$$$$$\       $$$$$$$$$  |       $$$$$$  |$$ | \$$\
    \__|  \__|\__|  \__|\________|      \_________/        \______/ \__|  \__|

    ```
    登入成功

### 路由器重启后，开启ssh
1. telnet登入
    ```
    telnet 192.168.31.1

    login: root
    password: admin
    ```
2. 开启ssh
    ```
    sed -i '/flg_ssh=`nvram get ssh_en`/{:loop; N; /\n.*channel=`\/sbin\/uci get \/usr\/share\/xiaoqiang\/xiaoqiang_version.version.CHANNEL`\n.*return 0\n.*fi/!b loop; d}' /etc/init.d/dropbear
    /etc/init.d/dropbear restart
    echo -e 'admin\nadmin' | passwd root
    ```

### 文件传输
```
scp -O source/path root@192.168.31.1:destination/path
```

## 参考
1. https://halo.sherlocky.com/archives/BE6500Pro
1. https://www.gaicas.com/xiaomi-be6500-pro.html
1. https://b23.tv/cFzVHwV