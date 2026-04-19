include $(INCLUDE_DIR)/rules.mk

PKG_NAME:=msd_lite
PKG_SOURCE_DATE:=2025-05-02

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/rozhuk-im/msd_lite
PKG_SOURCE_VERSION:=79a6c62c8fced6128a5e445ee110709b3b51bb78

PKG_FIXUP:=autoreconf

# 定义 INCLUDE_DIR，解决 /autotools.mk 找不到的问题
INCLUDE_DIR := $(ROOTDIR)/include

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

$(eval $(call BuildPackage,msd_lite))

romfs:
	$(INSTALL_DIR) $(ROMFSDIR)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/msd_lite $(ROMFSDIR)/usr/bin/msd_lite
	$(INSTALL_DIR) $(ROMFSDIR)/etc_ro
	$(INSTALL_CONF) ./msd_lite.conf $(ROMFSDIR)/etc_ro/msd_lite.conf