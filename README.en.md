# Xiaomi Router BE6500Pro Scripts
[![cn](https://img.shields.io/badge/lang-cn-green.svg)](README.md)
[![en](https://img.shields.io/badge/lang-en-red.svg)](README.en.md)

## First
1. Backup, backup, backup!

## Firmware Flashing
1. Download the Xiaomi Router Repair Tool (MIWIFIRepairTool.x86.zip)
1. Download the `1.0.46` version firmware (miwifi_rd08_firmware_076b5_1.0.46.bin)
1. Disable antivirus software and firewall (especially public network firewall!)
1. Follow the prompts to flash the firmware

## Enable SSH
As of February 2025, this tutorial is still valid.
```
https://www.gaicas.com/xiaomi-be6500-pro.html
```

### SSH Login Error
    ```
    SSH No Matching Host Key Type Found
    ```
1. Modify SSH settings, recommended to use Linux/macOS/WSL.
    ```
    nano ~/.ssh/config
    ```
    Write to the file
    ```
    Host *
    HostKeyAlgorithms +ssh-rsa
    PubkeyAcceptedKeyTypes +ssh-rsa
    ```
1. Login to SSH
    ```
    ssh root@192.168.31.1
    ...: yes
    ...: Enter SSH key (https://www.gaicas.com/miwifi-ssh-key.html)
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
    Login successful

### Enable SSH after Router Reboot
1. Login via telnet
    ```
    telnet 192.168.31.1

    login: root
    password: admin
    ```
2. Enable SSH
    ```
    sed -i '/flg_ssh=`nvram get ssh_en`/{:loop; N; /\n.*channel=`\/sbin\/uci get \/usr\/share\/xiaoqiang\/xiaoqiang_version.version.CHANNEL`\n.*return 0\n.*fi/!b loop; d}' /etc/init.d/dropbear
    /etc/init.d/dropbear restart
    echo -e 'admin\nadmin' | passwd root
    ```

### File Transfer
```
scp -O source/path root@192.168.31.1:destination/path
```

## References
1. https://halo.sherlocky.com/archives/BE6500Pro
1. https://www.gaicas.com/xiaomi-be6500-pro.html
1. https://b23.tv/cFzVHwV