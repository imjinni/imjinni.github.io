---
layout: single
title: "Hello Linux Device Driver"
date: "2023-07-24 14:16:00 +0900"
post-order: 0
last_modified_at: "2023-07-24 14:16:00 +0900"
---

# 리눅스 디바이스 드라이버

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

