# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=tuic-server
PKG_VERSION:=1.0.0
PKG_RELEASE:=1

#PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
#PKG_SOURCE_URL:=https://codeload.github.com/EAimTY/tuic/tar.gz/tuic-server-$(PKG_VERSION)?
#PKG_HASH:=fa333037596b2ce447d3d99c48f002c0c9f8da5e0725a4c15b2c30b14a298532
#PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)/$(PKG_NAME)

PKG_SOURCE_PROTO:=git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/EAimTY/tuic.git
PKG_SOURCE_VERSION:=0acf4844c46e63b83eab3e58069fe8c060509974
PKG_MIRROR_HASH:=93407cc89a8aa94162c8b04b475750473f68beac3550fb26efb83910b7afdf84
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)/$(PKG_NAME)

PKG_MAINTAINER:=Tianling Shen <cnsztl@immortalwrt.org>
PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=rust/host
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/rust/rust-package.mk

TAR_OPTIONS+= --strip-components 1
TAR_CMD=$(HOST_TAR) -C $(1)/.. $(TAR_OPTIONS)

define Package/tuic-server
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=Delicately-TUICed 0-RTT proxy protocol (server)
  URL:=https://github.com/EAimTY/tuic
  DEPENDS:=@(aarch64||arm||x86_64)
endef

$(eval $(call RustBinPackage,tuic-server))
$(eval $(call BuildPackage,tuic-server))
