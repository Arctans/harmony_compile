参考 https://gitee.com/openharmony/docs/blob/master/zh-cn/device-dev/get-code/sourcecode-acquire.md#https://gitee.com/link?target=https%3A%2F%2Frepo.huaweicloud.com%2Fopenharmony%2Fos%2F4.0-Release%2Fdayu200_standard_arm32.tar.gz

设备开发
	https://gitee.com/openharmony/docs/blob/master/zh-cn/device-dev/Readme-CN.md


尝试一、
1、gitea 下载鸿蒙系统
	
	a、注册码云gitee帐号。
	b、注册码云SSH公钥，请参考码云帮助中心。
	c、安装git客户端和git-lfs并配置用户信息。
		sudo apt-get install git 
		sudo apt-get install git-lfs
2、修改ubuntu 系统的shell脚本为bash
	sudo dpkg-reconfigure dash  选择no
3、源码下载后 下载编译工具 执行./build.sh 提示需要下载 prebuild 编译工具	
	./build/prebuilts_download.sh
	提示
	You should consider upgrading via the '/home/arctan/work/workspace/harmonyOS/gitea_harmonyOS/prebuilts/python/linux-x86/3.10.2/bin/python3.10 -m pip install --upgrade pip' command.

4、编译配置
	安装 ohos-build 安装后位于~/.local/bin
	python3 -m pip install --user ohos-build
	设置环境变量
	export PATH=/home/arctan/.local/bin:$PATH

5、Hb -h出错
	Traceback (most recent call last):
	File “/home/demo/.local/bin/hb”, line 8, in
	sys.exit(main())
	File “/home/demo/.local/lib/python3.10/site-packages/hb/main.py”, line 49, in main
	topdir = find_top()
	File “/home/demo/.local/lib/python3.10/site-packages/hb/main.py”, line 37, in find_top
	raise
	Exception: Please call hb utilities inside source root directory

    a、卸载pip python3 -m pip uninstall ohos-build
    b、安装python3 -m pip install --user ohos-build==0.4.3
    此时应该就可以了如果再次输入hb -h报错如下

	c、vim ~/.local/lib/python3.10/sitepackages/prompt_toolkit/styles/from_dict.py
		打开后将from collections import Mapping改为from collections.abc import Mapping
6、hb set  选择 qemu-arm-linux-min 错误
	OHOS Which product do you need?  qemu-arm-linux-min
	[OHOS ERROR] Please run command "hb set" to init OHOS development environment
	
	执行  python3 -m  pip install build/lite
		  python3 -m  pip install ohos-build 
		  pip3 install build/lite
 

尝试二、
	下载源码后

	1、执行 ./build/build_scripts/env_setup.sh
	2、bash build/prebuilts_download.sh
		警告 WARNING: You are using pip version 21.2.4; however, version 23.0.1 is available.
		You should consider upgrading via the '/home/arctan/work/workspace/harmonyOS/gitea_harmonyOS/prebuilts/python/linux-x86/3.10.2/bin/python3.10 -m pip install --upgrade pip' command.
		执行 python3.10 -m pip install --upgrade pip

		提示如下：
			Defaulting to user installation because normal site-packages is not writeable
			Requirement already satisfied: pip in /usr/lib/python3/dist-packages (22.0.2)
	Collecting pip
