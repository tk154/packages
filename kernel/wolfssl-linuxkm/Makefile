include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=wolfssl-linuxkm
PKG_VERSION:=5.9.2
PKG_REAL_VERSION:=$(PKG_VERSION)-stable
PKG_RELEASE:=1

PKG_SOURCE:=wolfssl-$(PKG_REAL_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/wolfSSL/wolfssl/archive/v$(PKG_REAL_VERSION)
PKG_HASH:=2f4ef3d4fd387a9b3191d36a6316d69116c46ff69bb9583b6c82b36d7b8ca114

PKG_BUILD_DIR:=$(KERNEL_BUILD_DIR)/$(PKG_NAME)/wolfssl-$(PKG_REAL_VERSION)

PKG_FIXUP:=libtool
PKG_LICENSE:=GPL-3.0-or-later
PKG_LICENSE_FILES:=LICENSING COPYING
PKG_MAINTAINER:=wolfSSL Inc. <info@wolfssl.com>
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16
PKG_EXTMOD_SUBDIRS:=linuxkm

include $(INCLUDE_DIR)/package.mk

DISABLE_NLS:=

define KernelPackage/libwolfssl
  SUBMENU:=Cryptographic API modules
  TITLE:=wolfSSL Linux kernel module
  URL:=https://www.wolfssl.com/
  FILES:=$(PKG_BUILD_DIR)/linuxkm/libwolfssl.ko
endef

define KernelPackage/libwolfssl/description
  libwolfssl.ko provides wolfCrypt facilities for kernel modules such as
  wolfGuard.
endef

CONFIGURE_ARGS += \
	--enable-reproducible-build \
	--enable-wolfguard \
	--enable-cryptonly \
	--enable-linuxkm \
	--with-linux-source="$(LINUX_DIR)" \
	--with-linux-arch="$(LINUX_KARCH)" \
	--prefix="$(PKG_BUILD_DIR)/linuxkm/build" \
	--disable-examples \
	--disable-jobserver

ifdef CONFIG_TARGET_x86_64
CONFIGURE_ARGS += --enable-intelasm
endif

define Build/Compile
	+$(MAKE) -C $(PKG_BUILD_DIR) $(PKG_JOBS) \
		$(MAKE_VARS) \
		KERNEL_ROOT="$(LINUX_DIR)" \
		KERNEL_ARCH="$(LINUX_KARCH)" \
		CROSS_COMPILE="CROSS_COMPILE=$(KERNEL_CROSS)" \
		CC="$(KERNEL_CC)" \
		CONFIG_DEBUG_INFO_BTF_MODULES= \
		module
endef

$(eval $(call KernelPackage,libwolfssl))
