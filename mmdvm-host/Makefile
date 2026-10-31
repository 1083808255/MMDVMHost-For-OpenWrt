include $(TOPDIR)/rules.mk

# 1. 定义包名、版本、源码来源
PKG_NAME:=mmdvm-host
PKG_RELEASE:=1
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/g4klx/MMDVMHost.git
PKG_SOURCE_VERSION:=master   # 建议换成具体 commit hash
PKG_MIRROR_HASH:=skip
PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING

# 2. 引入 OpenWrt 的编译框架
include $(INCLUDE_DIR)/package.mk

# 3. 定义包的依赖和显示信息
define Package/mmdvm-host
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=MMDVM Host (Official G4KLX)
  URL:=https://github.com/g4klx/MMDVMHost
  # 声明依赖库（关键）
  DEPENDS:=+libstdcpp 
endef

define Package/mmdvm-host/description
  Official MMDVM Host by G4KLX.
endef

# 4. 定义交叉编译方法
define Build/Compile
	# 调用原生 Makefile，但强制使用 OpenWrt 的交叉编译器
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CXX="$(TARGET_CXX)" \
		CXXFLAGS="$(TARGET_CXXFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS)"
endef

# 5. 定义安装规则：把编译好的文件放到 .ipk 的对应目录里
define Package/mmdvm-host/install
	# 安装主程序到 /usr/bin
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/MMDVMHost $(1)/usr/bin/
	
	# 安装配置文件到 /etc
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_CONF) ./files/MMDVM.ini $(1)/etc/MMDVM.ini
	
	# 安装开机启动脚本到 /etc/init.d
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/mmdvm.init $(1)/etc/init.d/mmdvmhost
endef

$(eval $(call BuildPackage,mmdvm-host))
