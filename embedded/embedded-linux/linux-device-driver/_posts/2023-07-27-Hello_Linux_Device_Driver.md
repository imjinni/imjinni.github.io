---
layout: single
title: "Hello Linux Device Driver"
date: "2023-07-24 14:16:00 +0900"
post-order: 0
last_modified_at: "2023-07-24 14:16:00 +0900"
---

# 리눅스 디바이스 드라이버

## 시작하기 전에...

우선 커널을 컴파일해 시스템을 준비한다.

이 [Link](https://imjinni.github.io/embedded/embedded-linux/kernel/Raspberry_Pi_Linux_Kernel_Compile/)를 참조해 준비하면 된다.

리눅스 디바이스 드라이버를 빌드하기 위해서는 커널 헤더가 필요하다. 헤더만 내려 받아도 컴파일이 가능하나.

여기서는...

- 커널 소스를 참조하기 쉽다.
- 마찬가지로 device tree 찾아 보기 편하다.
- 커널을 빌드해 보는 것도 공부가 된다.
- 실제 해볼 지는 모르겠지만 Kernel API 추가 실습에서 커널 빌드가 필요하다.
- 커널 헤더만 내려 받았을 경우 컴파일을 위해 경로 설정을 추가로 해야 할 수 있다.

등의 이유로 커널을 빌드해 준비한다.

커널은 [Linux LXR kernel 5.10.110](https://elixir.bootlin.com/linux/v5.10.110/source)에서 검색 가능하다.

라즈베리 커널 소스는 [github](https://github.com/raspberrypi/linux/tree/rpi-5.10.y)에서 검색 가능하다.

앞으로 기본 리눅스 관련 코드는 LXR 에서, 라즈베리파이 관련 dts 또는 broadcom 종속적인 내용은 Raspberry pi github 에서 확인하겠다.

## 여기서 정리해 나갈 범위

## insmod 과정

## rmmod 과정

[stat.h](https://github.com/raspberrypi/linux/blob/rpi-5.10.y/include/uapi/linux/stat.h)


| **분류**    | **ID**                      | **Value**                                   | **Desc**                                                 |
| ----------- | --------------------------- | ------------------------------------------- | -------------------------------------------------------- |
| 소유자      | S_IRWXU                     | 00700                                       | owner에 대한 read write, excute 권한 허용                |
| S_IRUSR     | 00400                       | owner에 대한 read 권한 허용                 |                                                          |
| S_IWUSR     | 00200                       | owner에  대한 write 권한 허용               |                                                          |
| S_IXUSR     | 00100                       | group에 대한 excute 권한 허용               |                                                          |
| 그룹        | S_IRWXG                     | 00070                                       | group에 대한 read write, excute 권한 허용                |
| S_IRGRP     | 00040                       | group에 대한 read 권한 허용                 |                                                          |
| S_IWGRP     | 00020                       | group에 대한 write 권한 허용                |                                                          |
| S_IXGRP     | 00010                       | group에 대한 excute 권한 허용               |                                                          |
| 그외 사용자 | S_IRWXO                     | 00007                                       | other에 대한 read write, excute 권한 허용                |
| S_IROTH     | 00004                       | other에 대한 read 권한 허용                 |                                                          |
| S_IWOTH     | 00002                       | other에 대한 read write, excute 권한 허용   |                                                          |
| S_IXOTH     | 00001                       | other에 대한 excute 권한 허용               |                                                          |
| \#define     | S_IRWXUGO                   | (S_IRWXU\|S_IRWXG\|S_IRWXO)                 | owner, group, other에 대한 read, write, excute 권한 허용 |
| S_IRUGO     | (S_IRUSR\|S_IRGRP\|S_IROTH) | owner, group, other에 대한 read, 권한 허용  |                                                          |
| S_IWUGO     | (S_IWUSR\|S_IWGRP\|S_IWOTH) | owner, group, other에 대한 write 권한 허용  |                                                          |
| S_IXUGO     | (S_IXUSR\|S_IXGRP\|S_IXOTH) | owner, group, other에 대한 excute 권한 허용 |                                                          |

