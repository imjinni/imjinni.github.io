---
layout: single
title: "Raspberry Pi Linux Kernel Compile"
date: "2023-07-24 14:17:00 +0900"
post-order: 0
last_modified_at: "2023-07-24 14:17:00 +0900"
---

# 리눅스 커널 컴파일

## 이 문서는...

이 문서는 라즈베리파이의 리눅스 커널 컴파일 과정을 요약한다.

여기서 커널 컴파일을 하는 목적은 컴파일 자체가 아니라 디바이스 드라이버와 커널을 공부하기 위한 셋팅을 하기 위해서이다. 추후 추가 될 수는 있겠으나 보드 상에서 Native Compile을 기준으로 작업해 각종 설정을 최소화 한다.

기본 참고 사항은 Raspberry pi Document의 [Reference](https://www.raspberrypi.com/documentation/computers/linux_kernel.html)를 따른다.

타겟 보드는 RPi 4B, 커널은 5.10을 기준의로 한다.

4B는 현재 최신의 보드라 사용했고, 커널은 기본 참고 소스들이 이 버전 커널에서 빌드 가능한 상태이며 현재 실무에 사용중인 버전이라 선택했다.

## 커널 소스

빌드에 필요한 유틸리티를 받는다.

```
$ sudo apt install git bc bison flex libssl-dev make
```

커널 소스를 받는다.

```
$ git clone --depth=1 --branch rpi-5.10.y https://github.com/raspberrypi/linux linux-5.10.y

Cloning into 'linux-5.10.y'...
remote: Enumerating objects: 75576, done.
remote: Counting objects: 100% (75576/75576), done.
remote: Compressing objects: 100% (71823/71823), done.
remote: Total 75576 (delta 6160), reused 16645 (delta 2954), pack-reused 0
Receiving objects: 100% (75576/75576), 199.27 MiB | 6.52 MiB/s, done.
Resolving deltas: 100% (6160/6160), done.
```

## 기본 Config 적용

Raspberry pi 4 64bit 기본 Config 를 적용한다.

```
$ KERNEL=kernel8

$ make bcm2711_defconfig

  HOSTCC  scripts/basic/fixdep
  HOSTCC  scripts/kconfig/conf.o
  HOSTCC  scripts/kconfig/confdata.o
  HOSTCC  scripts/kconfig/expr.o
  LEX     scripts/kconfig/lexer.lex.c
  YACC    scripts/kconfig/parser.tab.[ch]
  HOSTCC  scripts/kconfig/lexer.lex.o
  HOSTCC  scripts/kconfig/parser.tab.o
  HOSTCC  scripts/kconfig/preprocess.o
  HOSTCC  scripts/kconfig/symbol.o
  HOSTCC  scripts/kconfig/util.o
  HOSTLD  scripts/kconfig/conf
#
# configuration written to .config
#
```

커널 버전의 이름을 설정해 놓는다.

```
$ CONFIG_LOCALVERSION="v8+ CUSTOM-KERNEL"
```

## 커널 빌드

```
$ make -j4 Image.gz modules dtbs
```

## 설치

```
$ sudo make modules_install
$ sudo cp arch/arm64/boot/dts/broadcom/*.dtb /boot/
$ sudo cp arch/arm64/boot/dts/overlays/*.dtb* /boot/overlays/
$ sudo cp arch/arm64/boot/dts/overlays/README /boot/overlays/
$ sudo cp arch/arm64/boot/Image.gz /boot/$KERNEL.img
```

## 확인

재부팅 후 부팅한 커널 버전을 확인한다.

```
$ uname -a
Linux raspberrypi 5.10.110-v8+ #1 SMP PREEMPT Mon Aug 14 16:11:07 KST 2023 aarch64 GNU/Linux
```

```
/lib/modules/5.10.110-v8+$ ls -al
total 2440
drwxr-xr-x  3 root root   4096 Aug 14 17:44 .
drwxr-xr-x  9 root root   4096 Jul  8 17:33 ..
lrwxrwxrwx  1 root root     25 Aug 14 17:44 build -> /home/pi/smb/linux-5.10.y
drwxr-xr-x 11 root root   4096 Aug 14 17:44 kernel
-rw-r--r--  1 root root 594716 Aug 14 17:44 modules.alias
-rw-r--r--  1 root root 618213 Aug 14 17:44 modules.alias.bin
-rw-r--r--  1 root root  14651 Aug 14 17:44 modules.builtin
-rw-r--r--  1 root root      0 Aug 14 17:44 modules.builtin.alias.bin
-rw-r--r--  1 root root  16170 Aug 14 17:44 modules.builtin.bin
-rw-r--r--  1 root root  82636 Aug 14 17:44 modules.builtin.modinfo
-rw-r--r--  1 root root 201598 Aug 14 17:44 modules.dep
-rw-r--r--  1 root root 277678 Aug 14 17:44 modules.dep.bin
-rw-r--r--  1 root root    384 Aug 14 17:44 modules.devname
-rw-r--r--  1 root root  66219 Aug 14 17:44 modules.order
-rw-r--r--  1 root root    407 Aug 14 17:44 modules.softdep
-rw-r--r--  1 root root 264410 Aug 14 17:44 modules.symbols
-rw-r--r--  1 root root 321553 Aug 14 17:44 modules.symbols.bin
lrwxrwxrwx  1 root root     25 Aug 14 17:44 source -> /home/pi/smb/linux-5.10.y
```
