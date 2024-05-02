#
# Copyright (C) 2024 Til Kaiser
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libibmad
PKG_VERSION:=1.3.13
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://downloads.openfabrics.org/management
PKG_SOURCE_DATE:=2017-03-08
PKG_HASH:=17cdd721c81fecefc366601c46c55a4d44c93799980a0a34c271b12bc544520b

PKG_MAINTAINER:=Til Kaiser <mail@tk154.de>
PKG_LICENSE:=GPL-2.0-only
PKG_LICENSE_FILES:=COPYING

PKG_FIXUP:=autoreconf
PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libibmad
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Library to encode, decode, and dump IB MAD packets
  DEPENDS:=+libibumad
endef

define Package/libibmad/description
  libibmad provides low layer Infiniband functions for use by
  the InfiniBand diagnostic and management programs. These include
  Management Datagrams (MAD), Subnet Administration (SA),
  Subnet Management Packets (SMP) and other basic functions.
endef

CONFIGURE_ARGS += \
  --includedir=$(STAGING_DIR)/usr/include

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/infiniband
	$(CP) $(PKG_INSTALL_DIR)/$(STAGING_DIR)/usr/include/infiniband $(1)/usr/include/

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libibmad.* $(1)/usr/lib/
endef

Package/libibmad/install = $(Build/InstallDev)

$(eval $(call BuildPackage,libibmad))
