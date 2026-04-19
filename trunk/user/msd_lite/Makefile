include $(ROOTDIR)/rules.mk

PKG_NAME:=msd_lite
PKG_SOURCE_DATE:=2025-05-02
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/rozhuk-im/msd_lite
PKG_SOURCE_VERSION:=79a6c62c8fced6128a5e445ee110709b3b51bb78
PKG_FIXUP:=autoreconf

INCLUDE_DIR := $(ROOTDIR)/include

include $(INCLUDE_DIR)/package.mk

define Build/Configure
	# 1. 强制定义 _GNU_SOURCE 以启用系统函数声明
	# 2. 注释掉 os.h 中与系统冲突的静态 fallback 函数
	if [ -f $(PKG_BUILD_DIR)/src/liblcb/include/al/os.h ]; then \
		sed -i 's/^static void explicit_bzero(/\/\/ static void explicit_bzero(/' $(PKG_BUILD_DIR)/src/liblcb/include/al/os.h; \
		sed -i 's/^static void \*memrchr(/\/\/ static void *memrchr(/' $(PKG_BUILD_DIR)/src/liblcb/include/al/os.h; \
		sed -i 's/^static void \*memmem(/\/\/ static void *memmem(/' $(PKG_BUILD_DIR)/src/liblcb/include/al/os.h; \
		sed -i 's/^static void \*reallocarray(/\/\/ static void *reallocarray(/' $(PKG_BUILD_DIR)/src/liblcb/include/al/os.h; \
		sed -i 's/^static size_t strlcpy(/\/\/ static size_t strlcpy(/' $(PKG_BUILD_DIR)/src/liblcb/include/al/os.h; \
		sed -i 's/^static int pipe2(/\/\/ static int pipe2(/' $(PKG_BUILD_DIR)/src/liblcb/include/al/os.h; \
		sed -i 's/^static int accept4(/\/\/ static int accept4(/' $(PKG_BUILD_DIR)/src/liblcb/include/al/os.h; \
	fi
	mkdir -p $(PKG_BUILD_DIR)/build
	(cd $(PKG_BUILD_DIR)/build; \
		CFLAGS="$(TARGET_CFLAGS) $(EXTRA_CFLAGS) -D_GNU_SOURCE" \
		CXXFLAGS="$(TARGET_CXXFLAGS) $(EXTRA_CXXFLAGS) -D_GNU_SOURCE" \
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