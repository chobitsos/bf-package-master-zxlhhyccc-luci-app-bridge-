# SPDX-License-Identifier: GPL-3.0-only
#
# Copyright (C) 2022 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=baidupcs-web
PKG_VERSION:=3.7.4-nnew
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/zxlhhyccc/BaiduPCS-Go.git
PKG_SOURCE_VERSION:=ff0f4736c9482b57d3b70fbc2b7d2ba73ddb9275
PKG_MIRROR_HASH:=0fb1e2582e002b545fcb86fe485cfe9a072e7ae7261035810fbf7589809bd522

PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_CONFIG_DEPENDS:= \
	CONFIG_BAIDUPCS_WEB_COMPRESS_GOPROXY \
	CONFIG_BAIDUPCS_WEB_COMPRESS_UPX

PKG_BUILD_DEPENDS:=golang/host go-rice/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/Erope/BaiduPCS-Go
GO_PKG_LDFLAGS:=-s -w
GO_PKG_LDFLAGS_X:=main.Version=v$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/baidupcs-web
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Download Manager
  TITLE:=A web controller for BaiduPCS-Go
  URL:=https://github.com/zxlhhyccc/BaiduPCS-Go
  DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle
endef

define Package/baidupcs-web/description
BaiduPCS-Web is a web controller for BaiduPCS-Go
endef

define Package/baidupcs-web/config
config BAIDUPCS_WEB_COMPRESS_GOPROXY
	bool "Compiling with GOPROXY proxy"
	default n

config BAIDUPCS_WEB_COMPRESS_UPX
	bool "Compress executable files with UPX"
	depends on !mips64
	default n
endef

ifeq ($(CONFIG_BAIDUPCS_WEB_COMPRESS_GOPROXY),y)
	export GO111MODULE=on
	export GOPROXY=https://goproxy.baidu.com
endif

define Build/Prepare
	$(call Build/Prepare/Default)
endef

#define Build/UpdateDependencies
#	cd $(PKG_BUILD_DIR) && \
#		go get -u && \
#		go mod tidy && \
#		go mod vendor
#endef

define Build/Compile
	#$(call Build/UpdateDependencies)
	( \
		pushd "$(PKG_BUILD_DIR)/internal/pcsweb" ; \
		rice embed-go ; \
		popd ; \
		$(call GoPackage/Build/Compile) ; \
	)
ifeq ($(CONFIG_BAIDUPCS_WEB_COMPRESS_UPX),y)
	$(STAGING_DIR_HOST)/bin/upx --lzma --best $(GO_PKG_BUILD_BIN_DIR)/BaiduPCS-Go
endif
endef

define Package/baidupcs-web/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(GO_PKG_BUILD_BIN_DIR)/BaiduPCS-Go $(1)/usr/bin/baidupcs-web
endef

$(eval $(call GoBinPackage,baidupcs-web))
$(eval $(call BuildPackage,baidupcs-web))
