include $(TOPDIR)/rules.mk

PKG_NAME:=Szt-k3sreenctrl
PKG_VERSION:=1.0
PKG_RELEASE:=2

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/shenzt68/k3screenctrl.git
PKG_SOURCE_VERSION:=182bd161e01493959a3a12cdc58e4ef37e3a472c
PKG_MIRROR_HASH:=f1cfb89a7b8b3353b916e253a0d58b859538b3edc182c5b8b35239b20334eca2
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_SOURCE_VERSION)
PKG_SOURCE:=$(PKG_SOURCE_SUBDIR).tar.xz
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)

PKG_MAINTAINER:=Hamster Tian <haotia@gmail.com>

include $(INCLUDE_DIR)/package.mk

TARGET_CFLAGS+= -D_GNU_SOURCE

define Package/$(PKG_NAME)
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=@TARGET_bcm53xx_generic_DEVICE_phicomm_k3 +@BUSYBOX_CONFIG_ARPING +@KERNEL_DEVMEM +coreutils +coreutils-od +bash +curl +bc +jq +@BUSYBOX_CONFIG_ARP +@BUSYBOX_CONFIG_OD
  TITLE:=LCD screen controller on PHICOMM K3
  URL:=https://github.com/shenzt68/k3screenctrl 
endef

define Package/$(PKG_NAME)/description
 K3 Screen Controller (k3screenctrl) is a program utilizing
the LCD screen on PHICOMM K3 to display some stats.
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
	if [ -f /etc/uci-defaults/k3screenctrl ]; then
		( . /etc/uci-defaults/k3screenctrl ) && \
		rm -f /etc/uci-defaults/k3screenctrl
	fi
	rm -rf /tmp/luci-indexcache /tmp/luci-modulecache
fi
exit 0
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/lib/k3screenctrl
	$(INSTALL_DIR) $(1)/lib/k3screenctrl/oui
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/k3screenctrl $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/k3screenctrl/*.sh $(1)/lib/k3screenctrl/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/lib/k3screenctrl/oui/oui.txt $(1)/lib/k3screenctrl/oui/
	$(INSTALL_DATA) ./files/k3screenctrl.config $(1)/etc/config/k3screenctrl
	$(INSTALL_BIN) ./files/k3screenctrl.init $(1)/etc/init.d/k3screenctrl
	$(INSTALL_BIN) ./files/k3screenctrl.uci $(1)/etc/uci-defaults/k3screenctrl
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
