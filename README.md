## Voltage OS Setup and Build Guide

This guide walks you through setting up the environment, cloning necessary repositories, configuring device-specific settings, and building Voltage OS for Android 14. Each command is ready to copy-paste for smooth execution.

### Environment Setup
</b>

  ```bash
mkdir voltage && cd voltage
  ```

<b>
  1. Install Repo Tool
</b>

  ```bash
curl https://storage.googleapis.com/git-repo-downloads/repo > repo
chmod +x repo
sudo mv repo /usr/bin/repo
  ```
<b>
  2. Install Required Packages
</b>

  ```bash
sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev libc6-dev-i386 x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig
  ```
<b>
  3. Configure Git
</b>

  ```bash
git config --global user.email "EMAIL_ADDRESS"
git config --global user.name "sabamdarif"
  ```
<b>
  4. Install Git LFS
</b>

```bash
sudo apt install git-lfs
```

```bash
git lfs install
```
<b>
  5. Optimize Git for Large Repositories
</b>

```bash
git config --global http.postBuffer 524288000
```

```bash
git config --global core.compression 0
```

```bash
git config --global http.version HTTP/1.1
```

---

### Initialize Voltage OS Repository

<b>
  1. Initialize Repo
</b>

  ```bash
repo init -u https://github.com/VoltageOS/manifest.git -b 14 --git-lfs
  ```
<b>
  2. Sync Repo
</b>

  ```bash
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
  ```

---

### Local Manifests Repository

**Benefits:**

- ✨ Manage additional repositories for your project.
-   Simplify `repo sync` to update all repositories.
-   Keep your local customizations organized.

**How it Works:**

The `repo` tool reads manifests (`.xml` files) to determine which repositories to download. This local manifest acts as an extension, allowing you to specify additional repositories without modifying the default manifest.

**Getting Started:**

1. Clone this repository into your local project directory (e.g., `.repo/manifests`).
2. Add entries for your custom repositories using the standard manifest format.
3. Run `repo sync` to download and update all repositories, including your custom ones.

**Note:**

- Ensure proper indentation and formatting for valid manifest files.
- Refer to the `repo` documentation for detailed information on manifest syntax.

#### Example:-

<b>
  1. DELETED DIRECTORIES
</b>

```bash
rm -rf .repo/local_manifests
```
```bash
rm -rf .repo/local_manifests
rm -rf device/realme
rm -rf kernel/oplus
rm -rf vendor/realme
rm -rf hardware/oplus
rm -rf device/oneplus
rm -rf vendor/oneplus
rm -rf vendor/oplus
rm -rf vendor/lineage-priv/keys/
rm -rf packages/apps/RevampedFMRadio
rm -rf packages/apps/Droid-ify
rm -rf packages/apps/ViMusic
rm -rf packages/apps/ViPER4AndroidFX
```

<b>
  2. Initialize Repo
</b>

```bash
repo init -u https://github.com/RisingOS-staging/android -b fifteen --git-lfs --depth=1
```

<b>
  3. Clone local_manifests repository
</b>

```bash
git clone https://github.com/DevInfinix/android-aosp-local-manifests --depth 1 -b 15-rising .repo/local_manifests
if [ ! 0 == 0 ]
    then curl -o .repo/local_manifests https://github.com/DevInfinix/android-aosp-local-manifests.git
fi
```

<b>
  4. Sync Repo
</b>

```bash
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
```

<b>
  5. Clone Keys
</b>

```bash
rm -rf vendor/lineage-priv/keys/
```

```bash
git clone https://github.com/DevInfinix/devinfinix-aosp-roms-keys --depth=1 -b 14.0-los21 vendor/lineage-priv/keys/
```

<b>
  6. Set some environment variables
</b>

```bash
export TZ=Asia/Kolkata
```

```bash
export BUILD_USERNAME=DevInfinix
```

```bash
export BUILD_HOSTNAME=Garudinix
```

<b>
  7. Start Build Process
</b>

```bash
source build/envsetup.sh
```

```bash
riseup ice userdebug
```

```bash
rise b
```

### Clone Local Manifests and Required Repositories

<b>
  1. Clone Local Manifests
</b>

  ```bash
git clone https://github.com/sabamdarif/local_manifests --depth 1 -b spark .repo/local_manifests
  ```
<b>
  2. Additional Repo Sync (with Retry)
</b>

  ```bash
repo sync -c --no-clone-bundle --no-tags --optimized-fetch --prune --force-sync -j $(nproc --all) || repo sync -c --no-clone-bundle --no-tags --optimized-fetch --prune --force-sync -j $(nproc --all)
  ```
