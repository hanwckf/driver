# All rights reserved.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=rt5592
PKG_VERSION:=

PKG_KCONFIG_COMMON:= \
RALINK_MT7628 RALINK_MT7621 RALINK_MT7620 RALINK_RT3883 RALINK_RT5350 RALINK_RT3352 RALINK_RT3052 \
RT_FIRST_IF_NONE RT_FIRST_IF_RT2860  RT_FIRST_IF_MT7620 RT_FIRST_IF_MT7628 RT_FIRST_IF_RT3090 \
RT_FIRST_IF_RT5392 RT_FIRST_IF_RT5592 RT_FIRST_IF_RT3593 RT_FIRST_IF_MT7610E  RT_FIRST_IF_MT7602E \
RT_FIRST_IF_MT7612E  RT_FIRST_IF_MT7603E RT_FIRST_IF_MT7615E RT_FIRST_IF_RANGE_2GHZ \
RT_FIRST_IF_RANGE_5GHZ RT_FIRST_IF_EEPROM_FLASH RT_FIRST_IF_EEPROM_EFUSE RT_FIRST_IF_EEPROM_PROM \
RT_FIRST_IF_RF_OFFSET RT_FIRST_CARD RT_FIRST_CARD_EEPROM RT_SECOND_IF_NONE \
RT_SECOND_IF_RT3352_INIC RT_SECOND_IF_RT3090 RT_SECOND_IF_RT5392 RT_SECOND_IF_RT5592 \
RT_SECOND_IF_RT3593 RT_SECOND_IF_MT7610E RT_SECOND_IF_MT7602E RT_SECOND_IF_MT7612E \
RT_SECOND_IF_MT7603E RT_SECOND_IF_MT7615E RT_SECOND_IF_RANGE_2GHZ RT_SECOND_IF_RANGE_5GHZ \
SECOND_IF_EEPROM_FLASH SECOND_IF_EEPROM_EFUSE SECOND_IF_EEPROM_PROM \
RT_SECOND_IF_RF_OFFSET RT_SECOND_CARD RT_SECOND_CARD_EEPROM RALINK_RT5350_1T1R \
RALINK_RT3050_1T1R RALINK_RT3051_1T2R RALINK_RT3052_2T2R RALINK_RT3352  RALINK_RT3352_2T2R \
RALINK_RT3662_2T2R RALINK_RT3883_3T3R \
RT_SINGLE_SKU RT_MAX_CLIENTS  RT_BAND_STEERING RT_DOT11R_FT RT_DOT11K_RRM RT_80211N_DRAFT3 \
RT_80211W_PMF RT_WAPI RT_WSC RT_WSC_V2 RT_WSC_NFC RT_ED_MONITOR RT_IGMP_SNOOP \
RT_MCAST_RATE_SPECIFIC RT_DELAYED_TCP_ACK  RT_HDR_TRANS RT_CSO RT_NETIF_BLOCK \
RT_VIDEO_TURBINE RT_SNMP RT_CFG80211 RT_READ_MAC_FROM_MTD RT_MEMORY_OPTIMIZATION RT_BIG_ENDIAN \
RT_MC_SUPPORT RT_ATE RT_QA RT_DEBUG 

PKG_KCONFIG:= $(PKG_KCONFIG_COMMON) \
RT5592_AP RT5592_AP_LED RT5592_AP_LED_SOFT RT5592_AP_LED_SOFT_GPIO RT5592_AP_LLTD \
RT5592_AP_WDS RT5592_AP_MBSS RT5592_AP_MBSS_NEW_MBSSID_MODE RT5592_AP_APCLI RT5592_AP_DFS \
RT5592_AP_CARRIER RT5592_AP_EXT_CHANNEL_LIST RT5592_AP_DLS RT5592_AP_IDS RT5592_AP_GREENAP \
RT5592_AP_EP_SUPPORT

include $(INCLUDE_DIR)/package.mk

define KernelPackage/rt5592
  CATEGORY:=Ralink
  TITLE:=Ralink RT5592 AP driver
  FILES:=$(PKG_BUILD_DIR)/rt5592_ap/rt5592_ap.ko
  AUTOLOAD:=$(call AutoLoad,91,rt5592_ap)
  MENU:=1
endef

define KernelPackage/rt5592/config
	source "$(SOURCE)/Config-rt5592.in"
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./src/* $(PKG_BUILD_DIR)/
	$(call Build/Prepare/Default)
endef

#		NOSTDINC_FLAGS="-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi -I$(STAGING_DIR)/usr/include/mac80211-backport \
#			-I$(STAGING_DIR)/usr/include/mac80211/uapi -I$(STAGING_DIR)/usr/include/mac80211 \
#			-include backport/backport.h \
#			-Wno-unused-variable -Wno-unused-function \
#			-Wno-return-type" \
#


NOSTDINC_FLAGS = \
	-I$(LINUX_DIR)/net/nat/hw_nat/ 
	
# TARGET_CFLAGS += \
# 	-I$(LINUX_DIR)/net/nat/hw_nat/

define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(LINUX_KARCH)" \
		RT_DRV_DIR="$(PKG_BUILD_DIR)/rt5592" \
		RT_DRV_PATH="../rt5592" \
		RT_DRV_NAME="rt5592_ap" \
		SUBDIRS="$(PKG_BUILD_DIR)/rt5592_ap/" \
		$(foreach c, $(PKG_KCONFIG),$(if $(CONFIG_$c),CONFIG_$(c)=$(CONFIG_$c)))\
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

define KernelPackage/rt5592/install
	$(INSTALL_DIR) $(1)/etc_ro/Wireless/
# 	$(INSTALL_DATA) ./files/RT5592_EEPROM.bin $(1)/etc_ro/Wireless/
	echo -------------------------------------------------------------------$(CONFIG_RT_FIRST_CARD) $(CONFIG_RT_SECOND_CARD)

endef

$(eval $(call KernelPackage,rt5592))
