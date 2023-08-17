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
- 실제 해볼지는 모르겠지만 Kernel API를 추가하는 경우 커널 빌드가 필요하다.
- 커널 헤더만 내려 받았을 경우 컴파일을 위해 경로 설정을 추가로 해야 할 수 있다.

등의 이유로 커널을 빌드해 준비한다.

- 바닐라 커널은 [Linux LXR](https://elixir.bootlin.com/linux/v5.10.110/source)에서 검색 가능하다.
- 라즈베리파이 커널 소스는 [RPi Github](https://github.com/raspberrypi/linux/tree/rpi-5.10.y)에서 검색 가능하다.

앞으로 리눅스 기본 코드는 *LXR*에서, 라즈베리파이 관련 dts 또는 broadcom 종속적인 내용은 *RPi Github*에서 확인하겠다.

## hello\_v01

Module을 작성해 보고 동작시켜 확인해 본다.

소스코드다.

```c
#include <linux/module.h>
#include <linux/printk.h>

// MODULE_LICENSE("GPL");

int init_module(void)
{
	pr_info("[imjinni] %s():%d HELLO\n", __func__, __LINE__);

	return 0;
}

void cleanup_module(void)
{
	pr_info("[imjinni] %s():%d BYE\n", __func__, __LINE__);

	return;
}
```

Makefile이다.

```makefile
obj-m := hello_module.o
KDIR := /lib/modules/$(shell uname -r)/build

default:
	make -C$(KDIR) M=$(shell pwd) modules

clean:
	make -C$(KDIR) M=$(shell pwd) clean

```

라이센스 관련 오류가 있거나 warning이 거슬린다면 `MODULE_LICENSE("GPL")`을 살려서 컴파일 하면 된다.

```
$ make
make -C/lib/modules/5.10.110-v8+/build M=/home/pi/smb/wrk-drv/ido/hello_module/hello_v01 modules
make[1]: Entering directory '/home/pi/smb/linux-5.10.y'
  CC [M]  /home/pi/smb/wrk-drv/ido/hello_module/hello_v01/hello_module.o
  MODPOST /home/pi/smb/wrk-drv/ido/hello_module/hello_v01/Module.symvers
WARNING: modpost: missing MODULE_LICENSE() in /home/pi/smb/wrk-drv/ido/hello_module/hello_v01/hello_module.o
  CC [M]  /home/pi/smb/wrk-drv/ido/hello_module/hello_v01/hello_module.mod.o
  LD [M]  /home/pi/smb/wrk-drv/ido/hello_module/hello_v01/hello_module.ko
make[1]: Leaving directory '/home/pi/smb/linux-5.10.y'
```

`modinfo`로 모듈 정보를 확인 할 수 있다.

```
$ modinfo hello_module.ko
filename:       /home/pi/smb/wrk-drv/ido/hello_module/hello_v01/hello_module.ko
srcversion:     6DD544FCD750AC231C89D5C
depends:
name:           hello_module
vermagic:       5.10.110-v8+ SMP preempt mod_unload modversions aarch64
```

`insmod`로 모듈을 삽입한다.

```
$ sudo insmod hello_module.ko
```

`dmesg`로 커널 로그를 확인해 `pr_info`로 출력한 메시지를 확인한다.

```
$ dmesg | tail -3
[34432.576465] hello_module: module license 'unspecified' taints kernel.
[34432.576476] Disabling lock debugging due to kernel taint
[34432.576749] [imjinni] init_module():8 HELLO
```

`lsmod`로 현재 load된 모듈을 확인 할 수 있다.

```
$ lsmod | grep -i hello
hello_module           16384  0
```

`rmmod`로 모듈을 제거한다.

```
$ sudo rmmod hello_module
```

`dmesg`로 로그를 확인한다.

```
$ dmesg | tail -1
[34490.610438] [imjinni] cleanup_module():15 BYE
```

## hello\_v02

```c
#include <linux/init.h>
#include <linux/module.h>
#include <linux/printk.h>

MODULE_LICENSE("GPL");
MODULE_AUTHOR("imjinni@gmail.com");
MODULE_DESCRIPTION("hello module");

// MODULE_INFO(intree, "Y");
MODULE_INFO(priv, "kernel hello module v02");

static int test_init_data __initdata = 0xdeadbeef;

static int __init hello_init(void)
{
	pr_info("[imjinni] %s():%d HELLO\n", __func__, __LINE__);

	pr_info("    test_init_data is [0x%x]\n", test_init_data);

	return 0;
}

static void __exit hello_exit(void)
{
	pr_info("[imjinni] %s():%d BYE\n", __func__, __LINE__);

	return;
}

module_init(hello_init);
module_exit(hello_exit);
```

## hello\_v03

```c
#include <linux/init.h>
#include <linux/module.h>
#include <linux/printk.h>

#include <linux/kernel.h>
#include <linux/moduleparam.h>
#include <linux/stat.h>

MODULE_LICENSE("GPL");
MODULE_AUTHOR("imjinni@gmail.com");
MODULE_DESCRIPTION("hello module");

// MODULE_INFO(intree, "Y");
MODULE_INFO(priv, "kernel hello module v03");

#define S_IRWUG		(S_IRUSR | S_IWUSR | S_IRGRP | S_IWGRP)
#define S_IRWUSR	(S_IRUSR | S_IWUSR)
#define S_IRGO		(S_IRGRP | S_IROTH)

static int test_init_data __initdata = 0xdeadbeef;

static short int var_short = 1;
static int var_int = 2;
static long int var_long = 3;
static char *var_string = "unknown";
static int var_int_array[2] = { 90, 91 };
static int array_count = 0;
static bool var_bool = false;

module_param(var_short, short, S_IRWUG);
MODULE_PARM_DESC(var_short, "Short Integer");
module_param(var_int, int, S_IRWUSR | S_IRGO);
MODULE_PARM_DESC(var_int, "Integer");
module_param(var_long, long, S_IRUSR);
MODULE_PARM_DESC(var_long, "Long Integer");
module_param(var_string, charp, 0000);
MODULE_PARM_DESC(var_string, "String");
module_param(var_bool, bool, S_IRWUG);
MODULE_PARM_DESC(var_bool, "Bool");

module_param_array(var_int_array, int, &array_count, 0000);
MODULE_PARM_DESC(var_int_array, "Integer Array");

int hello_print_params(void)
{
	int i;

	pr_info("[imjinni] %s():%d\n", __func__, __LINE__);
	pr_info("    var_short           ==> %hd\n", var_short);
	pr_info("    var_int             ==> %d\n", var_int);
	pr_info("    var_long            ==> %ld\n", var_long);
	pr_info("    var_string          ==> %s\n", var_string);
	pr_info("    var_bool            ==> %s\n", var_bool ? "true" : "false");

	pr_info("\n");
	pr_info("    var_int_array count ==> %d\n", array_count);

	for (i = 0; i < ARRAY_SIZE(var_int_array); i++)
	{
		pr_info("    var_int_array[%d] = %d\n", i, var_int_array[i]);
	}

	return 0;
}

static int __init hello_init(void)
{
	pr_info("[imjinni] %s():%d HELLO\n", __func__, __LINE__);

	pr_info("    test_init_data is [0x%x]\n", test_init_data);

	hello_print_params();

	return 0;
}

static void __exit hello_exit(void)
{
	pr_info("[imjinni] %s():%d BYE\n", __func__, __LINE__);

	return;
}

module_init(hello_init);
module_exit(hello_exit);
```



### Module Parameter 처리

`S_IRUSR`과 같은 상수는 [stat.h](https://elixir.bootlin.com/linux/v5.10.110/source/include/uapi/linux/stat.h)에서 찾을 수 있다.


| **분류**    | **ID**                      | **Value**                                   | **Desc**                                                 |
| ----------- | --------------------------- | ------------------------------------------- | -------------------------------------------------------- |
| Owner      | S_IRWXU                     | 00700                                       | owner에 대한 read write, excute 권한 허용                |
| S_IRUSR     | 00400                       | owner에 대한 read 권한 허용                 |                                                          |
| S_IWUSR     | 00200                       | owner에  대한 write 권한 허용               |                                                          |
| S_IXUSR     | 00100                       | owner에 대한 excute 권한 허용               |                                                          |
| Group        | S_IRWXG                     | 00070                                       | group에 대한 read write, excute 권한 허용                |
| S_IRGRP     | 00040                       | group에 대한 read 권한 허용                 |                                                          |
| S_IWGRP     | 00020                       | group에 대한 write 권한 허용                |                                                          |
| S_IXGRP     | 00010                       | group에 대한 excute 권한 허용               |                                                          |
| Others | S_IRWXO                     | 00007                                       | other에 대한 read write, excute 권한 허용                |
| S_IROTH     | 00004                       | other에 대한 read 권한 허용                 |                                                          |
| S_IWOTH     | 00002                       | other에 대한 read write, excute 권한 허용   |                                                          |
| S_IXOTH     | 00001                       | other에 대한 excute 권한 허용               |                                                          |
| Combination     | S_IRWXUGO                   | (S_IRWXU\|S_IRWXG\|S_IRWXO)                 | owner, group, other에 대한 read, write, excute 권한 허용 |
| S_IRUGO     | (S_IRUSR\|S_IRGRP\|S_IROTH) | owner, group, other에 대한 read, 권한 허용  |                                                          |
| S_IWUGO     | (S_IWUSR\|S_IWGRP\|S_IWOTH) | owner, group, other에 대한 write 권한 허용  |                                                          |
| S_IXUGO     | (S_IXUSR\|S_IXGRP\|S_IXOTH) | owner, group, other에 대한 excute 권한 허용 |                                                          |

## hello\_v04

*hello\_v03*의 파일을 나누었다. 별다른 설명은 없고 코드와 Makefile을 확인하면 된다. 너무 큰 드라이버는 이렇게도 할 수 있다.

## insmod 과정

## rmmod 과정