CELL_MK_DIR = $(CELL_SDK)/samples/mk
include $(CELL_MK_DIR)/sdk.makedef.mk

BUILD_TYPE     	= release

LIBSTUB_DIR		= ../lib
PRX_DIR		= .
INSTALL			= cp
PEXPORTPICKUP		= ppu-lv2-prx-exportpickup
PRX_LDFLAGS_EXTRA	= -L ../lib -Wl,--strip-unused-data

CRT_HEAD                += $(shell ppu-lv2-gcc -print-file-name'='ecrti.o)
CRT_HEAD                += $(shell ppu-lv2-gcc -print-file-name'='crtbegin.o)
CRT_TAIL                += $(shell ppu-lv2-gcc -print-file-name'='crtend.o)
CRT_HEAD                += $(shell ppu-lv2-gcc -print-file-name'='ecrtn.o)

PPU_SRCS = main.c printf.c libc.c asmcode.s
PPU_PRX_TARGET = rawseciso.prx
PPU_PRX_LDFLAGS += $(PRX_LDFLAGS_EXTRA)
PPU_PRX_STRIP_FLAGS = -s
PPU_PRX_LDLIBS 	+= -lfs_stub
PPU_CFLAGS += -Os -ffunction-sections -fdata-sections -fno-builtin-printf -nodefaultlibs -std=gnu99 -Wno-shadow -Wno-unused-parameter -Werror -Wno-inline

ifeq ($(BUILD_TYPE), debug)
PPU_CFLAGS += -DDEBUG
endif

CLEANFILES = $(PRX_DIR)/$(PPU_SPRX_TARGET)

SCETOOL_FLAGS := --sce-type=SELF --compress-data=TRUE --key-revision=0004 --self-auth-id=1070000052000001 
SCETOOL_FLAGS += --self-vendor-id=01000002 --self-type=APP --self-app-version=0003004000000000 
SCETOOL_FLAGS += --self-ctrl-flags=4000000000000000000000000000000000000000000000000000000000000000 
SCETOOL_FLAGS += --self-cap-flags=00000000000000000000000000000000000000000000007B0000000100020000 

all:
	$(MAKE) $(PPU_OBJS_DEPENDS)
	$(PPU_PRX_STRIP) --strip-debug --strip-section-header $(PPU_PRX_TARGET)
	scetool $(SCETOOL_FLAGS) --encrypt $(PPU_PRX_TARGET) $(PPU_SPRX_TARGET)
	
include $(CELL_MK_DIR)/sdk.target.mk

clean:
	rm -f objs *.prx *.sym *.sprx 


