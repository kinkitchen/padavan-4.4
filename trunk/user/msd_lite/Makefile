include $(ROOTDIR)/rules.mk

PKG_NAME:=msd_lite
PKG_SOURCE_DATE:=2025-05-02
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/rozhuk-im/msd_lite
PKG_SOURCE_VERSION:=79a6c62c8fced6128a5e445ee110709b3b51bb78
PKG_FIXUP:=autoreconf

INCLUDE_DIR := $(ROOTDIR)/include

include $(INCLUDE_DIR)/package.mk

# 强制声明系统提供这些函数，避免 fallback 重复定义
EXTRA_FLAGS = -D_GNU_SOURCE \
              -DHAVE_EXPLICIT_BZERO=1 \
              -DHAVE_MEMRCHR=1 \
              -DHAVE_MEMMEM=1 \
              -DHAVE_REALLOCARRAY=1 \
              -DHAVE_STRLCPY=1 \
              -DHAVE_PIPE2=1 \
              -DHAVE_ACCEPT4=1

define Build/Configure
	mkdir -p $(PKG_BUILD_DIR)/build
	(cd $(PKG_BUILD_DIR)/build; \
		CFLAGS="$(TARGET_CFLAGS) $(EXTRA_CFLAGS) $(EXTRA_FLAGS)" \
		CXXFLAGS="$(TARGET_CXXFLAGS) $(EXTRA_CXXFLAGS) $(EXTRA_FLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS) $(EXTRA_LDFLAGS) -lpthread" \
		cmake \
			-DCMAKE_C_COMPILER=$(TOOLCHAIN_DIR)/bin/mipsel-linux-gcc \
			-DCMAKE_CXX_COMPILER=$(TOOLCHAIN_DIR)/bin/mipsel-linux-g++ \
			-DCMAKE_SYSTEM_NAME=Linux \
			-DCMAKE_BUILD_TYPE=RELEASE \
			-DPTHREAD_LIBRARY=-lpthread \
			$(PKG_BUILD_DIR) \
	)
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/build
endef

define Build/Install
endef

$(eval $(call BuildPackage,msd_lite))

romfs:
	$(INSTALL_DIR) $(ROMFSDIR)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/msd_lite $(ROMFSDIR)/usr/bin/msd_lite
	$(INSTALL_DIR) $(ROMFSDIR)/etc_ro
	$(INSTALL_CONF) ./msd_lite.conf $(ROMFSDIR)/etc_ro/msd_lite.conf