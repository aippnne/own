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

使用ssh工具登陆路由，使用Notepad+=修改/jffs/.koolshare/scripts/ss_update.sh脚本的第8行地址为https://raw.githubusercontent.com/aippnne/meilin_v2ray_core/master/v2ray_binary/
-------------------------------------------------------------------------------------------------------------------------------------
#!/bin/sh

# shadowsocks script for AM380 merlin firmware
# by sadog (sadoneli@gmail.com) from koolshare.cn

eval `dbus export ss`
alias echo_date='echo 【$(TZ=UTC-8 date -R +%Y年%m月%d日\ %X)】:'
main_url="https://raw.githubusercontent.com/aippnne/meilin_v2ray_core/master/v2ray_binary/"
backup_url=""

install_ss(){
	echo_date 开始解压压缩包...
	tar -zxf shadowsocks.tar.gz
	chmod a+x /tmp/shadowsocks/install.sh
	echo_date 开始安装更新文件...
	sh /tmp/shadowsocks/install.sh
	rm -rf /tmp/shadowsocks*
}

update_ss(){
	echo_date 更新过程中请不要刷新本页面或者关闭路由等，不然可能导致问题！
	echo_date 开启SS检查更新：使用主服务器：github
	echo_date 检测主服务器在线版本号...
	ss_basic_version_web1=`curl --connect-timeout 5 -s "$main_url"/version | sed -n 1p`
	if [ -n "$ss_basic_version_web1" ];then
		echo_date 检测到主服务器在线版本号：$ss_basic_version_web1
		dbus set ss_basic_version_web=$ss_basic_version_web1
		if [ "$ss_basic_version_local" != "$ss_basic_version_web1" ];then
		echo_date 主服务器在线版本号："$ss_basic_version_web1" 和本地版本号："$ss_basic_version_local" 不同！
			cd /tmp
			md5_web1=`curl -s "$main_url"/version | sed -n 2p`
			echo_date 开启下载进程，从主服务器上下载更新包...
			wget --no-check-certificate --timeout=5 "$main_url"/shadowsocks.tar.gz
			md5sum_gz=`md5sum /tmp/shadowsocks.tar.gz | sed 's/ /\n/g'| sed -n 1p`
			if [ "$md5sum_gz" != "$md5_web1" ]; then
				echo_date 更新包md5校验不一致！估计是下载的时候出了什么状况，请等待一会儿再试...
				rm -rf /tmp/shadowsocks* >/dev/null 2>&1
				sleep 1
				echo_date 更换备用备用更新地址，请稍后...
				sleep 1
				update_ss2
			else
				echo_date 更新包md5校验一致！ 开始安装！...
				install_ss
			fi
		else
			echo_date 主服务器在线版本号："$ss_basic_version_web1" 和本地版本号："$ss_basic_version_local" 相同！
			echo_date 退出插件更新!
			sleep 1
			exit
		fi
	else
		echo_date 没有检测到主服务器在线版本号,访问github服务器可能有点问题！
		sleep 1
		echo_date 更换备用备用更新地址，请稍后...
		sleep 1
		update_ss2
	fi
}

#update_ss2(){
#	echo_date "目前还没有任何备用服务器！请尝试使用离线安装功能！"
#	echo_date "历史版本下载地址：https://github.com/idealism-xxm/fancyss/tree/master/fancyss_arm/history_package"
#	echo_date "下载后请将下载包名字改为：shadowsocks.tar.gz，再使用离线安装进行安装"
#	sleep 1
#	exit
#}

修改