Downloading pip-23.3.2-py3-none-any.whl (2.1 MB)
	━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 MB 1.8 MB/s eta 0:00:00
	Installing collected packages: pip
	WARNING: The scripts pip, pip3, pip3.10 and pip3.11 are installed in '/home/arctan/.local/bin' which is not on PATH.
	Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
	Successfully installed pip-23.3.2

	3、执行 ./build.sh --product-name rk3568 --gn-args is_debug=true
	   
	   编译错误
		[OHOS ERROR] scripts/Kconfig.include:39: compiler 'ccache clang' not found
		[OHOS ERROR] make[2]: *** [/home/arctan/work/workspace/harmonyOS/gitea_harmonyOS/out/kernel/src_tmp/linux-5.10/scripts/kconfig/Makefile:88：rockchip_linux_defconfig] 错误 1
		[OHOS ERROR] make[1]: *** [/home/arctan/work/workspace/harmonyOS/gitea_harmonyOS/out/kernel/src_tmp/linux-5.10/Makefile:616：rockchip_linux_defconfig] 错误 2

		sudo apt-get install -y apt-utils binutils bison flex bc build-essential make mtd-utils gcc-arm-linux-gnueabi u-boot-tools python3.9 python3-pip git zip unzip curl wget gcc g++ ruby dosfstools mtools default-jre default-jdk scons python3-distutils perl openssl libssl-dev cpio git-lfs m4 ccache zlib1g-dev tar rsync liblz4-tool genext2fs binutils-dev device-tree-compiler e2fsprogs git-core gnupg gnutls-bin gperf lib32ncurses5-dev libffi-dev zlib* libelf-dev libx11-dev libgl1-mesa-dev lib32z1-dev xsltproc x11proto-core-dev libc6-dev-i386 libxml2-dev lib32z-dev libdwarf-dev 
		
		sudo apt-get install -y grsync xxd libglib2.0-dev libpixman-1-dev kmod jfsutils reiserfsprogs xfsprogs squashfs-tools  pcmciautils quota ppp libtinfo-dev libtinfo5 libncurses5 libncurses5-dev libncursesw5 libstdc++6  gcc-arm-none-eabi vim ssh locales doxygen

		sudo apt-get install -y libxinerama-dev libxcursor-dev libxrandr-dev libxi-dev
	
	4、修改后再次编译报错 exceptions.ohos_exception.OHOSException: COMPILE Failed! Please check error in /home/arctan/work/workspace/harmonyOS/gitea_harmonyOS/out/rk3568/error.log, and for more build information in /home/arctan/work/workspace/harmonyOS/gitea_harmonyOS/out/rk3568/build.log
		log 显示
		../../third_party/abseil-cpp/abseil-cpp/absl/status/statusor.cc:70:63: error: function 'HandleInvalidStatusCtorArg' could be declared with attribute 'noreturn' [-Werror,-Wmissing-noreturn]
		修改
		third_party/abseil-cpp/abseil-cpp/absl/status/internal/statusor_internal.h
		添加 noreturn
		static void HandleInvalidStatusCtorArg(Status*) __attribute__((noreturn));

三、rk3568 成功编译
	./build.sh --product-name rk3568

四、qemu 编译
	a、参考网址:
		https://gitee.com/openharmony/device_qemu/blob/HEAD/arm_virt/linux/README_zh.md

		./build.sh --product-name qemu-arm-linux-min --ccache --jobs 4
		./build.sh --product-name qemu-arm-linux-headless --ccache --jobs 4
	b、


