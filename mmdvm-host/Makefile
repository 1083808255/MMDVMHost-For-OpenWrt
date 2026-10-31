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
  # 加回 MQTT 依赖
  DEPENDS:=+libstdcpp +libpthread +nlohmannjson +libmosquitto
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	-$(SED) 's/if (getuid() == 0)/if (false)/g' $(PKG_BUILD_DIR)/MMDVM-Host.cpp
	-$(SED) 's/if (getuid() == 0)/if (false)/g' $(PKG_BUILD_DIR)/MMDVMHost.cpp
endef

define Build/Compile
	# 加上 mosquitto 的头文件路径和链接库
	$(TARGET_CXX) $(TARGET_CXXFLAGS) -I$(STAGING_DIR)/usr/include -I$(STAGING_DIR)/usr/include/mosquitto -D_OPENWRT -std=c++11 -O2 -o $(PKG_BUILD_DIR)/MMDVMHost $(PKG_BUILD_DIR)/*.cpp -lpthread -lmosquitto
endef

define Package/mmdvm-host/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/MMDVMHost $(1)/usr/bin/
endef

$(eval $(call BuildPackage,mmdvm-host))
