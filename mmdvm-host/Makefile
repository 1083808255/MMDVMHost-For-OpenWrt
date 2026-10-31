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
  DEPENDS:=+libstdcpp +libpthread +nlohmannjson
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	# 尝试修改源码，跳过 root 降权报错（带横杠和不带横杠都试一遍，加 - 防止找不到文件报错）
	-$(SED) 's/if (getuid() == 0)/if (false)/g' $(PKG_BUILD_DIR)/MMDVM-Host.cpp
	-$(SED) 's/if (getuid() == 0)/if (false)/g' $(PKG_BUILD_DIR)/MMDVMHost.cpp
endef

define Build/Compile
	# 直接调用交叉编译器，强制包含 nlohmannjson 的头文件路径，规避官方 Makefile 问题
	$(TARGET_CXX) $(TARGET_CXXFLAGS) -I$(STAGING_DIR)/usr/include -D_OPENWRT -std=c++11 -O2 -o $(PKG_BUILD_DIR)/MMDVMHost $(PKG_BUILD_DIR)/*.cpp -lpthread
endef

define Package/mmdvm-host/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/MMDVMHost $(1)/usr/bin/
endef

$(eval $(call BuildPackage,mmdvm-host))