<b>
  3. Kernel Tree
</b>

  ```bash
git clone https://github.com/nashc-dev/android_kernel_realme_nashc.git -b lineage-21 kernel/realme/mt6785
  ```
<b>
  4. Vendor Tree
</b>

  ```bash
git clone --depth=1 https://github.com/nashc-dev/android_vendor_realme_nashc.git -b lineage-21 vendor/realme/nashc
  ```
OR,

  ```bash
git clone --depth=1 https://github.com/realme-nashc/android_vendor_realme_nashc.git -b lineage-21 vendor/realme/nashc
  ```

<b>
  5. Device Tree
</b>

  ```bash
git clone --depth=1 https://github.com/sabamdarif/android_device_realme_nashc.git -b ROM-NAME (lineage-21) device/realme/nashc
  ```

<b>
  6. android_frameworks_base
</b>

  ```bash
git clone --depth=1 https://github.com/sabamdarif/android_frameworks_base.git -b lineage-21.0 frameworks/base
  ```
---
### Hardware Components Setup

<b>
  1. Oplus Hardware Tree
</b>

  ```bash
git clone --depth=1 https://github.com/nashc-dev/android_hardware_oplus.git -b lineage-21 hardware/oplus
  ```
or
```bash
git clone --depth=1 https://github.com/LineageOS/android_hardware_oplus.git -b lineage-21 hardware/oplus
```

<b>
  2. MediaTek Hardware Tree
</b>

  ```bash
git clone --depth=1 https://github.com/nashc-dev/android_hardware_mediatek.git -b lineage-20 hardware/mediatek
  ```
or

```bash
git clone --depth=1 https://github.com/LineageOS/android_hardware_mediatek.git -b lineage-21 hardware/mediatek
  ```

<b>
  3. SELinux Policy
</b>

  ```bash
git clone https://github.com/nashc-dev/android_device_mediatek_sepolicy_vndr.git -b lineage-20 device/mediatek/sepolicy_vndr
  ```
or

  ```bash
git clone https://github.com/realme-nashc/android_device_mediatek_sepolicy_vndr.git -b lineage-21 device/mediatek/sepolicy_vndr
  ```
or

  ```bash
git clone https://github.com/LineageOS/android_device_mediatek_sepolicy_vndr.git -b lineage-21 device/mediatek/sepolicy_vndr
  ```

<b>
  4. GPS Tree
</b>

  ```bash
git clone --depth=1 https://github.com/oplus-ossi-development/android_hardware_mediatek_gnss -b lineage-21 hardware/mediatek/gnss
  ```
<b>
  5. Patches
</b>

  ```bash
git clone https://github.com/nashc-dev/patches.git -b lineage-21
  ```
<b>
  6. OPlus Extras Package
</b>

  ```bash
git clone https://github.com/Killerpac/packages_apps_OPlusExtras.git
  ```

<b>
  7. ImsService
</b>

  ```bash
git clone https://github.com/realme-nashc/ImsService.git
  ```


___

### ROM Configuration

<b>
  1. Camera Configuration
</b>

  ```bash
git clone https://gitlab.com/pjgowtham/proprietary_vendor_oplus_camera.git -b lineage-21 vendor/oplus/camera
  ```
<b>
  2. GApps Setup
</b>

  ```bash
git clone https://gitlab.com/MindTheGapps/vendor_gapps.git -b upsilon vendor/gapps
  ```
  Add the following to the device tree under `lineage_porsche.mk`:
  ```bash
$(call inherit-product, vendor/gapps/arm64/arm64-vendor.mk)
  ```
<b>
  3. Theme and Utility Commit (Syberia Project)
</b>

```bash
cd frameworks/base
```

```bash
git fetch https://github.com/syberia-project/platform_frameworks_base.git
```

```bash
git cherry-pick c2aaec3ff18f427d5f2984a8f05e263360a0c1f8
```
<b>
  4. Rename and Modify Configurations
</b>

```bash
cd ROM_FOLDER
```

```bash
mv lineage_nashc.mk ROM-NAME_nashc.mk
```

```bash
nano ROM-NAME_nashc.mk
```

set

```bash
PRODUCT_NAME := ROM-NAME_nashc
```

<b>
  5. Update AndroidProducts
</b>

  ```bash
nano AndroidProducts.mk
  ```

Add:

```bash
PRODUCT_MAKEFILES := \
    $(LOCAL_DIR)/ROM-NAME_nashc.mk
```

