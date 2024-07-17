ifneq ($(KERNELRELEASE),)
        obj-m := asus-ec-sensors.o
else
        KERNELDIR ?= /root/build/linux-5.10.x
        PWD := $(shell pwd)
KVER ?= "5.10.55+"
ASUS_EC_SENSORS_CFLAGS=""
KVER_NUM = $(word 1, $(subst -, ,$(KVER)))

HAVE_MILLI := $(shell /bin/echo -e "5.15\n$(KVER_NUM)"|\
sort -Ct. -k1,1n -k2,2n && echo YES)
ifneq ("YES", "$(HAVE_MILLI)")
	ASUS_EC_SENSORS_CFLAGS +=-DMILLI=1000UL
endif

default:
	$(MAKE) EXTRA_CFLAGS="$(ASUS_EC_SENSORS_CFLAGS)" -C $(KERNELDIR) M=$(PWD) modules
clean:
	$(MAKE) EXTRA_CFLAGS="$(ASUS_EC_SENSORS_CFLAGS)" -C $(KERNEL_PATH) M=$(PWD) clean
endif
