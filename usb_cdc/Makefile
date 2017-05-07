#
# Simple makefile to compile the STM32Cube example code given for the STM32F0xx
# family of ARM processors. This file should be placed in the example/demo
# directory that you wish to compile.
#
# INCLUDEDIRS selects all folders in the package that have header files. Since
#             this is already an inclusive list, it should not need editing.
#
# LIBSOURCES contains all the Driver (HAL/BSP) files that need to be compiled
#            for this specific project. This should be updated with the help
#            of the dependencies file (main.d) generated on any (including 
#            unsucessful) compilation.
#
# Note: The provided code sometimes uses windows backslashes (\) in include
#       paths. Update as needed.
#
# Note: When building the lists for LIBSOURCES, *_ex.c files must come before
#       the generic version of the file. This is necessary for linking to find
#       the extended (board-specific) version of the function.
#
# Example: These sources are meant for the CORTEXM_SysTick example. The file
#          should be saved at STM32Cube_FW_F0_V1.1.0/Projects/
#          STM32F072B-Discovery/Examples/Cortex/CORTEXM_SysTick/Makefile.
#
# Author: Ian Hartwig (ihartwig)
#
# Derived from szczys's makefile in 
# www.github.com/szczys/stm32f0-discovery-basic-template/
#

# build environment
CC = arm-none-eabi-gcc
CPP = arm-none-eabi-g++
AR = arm-none-eabi-ar
AS = arm-none-eabi-as
OBJCOPY = arm-none-eabi-objcopy
OBJDUMP = arm-none-eabi-objdump
SIZE = arm-none-eabi-size
MAKE = make

# project parameters
PROJ_NAME = usb_cdc
CPU_FAMILY = STM32F1xx
CPU_MODEL_GENERAL = STM32F103xB
CPU_MODEL_SPECIFIC = STM32F103C8

INCLUDEDIRS =
INCLUDEDIRS += Inc
INCLUDEDIRS += Drivers/STM32F1xx_HAL_Driver/Inc
INCLUDEDIRS += Drivers/CMSIS/Include
INCLUDEDIRS += Drivers/CMSIS/Device/ST/STM32F1xx/Include
INCLUDEDIRS += Middlewares/ST/STM32_USB_Device_Library/Class/CDC/Inc
INCLUDEDIRS += Middlewares/ST/STM32_USB_Device_Library/Core/Inc

# -lc      : link to standard C library ( stdio.h and stdlib.h)
# -lg      : a debugging-enabled libc
# -lm      : link to math C library ( math.h )
# -lnosys  : non-semihosting
#            link to libnosys.a - The libnosys.a is used to satisfy all system call references,
#            although with empty calls. In this configuration, the debug output
#            is forwarded to the semihosting debug channel, vis SYS_WRITEC.
#            The application and redefine all syscall implementation functions, like _write(), 
#            _read(), etc. When using libnosys.a, the startup files are not needed
#            Since 4.8, recommended -specs=nosys.specs instead
# -lrdimon : semihosting
#            link to librdimon.a - implements all system calls via the semihosting API with all 
#            functionality provided by the host. When using librdimon.a, the startup files are
#            required to provide all specific initialisation, and the rdimon.specs
#            must be added to the linker
#            Since 4.8, recommended -specs=rdimon.specs instead
LIBSOURCES = 
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal.c
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_cortex.c
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_dma.c
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_flash.c
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_flash_ex.c
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_gpio.c
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_gpio_ex.c
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_pcd.c
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_pcd_ex.c
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_pwr.c
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_rcc.c
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_rcc_ex.c
LIBSOURCES += Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_ll_usb.c
# usb cdc driver
LIBSOURCES += Middlewares/ST/STM32_USB_Device_Library/Class/CDC/Src/usbd_cdc.c
LIBSOURCES += Middlewares/ST/STM32_USB_Device_Library/Core/Src/usbd_core.c
LIBSOURCES += Middlewares/ST/STM32_USB_Device_Library/Core/Src/usbd_ctlreq.c
LIBSOURCES += Middlewares/ST/STM32_USB_Device_Library/Core/Src/usbd_ioreq.c
LIBOBJS = $(LIBSOURCES:.c=.o)

################################################################################
# auto-generated project paths
#SOURCES = main.c gpio.c stm32f1xx_hal_msp.c stm32f1xx_it.c
SOURCES += $(shell find Src -name '*.c')
SOURCES += Src/system_stm32f1xx.c
SOURCES += startup/startup_stm32f103xb.s
OBJ1 = $(SOURCES:.c=.o)
OBJ = $(OBJ1:.s=.o)

LDSCRIPT = -Wl,-T,STM32F103C8_FLASH.ld

CFLAGS = -Wall -g -O0 -D $(CPU_MODEL_GENERAL)
CFLAGS += -mlittle-endian -mcpu=cortex-m3 -march=armv7-m -mthumb -DUSE_HAL_DRIVER -DSTM32F10X_LD -DHSE_VALUE=8000000
CFLAGS += -ffunction-sections -fdata-sections
CFLAGS += -Wl,--gc-sections -Wl,-Map=$(PROJ_NAME).map 
CFLAGS +=  $(addprefix -I ,$(INCLUDEDIRS))



.PHONY: all

all: proj

proj: libstm32f1.a $(PROJ_NAME).elf

libstm32f1.a: $(LIBOBJS)
	@echo
	$(AR) -r $@ $(LIBOBJS)

%.o: %.c
	@echo
	$(CC) -c -o $@ $< $(CFLAGS)

$(PROJ_NAME).elf: $(SOURCES)
	@echo
	$(CC) $(CFLAGS) $^ -o $@ $(LIBOBJS) $(LDSCRIPT) --specs=nano.specs --specs=nosys.specs -lc -lm -LD
	rm $(LIBOBJS) || true
	$(OBJCOPY) -O ihex $(PROJ_NAME).elf $(PROJ_NAME).hex
	$(OBJCOPY) -O binary $(PROJ_NAME).elf $(PROJ_NAME).bin
	$(OBJDUMP) -St $(PROJ_NAME).elf >$(PROJ_NAME).lst
	$(SIZE) $(PROJ_NAME).elf

#program to device using ST-link V2
flash: 
	st-flash write $(PROJ_NAME).bin 0x8000000

clean:
	rm -f libstm32f1.a     || true
	rm -f $(PROJ_NAME).d   || true
	rm -f $(PROJ_NAME).elf || true 
	rm -f $(PROJ_NAME).hex || true
	rm -f $(PROJ_NAME).bin || true
	rm -f $(PROJ_NAME).map || true
	rm -f $(PROJ_NAME).lst || true
	rm -f $(PROJ_NAME).ioc || true 
	rm -f $(PROJ_NAME).elf.launch || true 

