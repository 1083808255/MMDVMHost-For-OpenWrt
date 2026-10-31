include $(TOPDIR)/rules.mk

PKG_NAME:=dmr-gateway
PKG_RELEASE:=1
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/g4klx/DMRGateway.git
PKG_SOURCE_VERSION:=master
PKG_MIRROR_HASH:=skip
PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING

include $(INCLUDE_DIR)/package.mk

define Package/dmr-gateway
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Official DMR Gateway by G4KLX
  URL:=https://github.com/g4klx/DMRGateway
  DEPENDS:=+libstdcpp +libmosquitto +nlohmannjson +libcurl
endef

define Package/dmr-gateway/description
  Multi-network DMR Gateway.
endef

define Build/Compile
	# 强行打补丁：解决官方最新源码在新版编译器下找不到头文件的问题
	find $(PKG_BUILD_DIR) -name "*.cpp" -exec sed -i '1i #include <cstring>\n#include <cstdio>' {} +
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CXX="$(TARGET_CXX)" \
		CXXFLAGS="$(TARGET_CXXFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS)"
endef

define Package/dmr-gateway/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/DMRGateway $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_CONF) ./files/DMRGateway.ini $(1)/etc/DMRGateway.ini
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/dmr-gateway.init $(1)/etc/init.d/dmr-gateway
endef

$(eval $(call BuildPackage,dmr-gateway))
