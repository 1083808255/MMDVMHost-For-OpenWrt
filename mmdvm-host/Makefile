include $(TOPDIR)/rules.mk

PKG_NAME:=mmdvm-host
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/g4klx/MMDVMHost.git
PKG_SOURCE_VERSION:=master
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING

include $(INCLUDE_DIR)/package.mk

define Package/mmdvm-host
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Official MMDVM Host by G4KLX
  URL:=https://github.com/g4klx/MMDVMHost
  DEPENDS:=+libstdcpp
endef

define Package/mmdvm-host/description
  MMDVM Host connects modems to digital voice networks.
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	# 如果你用方法A，可以在这里用 sed 注入头文件
	# 如果你用方法B，OpenWrt 会自动把 patches/ 文件夹下的补丁打上，这里什么都不用写
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CXX="$(TARGET_CXX)" \
		CXXFLAGS="$(TARGET_CXXFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS)"
endef

define Package/mmdvm-host/install
	# 1. 安装主程序
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/MMDVMHost $(1)/usr/bin/
	
	# 2. 安装默认配置文件
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_CONF) ./files/MMDVM.ini $(1)/etc/MMDVM.ini
	
	# 3. 安装 OpenWrt 启动脚本
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/mmdvm.init $(1)/etc/init.d/mmdvmhost
endef

$(eval $(call BuildPackage,mmdvm-host))
