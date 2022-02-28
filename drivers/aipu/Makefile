#Copyright (c) 2018-2021 Arm Technology (China) Co., Ltd. All rights reserved.
MODULE_NAME := aipu
SRC_DIR := src

DIR=drivers/aipu/
DEBUG_FLAG=BUILD_RELEASE_VERSION
VERSION_FLAG=BUILD_ZHOUYI_V1
PLATFORM_FLAG=BUILD_PLATFORM_R329_MAINLINE

EXTRA_CFLAGS += -D$(PLATFORM_FLAG)=1 -D$(VERSION_FLAG)=1 -I$(DIR)/src -I$(DIR)/src/aipu \
                   -I$(DIR)/src/aipu/zhouyi -I$(DIR)/src/aipu/soc -I$(DIR)/include/uapi/misc

ifeq ($(DEBUG_FLAG), BUILD_DEBUG_VERSION)
	EXTRA_CFLAGS += -DBUILD_DEBUG_VERSION=1
else
    EXTRA_CFLAGS += -DBUILD_DEBUG_VERSION=0
endif

COMM_OBJ := $(SRC_DIR)/aipu/zhouyi/zhouyi.o \
            $(SRC_DIR)/aipu/aipu_core.o \
            $(SRC_DIR)/aipu_priv.o \
            $(SRC_DIR)/aipu_io.o \
            $(SRC_DIR)/aipu_irq.o \
            $(SRC_DIR)/aipu_mm.o \
            $(SRC_DIR)/aipu_job_manager.o

ifeq ($(VERSION_FLAG), BUILD_ZHOUYI_V1)
    AIPU_OBJ := $(SRC_DIR)/aipu/zhouyi/z1/z1.o
else ifeq ($(VERSION_FLAG), BUILD_ZHOUYI_V2)
    AIPU_OBJ := $(SRC_DIR)/aipu/zhouyi/z2/z2.o
else
    AIPU_OBJ := $(SRC_DIR)/aipu/zhouyi/z1/z1.o \
            $(SRC_DIR)/aipu/zhouyi/z2/z2.o
endif

INIT_OBJ := $(SRC_DIR)/aipu.o
OBJS     := $(COMM_OBJ) $(AIPU_OBJ)

ifneq ($(KERNELRELEASE),)
	obj-m := $(MODULE_NAME).o
	$(MODULE_NAME)-objs := $(INIT_OBJ) $(OBJS)
else
	KDIR ?= /home/juwan/R329/linux # host compile or arm64 use # KDIR ?= /lib/modules/`uname -r`/build
	PWD :=$(shell pwd)

all:
	$(MAKE) -C $(KDIR) M=$(PWD) modules
clean:
	$(MAKE) -C $(KDIR) M=$(PWD) clean
endif