# meilin_v2ray_core

 meilin_380固件（7.9.1）_v2ray_core_update
 ------------------------------------------
由于路由器jffs空间有限，此处存放经过UPX压缩的v2ray二进制，以节约空间

此处存放的v2ray二进制来源于v2ray官方项目的releases页面，仅使用UPX处理，用于路由器下的v2ray二进制更新

压缩命令：upx --lzma --ultra-brute v2ray v2ray_armv7 v2ctl

提供的v2ray二进制已经过upx处理压缩，请直接食用

v2ray 从v4.21.0版本开始，v2ray官方项目release页面提供的二进制在博通BCM470X型号CPU上运行出现报错（如RT-AC68U,RT-AC88U等机型），因此从此版本后的v2ray二进制为本项目自编译后，经过upx压缩大小后在此处提供。

编译采用go version 

v2ray提供给fancyss_arm和fancyss_arm384，编译参数为GOARCH=arm, GOARM=5

v2ctl提供给fancyss_arm、fancyss_arm384和fancyss_hnd，，编译参数为GOARCH=arm, GOARM=5

食用方法
-----
使用ssh工具登陆路由，使用Notepad++修改/jffs/.koolshare/scripts/ss_update.sh

修改此脚本的第8行地址main_url=为https://raw.githubusercontent.com/aippnne/meilin_v2ray_core/master/v2ray_binary/
-------------------------------------------------------------------------------------------------------------------------------------
修改注释掉此脚本的第61-67行的内容，保存并推出然后返回插件点击更新v2ray程序即可

