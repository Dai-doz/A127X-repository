# OneUI 5.1 - 9 Porting Guide for Samsung Galaxy A12s Series [Exynos 850]

**Caution:** This process may require up to **20+ GB** of free space on your device for successful ROM porting. Ensure you have enough space available before proceeding!

**Warning:** Mediatek base will bootloop , so i prefer you don't use MSSI base also this tutorial only make it booting up , some type of bug you need to debug yourself!

## Requirements
1. **Download ROMs:**
   - Download the **Any latest One UI ROM for A12s** from SamFirm or SamFW.

## Features:
- Custom vendor with upstream r49p1 blobs
- Patched `services.jar` with:
  - Secure Folder fix.
  - Secure Screenshot fix.
  - Camera fix.
  - KnoxGuard fix.
- Identifies the device as **Galaxy A12s**.

## Compatibility:
- Works for:
  - **Galaxy M12** 
  - **Galaxy A12s**
  - **Galaxy A14 4G**
  - Possibly **M13** and **A13**.

## Step 1 

- Extract Downloaded A12s firmware and Target [S23FE] firmware

- Extract img use QCRB / CRB / MIO Kitchen

- Then import stock a12s vendor to target [S23FE] vendor 
```bash
00000000-0000-0000-0000-4b45594d5354
00000000-0000-0000-0000-46494e474502
bin/hw/vendor.samsung.hardware.biometrics.fingerprint@3.0-service
hal_fingerprint_default_exec
lib64/hw/fingerprint.default.so
lib64/vendor.samsung.hardware.biometrics.fingerprint@3.0.so
lib64/android.hardware.biometrics.fingerprint@2.1.so
lib64/libbauthserver.so & lib64/libbauthtzcommon.so
lib64/libsynaFpSensorTestNwd.so
lib64/libegis_fp_normal_sensor_test.so
lib64/libgf_in_system_lib.so
lib64/libqfp_sensortest.so
etc/init/vendor.samsung.hardware.biometrics.fingerprint@3.0-service.rc
etc/init/init.fingerprint.rc
etc/vintf/manifest/vendor.samsung.hardware.biometrics.fingerprint@3.0-service.xml
```

- Import init blobs from blobs/system to system 

- Patches vndk import vndk file from my release , a12s use vndk33 vendor so use 33.0 depends on vendor vndk

- Patches SELinux 
```bash

Remove fabricrypto "audiomirroring" "fabriccrypto" "hal_dsms_default" "qb_id_prop" "hal_dsms_service" "proc_compaction_proactiveness" "sbauth" "ker_app" "kpp_app" "kpp_data" "attiqi_app" "kpoc_charger" "sec_diag" "mosey_app" "vendor_smcinvoke_device" "perf_prop" "uwb_regulation_skip_prop" 

# In /system/system/etc/selinux/plat_sepolicy.cil commit: 

Remove genfscon sysfs
Remove genfscon proc "/sys/vm/compaction_proactiveness

# In /etc/selinux/system_ext_sepolicy.cil commit:

Remove genfscon proc "/sys/vm/compaction_proactiveness
Remove genfscon proc "/sys/kernel/firmware_config" 

# In/etc/selinux/system_ext_property_contexts commit:

Remove init.svc.vendor.wvkprov_server_hal
```

- Disable FRP lock in product/build.prop with ro.frp.pst=

- Remove SystemUI_* / TeleSerive_* in product/overlay and import blobs/product to target product/overlay

- Debloat if you want [Optional]

- Import patches from this repo . For example OneUI 8.5 will use Sixteen QRP2 branch 

- Merge stock values from ff.xml to target ff.xml , if you want GalaxyAI use extra.xml 

## Step 2

- Use apktool to decompile services.jar / ssrm.jar

- After that commit my change in patches.txt to smali and all done

## Step 3 

- Repack system.img / product.img / vendor.img / odm.img

- Repack to super.img [Optional] 

- Use repartition if too big [Optional] also Super of A12s is 5700MB - 5,7GB

- Flash in recovery and enjoy your own ROM , GPL3 hehe

> **Note:** Be sure to follow the instructions carefully and make sure your device has enough space to avoid issues during the porting process.
