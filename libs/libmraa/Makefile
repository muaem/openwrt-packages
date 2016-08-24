#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libmraa
PKG_VERSION:=0.8.0

PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git@git.muabaobao.com:depot/package/mraa.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=0d26d618127223623c12334df23d128e0ded243e
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
CMAKE_INSTALL:=1

ENABLE_PYTHON:=0
ENABLE_NODE:=0
ENABLE_SWIG:=0

PKG_BUILD_DEPENDS:=
ifeq ($(ENABLE_PYTHON),1)
	PKG_BUILD_DEPENDS+=python/host
endif
ifeq ($(ENABLE_NODE),1)
	PKG_BUILD_DEPENDS+=node node/host
endif
ifeq ($(ENABLE_SWIG),1)
	PKG_BUILD_DEPENDS+=swig/host
endif

PKG_MAINTAINER:=John Crispin <blogic@openwrt.org>
PKG_LICENSE:=LGPL-2.1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

CMAKE_OPTIONS=-DBUILDARCH=$(CONFIG_ARCH) \
	-DENABLEEXAMPLES=0 \
	-DNODE_EXECUTABLE=$(STAGING_DIR_HOST)/bin/node \
	-DSWIG_DIR=$(STAGING_DIR_HOST)/bin

TARGET_CFLAGS+=-I$(STAGING_DIR)/usr/include/node

define Package/libmraa
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libstdcpp
ifeq ($(ENABLE_PYTHON),1)
  DEPENDS:=+python
endif
# Workaround for "missing libpthread.so"
ifneq ($(ENABLE_PYTHON),1)
	DEPENDS:=+librt
endif
  TITLE:=Intel IoT lowlevel IO library
endef

define Package/libmraa/install
	$(INSTALL_DIR) $(1)/usr/lib $(1)/usr/bin
ifeq ($(ENABLE_PYTHON),1)
	$(INSTALL_DIR) $(1)/usr/lib/python2.7/site-packages
endif
ifeq ($(ENABLE_NODE),1)
	$(INSTALL_DIR) $(1)/usr/lib/node/mraa
endif
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/libmraa.so* $(1)/usr/lib/
ifeq ($(ENABLE_NODE),1)
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/node_modules/mraa/* $(1)/usr/lib/node/mraa/
endif
ifeq ($(ENABLE_PYTHON),1)
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/python2.7/site-packages/* $(1)/usr/lib/python2.7/site-packages/
	#$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/share/mraa/examples/python/blink-io8.py $(1)/usr/bin/
endif
endef

$(eval $(call BuildPackage,libmraa))
