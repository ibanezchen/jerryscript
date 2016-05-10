# Copyright 2016 Samsung Electronics Co., Ltd.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# use TAB-8

TYPE      ?= release
JERRYHEAP ?= 64
PREFIX     =$(shell pwd)/../prefix/arm-none-eabi
HYPERCLIB   = $(PREFIX)/lib
INTERM     = build/obj-hyperc
OUTPUT     = build/bin/$(TYPE).hyperc
COPYTARGET = $(HYPERCLIB)

EXT_CFLAGS := -mlittle-endian -mthumb -march=armv7-m
EXT_CFLAGS += -mfpu=fpv4-sp-d16 -mfloat-abi=hard -Wno-conversion

all:
	mkdir -p $(INTERM)
	mkdir -p $(OUTPUT)
	cmake -B$(INTERM) -H./ \
	 -DENABLE_LTO=OFF \
	 -DENABLE_VALGRIND=OFF \
	 -DCMAKE_TOOLCHAIN_FILE=build/configs/toolchain_external.cmake \
	 -DEXTERNAL_CMAKE_SYSTEM_PROCESSOR=armv7l-hf \
	 -DEXTERNAL_CMAKE_C_COMPILER=arm-none-eabi-gcc \
	 -DEXTERNAL_CMAKE_C_COMPILER_ID=GNU \
	 -DEXTERNAL_BUILD_ENTRY_FILE=./targets/hyperc/main-hyperc.c \
	 -DEXTERNAL_COMPILE_FLAGS="$(EXT_CFLAGS)" \
	 -DEXTERNAL_LIBC_INTERFACE=/opt/socware/prefix-gcc/arm-none-eabi/include \
	 -DEXTERNAL_CMAKE_SYSTEM_PROCESSOR=arm \
	 -DEXTERNAL_MEM_HEAP_SIZE_KB=$(JERRYHEAP)
	make -C $(INTERM) $(TYPE).external
	cp `cat $(INTERM)/$(TYPE).external/list` $(OUTPUT)/.
	cp $(OUTPUT)/lib$(TYPE).jerry-core.a $(COPYTARGET)/libjerrycore.a
	cp $(OUTPUT)/lib$(TYPE).jerry-libm.lib.a $(COPYTARGET)/libjerrylibm.a
	cp $(INTERM)/lib$(TYPE).external-entry.a $(OUTPUT)/.
	cp $(OUTPUT)/lib$(TYPE).external-entry.a $(COPYTARGET)/libjerryentry.a
	mkdir -p $(PREFIX)/include/jerryscript
	cp jerry-core/*.h $(PREFIX)/include/jerryscript
clean:
	rm -rf $(INTERM)
	rm -rf $(OUTPUT)
