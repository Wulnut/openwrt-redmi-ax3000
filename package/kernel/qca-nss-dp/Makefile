include $(TOPDIR)/rules.mk

PKG_NAME:=qca-nss-dp
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://git.codelinaro.org/clo/qsdk/oss/lklm/nss-dp.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2022-04-30
PKG_SOURCE_VERSION:=72e9ec4187414461cbcf6ccff100e8b5ebe5f40b
PKG_MIRROR_HASH:=805f16e59c75511132922f97740ebf6bf953845b0bbfd2089c4615456893bb37

PKG_BUILD_PARALLEL:=1
PKG_FLAGS:=nonshared

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/qca-nss-dp
  SECTION:=kernel
  CATEGORY:=Kernel modules
  SUBMENU:=Network Devices
  DEPENDS:=@(TARGET_ipq50xx||TARGET_ipq807x) +kmod-qca-ssdk
  TITLE:=Qualcom NSS dataplane ethernet driver
  FILES:=$(PKG_BUILD_DIR)/qca-nss-dp.ko
  AUTOLOAD:=$(call AutoLoad,31,qca-nss-dp,1)
endef

define KernelPackage/qca-nss-dp/Description
  NSS dataplane ethernet driver for Qualcom SoCs.
endef

define Build/InstallDev
	mkdir -p $(1)/usr/include/qca-nss-dp
	$(CP) $(PKG_BUILD_DIR)/exports/* $(1)/usr/include/qca-nss-dp/
endef

EXTRA_CFLAGS+= \
	-I$(STAGING_DIR)/usr/include/qca-ssdk

NSS_DP_HAL_DIR:=$(PKG_BUILD_DIR)/hal
define Build/Configure
	$(LN) $(NSS_DP_HAL_DIR)/soc_ops/$(CONFIG_TARGET_BOARD)/nss_$(CONFIG_TARGET_BOARD).h \
		$(PKG_BUILD_DIR)/exports/nss_dp_arch.h
endef

define Build/Compile
	+$(MAKE) -C "$(LINUX_DIR)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(LINUX_KARCH)" \
		M="$(PKG_BUILD_DIR)" \
		EXTRA_CFLAGS="$(EXTRA_CFLAGS)" SoC="$(CONFIG_TARGET_BOARD)" \
		$(KERNEL_MAKE_FLAGS) \
		$(PKG_JOBS) \
		modules
endef

$(eval $(call KernelPackage,qca-nss-dp))
