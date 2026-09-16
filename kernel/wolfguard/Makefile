include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=wolfguard
PKG_VERSION:=2026.09.09
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/wolfSSL/wolfGuard.git
PKG_SOURCE_VERSION:=26fcf8156398aeddf825aaf70b1ff8bbd39ec206
PKG_MIRROR_HASH:=35d0b7c3408cff324a8416743b605c67f2b47bb05ab5f9de1739e97e1dff2b9f

PKG_BUILD_DEPENDS:=wolfssl-linuxkm
PKG_FILE_DEPENDS:=$(LINUX_DIR)/.config
PKG_BUILD_PARALLEL:=1
PKG_EXTMOD_SUBDIRS:=kernel-src
PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING
PKG_MAINTAINER:=wolfSSL Inc. <info@wolfssl.com>

WOLFSSL_LINUXKM_DIR:=$(KERNEL_BUILD_DIR)/wolfssl-linuxkm/wolfssl-5.9.2-stable

include $(INCLUDE_DIR)/package.mk

define KernelPackage/wolfguard
  SUBMENU:=Network Support
  TITLE:=WolfGuard secure network tunnel
  URL:=https://github.com/wolfSSL/wolfGuard
  DEPENDS:=+kmod-libwolfssl +kmod-udptunnel4 +IPV6:kmod-udptunnel6
  FILES:=$(PKG_BUILD_DIR)/kernel-src/wolfguard.ko
  AUTOLOAD:=$(call AutoProbe,wolfguard)
endef

define KernelPackage/wolfguard/description
  WolfGuard is wolfSSL's FIPS-oriented refactor of kernel WireGuard. It uses
  its own wolfguard link type and generic netlink family and is not
  interoperable with WireGuard peers.
endef

define Package/wolfguard-tools
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  TITLE:=WolfGuard userspace control program and netifd proto
  URL:=https://github.com/wolfSSL/wolfGuard
  DEPENDS:=+@BUSYBOX_CONFIG_IP +@BUSYBOX_CONFIG_FEATURE_IP_LINK +kmod-wolfguard
endef

define Package/wolfguard-tools/description
  This package provides wg-fips and a dedicated OpenWrt netifd proto helper
  named wolfguard.
endef

define Build/Configure
endef

define Build/Compile
	+$(KERNEL_MAKE) $(PKG_JOBS) \
		M="$(PKG_BUILD_DIR)/kernel-src" \
		WOLFSSL_ROOT="$(WOLFSSL_LINUXKM_DIR)" \
		WOLFSSL_INCLUDE="$(WOLFSSL_LINUXKM_DIR)" \
		WOLFGUARD_VERSION="$(PKG_VERSION)" \
		modules
	+$(MAKE) -C $(PKG_BUILD_DIR)/user-src $(PKG_JOBS) \
		$(TARGET_CONFIGURE_OPTS) \
		EXTRA_CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS)" \
		EXTRA_LDFLAGS="$(TARGET_LDFLAGS)" \
		RUNSTATEDIR=/var/run \
		PLATFORM=linux \
		WITH_BASHCOMPLETION=no \
		WITH_SYSTEMDUNITS=no \
		WITH_WGQUICK=no \
		wg-fips
endef

define Package/wolfguard-tools/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/user-src/wg-fips $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/lib/netifd/proto
	$(INSTALL_DATA) ./files/wolfguard.uc $(1)/lib/netifd/proto/
endef

$(eval $(call KernelPackage,wolfguard))
$(eval $(call BuildPackage,wolfguard-tools))
