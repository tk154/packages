#
# Copyright (C) 2024 Til Kaiser
#

include $(TOPDIR)/rules.mk

PKG_NAME:=hw-mgmt
PKG_VERSION:=V.7.0040.1007
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/Mellanox/$(PKG_NAME)/tar.gz/$(PKG_VERSION)?
PKG_SOURCE_DATE:=2024-07-30
PKG_HASH:=397bdbf3ea1dee93310f4bbaca776b28bbe14c23e0d1a522bfa35dcf6417df2e

PKG_MAINTAINER:=Til Kaiser <mail@tk154.de>
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/hw-mgmt
  SECTION:=Utilities
  CATEGORY:=Utilities
  TITLE:=Mellanox Hardware Management package
  URL:=https://github.com/Mellanox/hw-mgmt
  DEPENDS:=+bash +lm-sensors +pciutils \
  	+coreutils +coreutils-sleep +coreutils-timeout
endef

define Package/hw-mgmt/description
  This package supports thermal control and hardware
  management for Mellanox switches by using the sysfs interface.
endef

Build/Compile:=
Build/Install:=

define Package/hw-mgmt/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/usr/usr/bin/hw-management.sh $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/usr/usr/bin/hw-management-devtree.sh $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/usr/usr/bin/hw-management-helpers.sh $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/usr/usr/bin/hw-management-i2c-gpio-expander.sh $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/usr/usr/bin/hw-management-start-post.sh $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/hw-mgmt.init $(1)/etc/init.d/hw-mgmt

	$(INSTALL_DIR) $(1)/etc/hw-management-sensors
	$(CP) $(PKG_BUILD_DIR)/usr/etc/hw-management-sensors $(1)/etc/
endef

$(eval $(call BuildPackage,hw-mgmt))