<b>
  6. Additional Configurations
</b>

  ```bash
nano device.mk
  ```

Add:

```bash
TARGET_DISABLE_EPPE := true
```

```bash
$(call inherit-product-if-exists, vendor/oplus/camera/opluscamera.mk)
```

---

### Build Setup

<b>
  1. Set up Build Environment
</b>

```bash
export USE_CCACHE=1
```

```bash
ccache -M 50G
```
>It will set ccache for building ROMs."50G" is to metion the amount of ccache you are allocating to build the ROM. 30 to 50 GB should be enough for building ROM for one device.

```bash
export CONFIG_STATE_NOTIFIER=y
```

```bash
export SELINUX_IGNORE_NEVERALLOWS=true
```
<b>
  2. Start Build Process
</b>

```bash
. build/envsetup.sh
```

```bash
breakfast nashc
```

```bash
brunch nashc
```

---

### Extra ROM Features

Add the following configurations in the specified XML files:

- Overlay for Smart Pixels:

  ```bash
  <bool name="config_supportSmartPixels">true</bool>
  ```

- Pocket Mode:

  ```bash
  <bool name="config_pocketModeSupported">true</bool>
  ```

- Battery Health:

  ```bash
  <bool name="config_supportBatteryHealth">true</bool>
  <string name="config_batteryCalculatedCapacity">/sys/class/power_supply/bms/charge_full</string>
  ```

- First item

  ```bash
  ```

- First item

  ```bash
  ```

- First item

  ```bash
  ```

## Kernelsu Build Process

<b>
  1. Clone Repositories
</b>

  ```bash
git clone https://github.com/nashc-dev/android_kernel_realme_nashc.git -b lineage-21
  ```

  ```bash
cd android_kernel_realme_nashc
  ```
<b>
  2. Clone Clang
</b>

  ```bash
git clone --depth=1 https://gitlab.com/Jprimero15/lolz_clang.git -b main
  ```
<b>
  3. Add Kernelsu Next
</b>

  ```bash
wget https://raw.githubusercontent.com/MrErenK/Scripts/refs/heads/main/kernel/KSU.patch
  ```

  ```bash
patch -p1 < KSU.patch
  ```

  ```bash
curl -LSs "https://raw.githubusercontent.com/rifsxd/KernelSU-Next/next/kernel/setup.sh" | bash -s next
  ```

<b>
  4. Add some prop in debconfig
</b>

- Keep in mind that on some devices, your defconfig may be in `arch/arm64/configs/your_defconfig` or in other cases `arch/arm64/configs/vendor/your_defconfig`
- see if you have this below props or not (if not add them)

```bash
CONFIG_KPROBES=y
CONFIG_HAVE_KPROBES=y
CONFIG_KPROBE_EVENTS=y
# KernelSU
CONFIG_KSU=y
  ```

<b>
  5. generate the config
</b>

  ```bash
make O=out ARCH=arm64 begonia_user_defconfig #depend on your device
  ```

<b>
  6. build
</b>

```bash
ARCH=arm64 \                                
CROSS_COMPILE="${PWD}/lolz_clang/bin/aarch64-linux-gnu-" \
CROSS_COMPILE_COMPAT="${PWD}/lolz_clang/bin/arm-linux-gnueabi" \
CROSS_COMPILE_ARM32="${PWD}/lolz_clang/bin/arm-linux-gnueabi-" \
CLANG_TRIPLE=aarch64-linux-gnu- \
make -j$(nproc --all) LLVM=1 LLVM_IAS=1 LD=ld.lld AR=llvm-ar NM=llvm-nm AS=llvm-as OBJCOPY=llvm-objcopy OBJDUMP=llvm-objdump STRIP=llvm-strip O=out
```
- might depend on the clang you use
- also run it inside main kernel folder like here it is `android_kernel_realme_nashc`

<b>
  5. Clone anykernel and zip it
</b>

  ```bash
git clone https://github.com/Neebe3289/AnyKernel3 -b begonia
  ```

```bash
cd AnyKernel3
```

```bash
nano anykernel.sh
```
- properties() { ' line add

```bash
kernel.string=Astera v4.14.336 lolz-KSU-Next by sabamdarif<https://github.com/sabamdarif> for Redmi Note 8 Pro (begonia) | KernelSU Version: 12017
```

```bash
cp /home/username/android_kernel_realme_nashc/out/arch/arm64/boot/Image.gz .
```

```bash
zip -r9 ksunext-1.0.3.zip *
```