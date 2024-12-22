include $(TOPDIR)/rules.mk

#内核编译相关mk文件
include $(INCLUDE_DIR)/kernel.mk

#组件版本信息
PKG_VERSION:=1.0.0
PKG_RELEASE:=$(SVN_VER)

#组件包名称
PKG_NAME:=qt_elf
#进程名前缀
PROC_NAME:=$(PKG_NAME)

#组件编译目录名称
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

#软件包编译头文件包含
include $(INCLUDE_DIR)/package.mk

#组件包定义，包括菜单CATEGORY/SUBMENU/TITLE，依赖DEPENDS
define Package/$(PKG_NAME)
	CATEGORY:=Custom modules
#	SUBMENU:=Gateway modules
	TITLE:=this is a qt elf
	DEPENDS:=+libstdcpp +qt5-core +qt5-concurrent +qt5-network +qt5-widgets +qt5-sql +qt5-xml +qt5-serialport +qt5-serialbus +qt5-xmlpatterns +qt5-test
endef

#编译准备：拷贝代码到编译目录
define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) -R ./src/* $(PKG_BUILD_DIR)
endef

#外部依赖组件的处理
# BUILDFLAGS为内核编译选项
# TARGET_CFLAGS为用户空间编译选项
# TARGET_LDFLAGS为用户空间链接选项
# BUILDFLAGS +=
# TARGET_CFLAGS +=
# TARGET_LDFLAGS +=

#组件编译
define Build/Compile
	echo "building"
#方式1：用户空间src目录编译
#	$(call Build/Compile/Default)
#方式2：用户空间src目录编译
#	$(MAKE) -C $(PKG_BUILD_DIR) \
#		$(TARGET_CONFIGURE_OPTS) \
#		EXTRA_CFLAGS="$(TARGET_CFLAGS)" \
#		EXTRA_LDFLAGS="$(TARGET_LDFLAGS)" \
#		all
#内核空间编译
#	$(MAKE) -C "$(LINUX_DIR)" \
#		CROSS_COMPILE="$(TARGET_CROSS)" \
#		ARCH="$(LINUX_KARCH)" \
#		M="$(PKG_BUILD_DIR)/kernel" \
#		SUBDIRS="$(PKG_BUILD_DIR)/kernel" \
#		EXTRA_CFLAGS="-g $(BUILDFLAGS) " \
#		modules
endef

#编译结果安装到编译环境，为其他组件提供头文件和库，$(1)的路径为：staging_dir/target-xxxx/
define Build/InstallDev
#	$(INSTALL_DIR) $(1)/usr/include/demo
#	$(CP) $(PKG_BUILD_DIR)/include/demo.h $(1)/usr/include/demo/

#	$(INSTALL_DIR) $(1)/usr/lib/
#	$(CP) $(PKG_BUILD_DIR)/lib/libdemo.so.1.0.0 $(1)/usr/lib/
#	cd $(1)/usr/lib && ln -sf libdemo.so.1.0.0 libdemo.so
endef

#编译结果安装到目标设备
define Package/$(PKG_NAME)/install
#sbin and shell
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/shell/myqttest.elf $(1)/usr/sbin/

#shell for start
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/shell/$(PROC_NAME).init $(1)/etc/init.d/$(PROC_NAME)
endef

#组件包执行
$(eval $(call BuildPackage,$(PKG_NAME)))