五、编译错误
	[OHOS ERROR]   File "/home/arctan/work/workspace/harmonyOS/gitea_harmonyOS/build/hb/util/log_util.py", line 180, in get_compiler_failed_log
	[OHOS ERROR]     raise OHOSException(
	[OHOS ERROR] exceptions.ohos_exception.OHOSException: COMPILE Failed! Please check error in /home/arctan/work/workspace/harmonyOS/gitea_harmonyOS/out/rk3568/error.log, and for more build information in /home/arctan/work/workspace/harmonyOS/gitea_harmonyOS/out/rk3568/build.log
	[OHOS ERROR] 
	
	[OHOS ERROR] Code:        4000
	[OHOS ERROR] 
	[OHOS ERROR] Reason:      COMPILE Failed! Please check error in /home/arctan/work/workspace/harmonyOS/gitea_harmonyOS/out/rk3568/error.log, and for more build information in /home/arctan/work/workspace/harmonyOS/gitea_harmonyOS/out/rk3568/build.log
	[OHOS ERROR] 
	[OHOS ERROR] Error Type:  Ninja build error
	[OHOS ERROR] 
	[OHOS ERROR] Description: An unknown error occurred while executing 'ninja -C'.
	[OHOS ERROR] 
	[OHOS ERROR] Solution:    no solution

	解决
		sudo apt-get install ninja-build



六、重新编译
	1、安装git 工具，注册git 账号
		a、安装git 工具
			sudo apt install git-all
		b、生成git 秘钥
		   ssh-keygen -t ed25519 -C "Gitee SSH Key"
		c、注册git 账号 并添加 公钥到账号 
			https://gitee.com/
			cat ~/.ssh/id_ed25519.pub
	2、安装repo
		a、下载	
			mkdir ~/bin
			curl https://gitee.com/oschina/repo/raw/fork_flow/repo-py3 -o ~/bin/repo
			chmod a+x ~/bin/repo
			pip3 install -i https://repo.huaweicloud.com/repository/pypi/simple requests
		b、将repo 添加到环境变量
			vim ~/.bashrc               # 编辑环境变量
			export PATH=~/bin:$PATH     # 在环境变量的最后添加一行repo路径信息
			source ~/.bashrc            # 应用环境变量
	3、下载编译工具
		a、安装编译工具
		sudo apt-get install -y apt-utils binutils bison flex bc build-essential make mtd-utils gcc-arm-linux-gnueabi u-boot-tools python3.9 python3-pip git zip unzip curl wget gcc g++ ruby dosfstools mtools default-jre default-jdk scons python3-distutils perl openssl libssl-dev cpio git-lfs m4 ccache zlib1g-dev tar rsync liblz4-tool genext2fs binutils-dev device-tree-compiler e2fsprogs git-core gnupg gnutls-bin gperf lib32ncurses5-dev libffi-dev zlib* libelf-dev libx11-dev libgl1-mesa-dev lib32z1-dev xsltproc x11proto-core-dev libc6-dev-i386 libxml2-dev lib32z-dev libdwarf-dev 

		sudo apt-get install -y grsync xxd libglib2.0-dev libpixman-1-dev kmod jfsutils reiserfsprogs xfsprogs squashfs-tools  pcmciautils quota ppp libtinfo-dev libtinfo5 libncurses5 libncurses5-dev libncursesw5 libstdc++6  gcc-arm-none-eabi vim ssh locales doxygen

		sudo apt-get install -y libxinerama-dev libxcursor-dev libxrandr-dev libxi-dev


		b、build/prebuilts_download.sh
		   安装完成
			Installing collected packages: pycparser, wcwidth, urllib3, six, idna, charset-normalizer, cffi, certifi, requests, pyyaml, prompt-toolkit, json5, cryptography, asn1crypto
			Successfully installed asn1crypto-1.5.1 certifi-2023.11.17 cffi-1.16.0 charset-normalizer-3.3.2 cryptography-41.0.5 idna-3.4 json5-0.9.6 prompt-toolkit-1.0.14 pycparser-2.21 pyyaml-6.0.1 requests-2.31.0 six-1.16.0 urllib3-2.1.0 wcwidth-0.2.5
			WARNING: You are using pip version 21.2.4; however, version 23.0.1 is available.
			You should consider upgrading via the '/home/arctan/work/workspace/harmonyOS/harmony_master/prebuilts/python/linux-x86/3.10.2/bin/python3.10 -m pip install --upgrade pip' command.
			======copy inside cxx finished!======
			======update llvm ndk finished!======
			======change rustlib name finished!======
			Created /home/arctan/work/workspace/harmonyOS/harmony_master/prebuilts/clang/ohos/linux-x86_64/llvm/bin/lldb-mi
			Created /home/arctan/work/workspace/harmonyOS/harmony_master/prebuilts/clang/ohos/windows-x86_64/llvm/bin/lldb-mi.exe
	4、编译rk3568
		a、配置dash 环境
			执行 sudo dpkg-reconfigure dash	选择否
		b、编译
			./build.sh --product-name rk3568 --ccache --jobs 4
			or
			./build.sh --product-name rk3568



	5、不同版本的鸿蒙hb编译
		a、鸿蒙3.2
			python3 -m pip install --user ohos-build
			export 配置环境变量
				export PATH=/home/arctan/.local/bin:$PATH
			或修改 ~/.bashrc 配置环境变量
				export PATH=/home/arctan/.local/bin:$PATH

		b、鸿蒙4.1 和master	
			如果已安装了鸿蒙3.2 的  ohohs-build 先卸载
				pip python3 -m pip uninstall ohos-build
			然后再安装
				python3 -m  pip install build/lite
