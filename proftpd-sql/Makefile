#
# Copyright (C) 2009-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=proftpd-sql
PKG_VERSION:=1.3.6
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=ftp://ftp.proftpd.org/distrib/source
PKG_MD5SUM:=13270911c42aac842435f18205546a1b

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/proftpd-sql
  SUBMENU:=File Transfer
  SECTION:=net
  CATEGORY:=Network
  TITLE:=ProFTPD FTP server with SQL support
  URL:=http://www.proftpd.org/
  MAINTAINER:=Cezary Jackiewicz <cezary@eko.one.pl>
  DEPENDS:=+libattr +libopenssl +libmysqlclient +libsqlite3
endef

define Package/proftpd-sql/conffiles
/opt/etc/proftpd.conf
endef

MAKE_FLAGS += \
	INSTALL_USER=$(shell id -u) \
	INSTALL_GROUP=$(shell id -g)

CONFIGURE_ARGS += \
	--disable-cap \
	--disable-auth-pam \
	--disable-strip \
	--with-lastlog=/opt/var/log \
	--enable-ctrls \
	--enable-nls \
	--with-modules=mod_ban:mod_copy:mod_deflate:mod_site_misc:mod_ctrls_admin:mod_ifsession:mod_load:mod_qos:mod_quotatab:mod_quotatab_file:mod_quotatab_sql:mod_rewrite:mod_shaper:mod_tls:mod_sql:mod_sql_mysql:mod_sql_sqlite:mod_sql_passwd

CONFIGURE_VARS += \
	LIBS="-ldl -lpthread" \
	ac_cv_header_curses_h=no \
	ac_cv_header_ncurses_h=no

define Package/proftpd-sql/install
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_CONF) $(PKG_INSTALL_DIR)/opt/etc/proftpd.conf $(1)/opt/etc
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/ftp{count,dctl,top,who} $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/sbin/{ftpscrub,ftpshut,proftpd} $(1)/opt/sbin
	$(LN) proftpd $(1)/opt/sbin/in.proftpd
	$(INSTALL_BIN) ./files/S88proftpd $(1)/opt/etc/init.d
endef

$(eval $(call BuildPackage,proftpd-sql))
