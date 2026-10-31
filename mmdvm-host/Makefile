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

define Build/Compile
	# 强行打补丁：解决最新版 GCC 编译器找不到头文件的报错
	find $(PKG_BUILD_DIR) -name "*.cpp" -exec sed -i '1i #include <cstring>\n#include <cstdio>' {} +
	# 编译
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CXX="$(TARGET_CXX)" \
		CXXFLAGS="$(TARGET_CXXFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS)"
endef

define Package/mmdvm-host/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/MMDVMHost $(1)/usr/bin/
	
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_CONF) ./files/MMDVM.ini $(1)/etc/MMDVM.ini
	
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/mmdvm.init $(1)/etc/init.d/mmdvmhost
endef

$(eval $(call BuildPackage,mmdvm-host))
