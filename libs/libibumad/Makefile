#
# Copyright (C) 2024 Til Kaiser
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libibumad
PKG_VERSION:=1.3.10.2
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://downloads.openfabrics.org/management
PKG_SOURCE_DATE:=2015-03-03
PKG_HASH:=500f74e31bd9f3d6c32dea912de3a2d1b299f3f23d7abd893652dd45c77a7e2e

PKG_MAINTAINER:=Til Kaiser <mail@tk154.de>
PKG_LICENSE:=GPL-2.0-only
PKG_LICENSE_FILES:=COPYING

PKG_FIXUP:=autoreconf
PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libibumad
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=libibumad
endef

define Package/libibumad/description
  libibumad provides userspace Infiniband Management Datagram (uMAD)
  functions which sit on top of the uMAD modules in the kernel.
  These are used by InfiniBand diagnostic and management tools.
endef

CONFIGURE_ARGS += \
  --includedir=$(STAGING_DIR)/usr/include

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/infiniband
	$(CP) $(PKG_INSTALL_DIR)/$(STAGING_DIR)/usr/include/infiniband $(1)/usr/include/

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libibumad.* $(1)/usr/lib/
endef

Package/libibumad/install = $(Build/InstallDev)

$(eval $(call BuildPackage,libibumad))
