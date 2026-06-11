include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

define Device/Default
	PROFILES := Default
	KERNEL_LOADADDR := 0x41000000
	DEVICE_DTS = $$(SOC)-$(lastword $(subst _, ,$(1)))
	DEVICE_DTS_CONFIG := config@1
	DEVICE_DTS_DIR := ../dts
	IMAGES := sysupgrade.bin
	IMAGE/sysupgrade.bin = sysupgrade-tar | append-metadata
	IMAGE/sysupgrade.bin/squashfs :=
endef

define Device/FitImage
	KERNEL_SUFFIX := -uImage.itb
	KERNEL = kernel-bin | libdeflate-gzip | fit gzip $$(KDIR)/image-$$(DEVICE_DTS).dtb
	KERNEL_NAME := Image
endef

define Device/FitImageLzma
	KERNEL_SUFFIX := -uImage.itb
	KERNEL = kernel-bin | lzma | fit lzma $$(KDIR)/image-$$(DEVICE_DTS).dtb
	KERNEL_NAME := Image
endef

define Device/EmmcImage
	IMAGES += factory.bin
	IMAGE/factory.bin := append-rootfs | pad-rootfs | pad-to 64k
	IMAGE/sysupgrade.bin/squashfs := append-rootfs | pad-to 64k | sysupgrade-tar rootfs=$$$$@ | append-metadata
endef

define Device/UbiFit
	KERNEL_IN_UBI := 1
	IMAGES += factory.ubi
	IMAGE/factory.ubi := append-ubi
endef

define Device/jio_6j11
  $(Device/FitImage)
  $(Device/UbiFit)
  DEVICE_VENDOR := Jio
  DEVICE_MODEL := JIDU6111 (6J11)
  SOC := ipq9574
  DEVICE_DTS := ipq9574-jio-6j11
  DEVICE_DTS_CONFIG := config@1
  DEVICE_PACKAGES := kmod-ath12k kmod-ath12k-pci kmod-ath12k-ahb \
	kmod-ath11k-pci ath11k-firmware-qcn9074 ipq-wifi-jio_6j11
  BLOCKSIZE := 128k
  PAGESIZE := 2048
  IMAGES += squashfs-initramfs-uImage.itb
  IMAGE/squashfs-initramfs-uImage.itb := append-kernel | pad-to 64k | append-rootfs
  IMAGE/sysupgrade.bin := sysupgrade-tar | append-metadata
endef
TARGET_DEVICES += jio_6j11

$(eval $(call BuildImage))
