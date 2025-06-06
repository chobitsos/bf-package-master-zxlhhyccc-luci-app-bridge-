# SPDX-License-Identifier: GPL-3.0-only
#
# Copyright (C) 2021 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=webdav-go
PKG_VERSION:=4.2.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/hacdias/webdav/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=bc8e11cf765e5506677968ccdf815e0648d93606dc3c5e8f5f2f76fe0407ee2c
PKG_BUILD_DIR:=$(BUILD_DIR)/webdav-$(PKG_VERSION)

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=ElonH <elonhhuang@gmail.com>

PKG_CONFIG_DEPENDS:= \
	CONFIG_WEBDAV_GO_COMPRESS_GOPROXY \
	CONFIG_WEBDAV_GO_COMPRESS_UPX

# PKG_BUILD_DIR:=$(BUILD_DIR)/$(firstword $(subst -, ,$(PKG_NAME)))-$(PKG_VERSION)
PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
# PKG_USE_MIPS16:=0
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/hacdias/webdav
GO_PKG_LDFLAGS:=-s -w

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/webdav-go
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=File Transfer
  TITLE:=Simple Go WebDAV server
  URL:=https://github.com/hacdias/webdav
  DEPENDS:=$(GO_ARCH_DEPENDS)
endef

define Package/webdav-go/description
  WebDAV command line interface is really easy to use so you can easily
  create a WebDAV server for your own user. By default, it runs on a
  random free port and supports JSON, YAML and TOML configuration.
endef

define Package/webdav-go/config
config WEBDAV_GO_COMPRESS_GOPROXY
	bool "Compiling with GOPROXY proxy"
	default n

config WEBDAV_GO_COMPRESS_UPX
	bool "Compress executable files with UPX"
	depends on !mips64
	default n
endef

ifeq ($(CONFIG_WEBDAV_GO_COMPRESS_GOPROXY),y)
	export GO111MODULE=on
	export GOPROXY=https://goproxy.baidu.com
endif

define Build/Compile
	$(call GoPackage/Build/Compile)
ifeq ($(CONFIG_WEBDAV_GO_COMPRESS_UPX),y)
	$(STAGING_DIR_HOST)/bin/upx --lzma --best $(GO_PKG_BUILD_BIN_DIR)/webdav
endif
endef

define Package/webdav-go/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/webdav $(1)/usr/bin/webdav-go
endef

$(eval $(call GoBinPackage,webdav-go))
$(eval $(call BuildPackage,webdav-go))
