include $(TOPDIR)/rules.mk

PKG_NAME:=mmdvm-host
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/g4klx/MMDVMHost.git
PKG_SOURCE_VERSION:=master
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/mmdvm-host
  SECTION:=net
  CATEGORY:=Network
  TITLE:=MMDVM Host for OpenWrt
  DEPENDS:=+libstdcpp +libpthread
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	# 直接修改源码文件，解决 root 降权报错问题 (替代 patch)
	$(SED) 's/if (getuid() == 0)/if (false)/g' $(PKG_BUILD_DIR)/MMDVM-Host.cpp
endef

define Build/Compile
	# 手动指定编译命令，绕过官方不兼容的 Makefile
	$(TARGET_CXX) $(TARGET_CXXFLAGS) -D_OPENWRT -std=c++11 -O2 -o $(PKG_BUILD_DIR)/MMDVMHost $(PKG_BUILD_DIR)/*.cpp -lpthread
endef

define Package/mmdvm-host/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/MMDVMHost $(1)/usr/bin/
endef

$(eval $(call BuildPackage,mmdvm-host))
