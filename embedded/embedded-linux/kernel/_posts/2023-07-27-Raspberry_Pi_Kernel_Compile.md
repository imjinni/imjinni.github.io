---
layout: single
title: "Raspberry pi Linux Kernel Compile"
date: "2023-07-24 14:17:00 +0900"
post-order: 0
last_modified_at: "2023-07-24 14:17:00 +0900"
---

# 리눅스 커널 컴파일

## 이 문서는...

이 문서는 라즈베리파이의 리눅스 커널 컴파일 과정을 요약한다.

여기서 커널 컴파일을 하는 목적은 컴파일에 있는게 아니라 디바이스 드라이버와 커널을 탐구하기 위한 셋팅으로서의 필요에 있다. 그래서 추후 추가 될 수는 있겠으나 보드 상에서 Native Compile을 기준으로 작업해 각종 설정을 최소화 한다.

기본 참고 사항은 Raspberry pi Document의 [Reference](https://www.raspberrypi.com/documentation/computers/linux_kernel.html)를 따른다.

타겟 보드는 RPi 4B, 커널은 5.10을 기준의로 한다.

4B는 현재 최신의 보드라 사용했고, 커널은 기본 참고 소스들이 이 버전 커널에서 빌드 가능한 상태이며 현재 실무에 사용중인 버전이라 선택했다.

## 빌드 과정
