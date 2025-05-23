# SPDX-License-Identifier: GPL-3.0-only
#
# Copyright (C) 2021 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=dnscrypt-proxy2-full
PKG_VERSION:=2.1.5
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/DNSCrypt/dnscrypt-proxy.git
PKG_SOURCE_VERSION:=7447fc4a0e7e85fa928628905bab98d9a13254d7
PKG_MIRROR_HASH:=143eaf644a2b693751b12068816478ed7ea9a90f54515b33a9ff895205c7f794

PKG_MAINTAINER:=Josef Schlehofer <josef.schlehofer@nic.cz>
PKG_LICENSE:=ISC
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0
PKG_BUILD_FLAGS:=no-mips16

PKG_CONFIG_DEPENDS:= \
	CONFIG_DNSCRYPT-PROXY_COMPRESS_GOPROXY \
	CONFIG_DNSCRYPT-PROXY_COMPRESS_UPX

GO_PKG:=https://github.com/DNSCrypt/dnscrypt-proxy
GO_PKG_LDFLAGS:=-s -w

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

GO_MOD_ARGS:=
GO_PKG_BUILD_VARS+= GO111MODULE=off

define Package/dnscrypt-proxy2-full
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Flexible DNS proxy with encrypted DNS protocols
  SUBMENU:=IP Addresses and Names
  DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle
  URL:=https://github.com/DNSCrypt/dnscrypt-proxy
endef

define Package/dnscrypt-proxy2-full/description
  A flexible DNS proxy, with support for modern encrypted DNS protocols such as DNSCrypt v2, DNS-over-HTTPS, Anonymized DNSCrypt and ODoH (Oblivious DoH).
endef

define Package/dnscrypt-proxy2-full/config
	config DNSCRYPT-PROXY_COMPRESS_GOPROXY
		bool "Compiling with GOPROXY proxy"
		default n

	config DNSCRYPT-PROXY_COMPRESS_UPX
		bool "Compress executable files with UPX"
		depends on !mips64
		default n
endef

ifeq ($(CONFIG_BROOK_COMPRESS_GOPROXY),y)
	export GO111MODULE=on
	export GOPROXY=https://goproxy.baidu.com
endif

define Build/Compile
	$(call GoPackage/Build/Compile)
ifeq ($(CONFIG_DNSCRYPT-PROXY_COMPRESS_UPX),y)
	$(STAGING_DIR_HOST)/bin/upx --lzma --best $(GO_PKG_BUILD_BIN_DIR)/*
endif
endef

define Package/dnscrypt-proxy2-full/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/sbin/
endef

$(eval $(call GoBinPackage,dnscrypt-proxy2-full))
$(eval $(call BuildPackage,dnscrypt-proxy2-full))
