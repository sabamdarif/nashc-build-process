## Environment Setup

### 1. Create Working Directory

```bash
mkdir voltage && cd voltage
```

### 2. Install Repo Tool

```bash
curl https://storage.googleapis.com/git-repo-downloads/repo > repo
chmod +x repo
sudo mv repo /usr/bin/repo
```

### 3. Install Required Packages

```bash
sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev libc6-dev-i386 x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig
```

### 4. Configure Git

```bash
git config --global user.email "EMAIL_ADDRESS"
git config --global user.name "sabamdarif"
```

### 5. Install Git LFS

```bash
sudo apt install git-lfs
git lfs install
```

### 6. Optimize Git for Large Repositories

```bash
git config --global http.postBuffer 524288000
git config --global core.compression 0
git config --global http.version HTTP/1.1
```

---

## Initialize Voltage OS Repository

### 1. Initialize Repo

```bash
repo init -u https://github.com/VoltageOS/manifest.git -b 14 --git-lfs
```

### 2. Sync Repo

```bash
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
```

---

## Local Manifests Repository

**Benefits:**

- ✨ Manage additional repositories for your project
- Simplify `repo sync` to update all repositories
- Keep your local customizations organized

**How it Works:**

The `repo` tool reads manifests (`.xml` files) to determine which repositories to download. This local manifest acts as an extension, allowing you to specify additional repositories without modifying the default manifest.

**Getting Started:**

1. Clone this repository into your local project directory (e.g., `.repo/manifests`)
2. Add entries for your custom repositories using the standard manifest format
3. Run `repo sync` to download and update all repositories, including your custom ones

**Note:**

- Ensure proper indentation and formatting for valid manifest files
- Refer to the `repo` documentation for detailed information on manifest syntax

### Example:

#### 1. Delete Directories

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

#### 2. Initialize Repo

```bash
repo init -u https://github.com/RisingOS-staging/android -b fifteen --git-lfs --depth=1
```

#### 3. Clone local_manifests Repository

```bash
git clone https://github.com/DevInfinix/android-aosp-local-manifests --depth 1 -b 15-rising .repo/local_manifests
if [ ! 0 == 0 ]
    then curl -o .repo/local_manifests https://github.com/DevInfinix/android-aosp-local-manifests.git
fi
```

#### 4. Sync Repo

```bash
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
```

#### 5. Clone Keys

```bash
rm -rf vendor/lineage-priv/keys/
git clone https://github.com/DevInfinix/devinfinix-aosp-roms-keys --depth=1 -b 14.0-los21 vendor/lineage-priv/keys/
```

#### 6. Set Environment Variables

```bash
export TZ=Asia/Kolkata
export BUILD_USERNAME=DevInfinix
export BUILD_HOSTNAME=Garudinix
```

#### 7. Start Build Process

```bash
source build/envsetup.sh
riseup ice userdebug
rise b
```

---

## Clone Local Manifests and Required Repositories

### 1. Clone Local Manifests

```bash
git clone https://github.com/sabamdarif/local_manifests --depth 1 -b spark .repo/local_manifests
```

### 2. Additional Repo Sync (with Retry)

```bash
repo sync -c --no-clone-bundle --no-tags --optimized-fetch --prune --force-sync -j $(nproc --all) || repo sync -c --no-clone-bundle --no-tags --optimized-fetch --prune --force-sync -j $(nproc --all)
```

### 3. Kernel Tree

```bash
git clone https://github.com/nashc-dev/android_kernel_realme_nashc.git -b lineage-21 kernel/realme/mt6785
```

### 4. Vendor Tree

```bash
git clone --depth=1 https://github.com/nashc-dev/android_vendor_realme_nashc.git -b lineage-21 vendor/realme/nashc
```

**OR**

```bash
git clone --depth=1 https://github.com/realme-nashc/android_vendor_realme_nashc.git -b lineage-21 vendor/realme/nashc
```

### 5. Device Tree

```bash
git clone --depth=1 https://github.com/sabamdarif/android_device_realme_nashc.git -b ROM-NAME (lineage-21) device/realme/nashc
```

### 6. android_frameworks_base

```bash
git clone --depth=1 https://github.com/sabamdarif/android_frameworks_base.git -b lineage-21.0 frameworks/base
```

---

## Hardware Components Setup

### 1. Oplus Hardware Tree

```bash
git clone --depth=1 https://github.com/nashc-dev/android_hardware_oplus.git -b lineage-21 hardware/oplus
```

**OR**

```bash
git clone --depth=1 https://github.com/LineageOS/android_hardware_oplus.git -b lineage-21 hardware/oplus
```

### 2. MediaTek Hardware Tree

```bash
git clone --depth=1 https://github.com/nashc-dev/android_hardware_mediatek.git -b lineage-20 hardware/mediatek
```

**OR**

```bash
git clone --depth=1 https://github.com/LineageOS/android_hardware_mediatek.git -b lineage-21 hardware/mediatek
```

### 3. SELinux Policy

```bash
git clone https://github.com/nashc-dev/android_device_mediatek_sepolicy_vndr.git -b lineage-20 device/mediatek/sepolicy_vndr
```

**OR**

```bash
git clone https://github.com/realme-nashc/android_device_mediatek_sepolicy_vndr.git -b lineage-21 device/mediatek/sepolicy_vndr
```

**OR**

```bash
git clone https://github.com/LineageOS/android_device_mediatek_sepolicy_vndr.git -b lineage-21 device/mediatek/sepolicy_vndr
```

### 4. GPS Tree

```bash
git clone --depth=1 https://github.com/oplus-ossi-development/android_hardware_mediatek_gnss -b lineage-21 hardware/mediatek/gnss
```

### 5. Patches

```bash
git clone https://github.com/nashc-dev/patches.git -b lineage-21
```

### 6. OPlus Extras Package

```bash
git clone https://github.com/Killerpac/packages_apps_OPlusExtras.git
```

### 7. ImsService

```bash
git clone https://github.com/realme-nashc/ImsService.git
```

---

## ROM Configuration

### 1. Camera Configuration

```bash
git clone https://gitlab.com/pjgowtham/proprietary_vendor_oplus_camera.git -b lineage-21 vendor/oplus/camera
```

### 2. GApps Setup

```bash
git clone https://gitlab.com/MindTheGapps/vendor_gapps.git -b upsilon vendor/gapps
```

Add the following to the device tree under `lineage_porsche.mk`:

```bash
$(call inherit-product, vendor/gapps/arm64/arm64-vendor.mk)
```

### 3. Theme and Utility Commit (Syberia Project)

```bash
cd frameworks/base
git fetch https://github.com/syberia-project/platform_frameworks_base.git
git cherry-pick c2aaec3ff18f427d5f2984a8f05e263360a0c1f8
```

### 4. Rename and Modify Configurations

```bash
cd ROM_FOLDER
mv lineage_nashc.mk ROM-NAME_nashc.mk
nano ROM-NAME_nashc.mk
```

Set:

```bash
PRODUCT_NAME := ROM-NAME_nashc
```

### 5. Update AndroidProducts

```bash
nano AndroidProducts.mk
```

Add:

```bash
PRODUCT_MAKEFILES := \
    $(LOCAL_DIR)/ROM-NAME_nashc.mk
```

### 6. Additional Configurations

```bash
nano device.mk
```

Add:

```bash
TARGET_DISABLE_EPPE := true
$(call inherit-product-if-exists, vendor/oplus/camera/opluscamera.mk)
```

---

## Build Setup

### 1. Set up Build Environment

```bash
export USE_CCACHE=1
ccache -M 50G
```

> This will set ccache for building ROMs. "50G" is to mention the amount of ccache you are allocating to build the ROM. 30 to 50 GB should be enough for building ROM for one device.

```bash
export CONFIG_STATE_NOTIFIER=y
export SELINUX_IGNORE_NEVERALLOWS=true
```

### 2. Start Build Process

```bash
. build/envsetup.sh
breakfast nashc
brunch nashc
```

---

## Extra ROM Features

Add the following configurations in the specified XML files:

**Overlay for Smart Pixels:**

```xml
<bool name="config_supportSmartPixels">true</bool>
```

**Pocket Mode:**

```xml
<bool name="config_pocketModeSupported">true</bool>
```

**Battery Health:**

```xml
<bool name="config_supportBatteryHealth">true</bool>
<string name="config_batteryCalculatedCapacity">/sys/class/power_supply/bms/charge_full</string>
```

---

## Additional Repository Setup

```bash
sudo add-apt-repository universe
sudo add-apt-repository multiverse
sudo apt update
```

---

## KernelSU Build Process

### Install Dependencies

```bash
sudo apt install -y python3-pip jq libarchive-tools zip lib32z-dev libghc-bzlib-dev pngcrush \
      liblzma-dev python-is-python3 libsdl1.2-dev autoconf libxml2-utils wget pkg-config unzip \
      w3m gawk imagemagick libc6-dev gcc-multilib patchelf gzip clang subversion optipng \
      device-tree-compiler ccache gcc liblz4-dev lzip rsync automake fastboot patch zip pngquant \
      expat lzop libswitch-perl make libcap-dev adb libxml2 bison libxml-simple-perl zlib1g-dev \
      libarchive-tools libtool squashfs-tools gperf libfl-dev ncurses-dev pwgen flex \
      libtinfo6 libmpfr-dev libssl-dev lib32z1-dev libgmp-dev git dpkg-dev libmpc-dev \
      lftp python3 rar git-lfs policycoreutils unrar ncftp tree python3-all-dev bzip2 bc \
      software-properties-common tar libgl1-mesa-dev texinfo schedtool curl libexpat1-dev llvm \
      libc6-dev-i386 apt-utils cmake g++-multilib build-essential re2c axel maven xsltproc g++ \
      libx11-dev libxml-sax-base-perl gnupg bash
```

**Alternative installation command:**

```bash
sudo apt install -y python3-pip jq libarchive-tools zip libghc-bzlib-dev pngcrush ^liblzma.* python-is-python3 libsdl1.2-dev autoconf libxml2-utils wget pkg-config unzip w3m gawk imagemagick libc6-dev gcc-multilib patchelf gzip clang subversion optipng device-tree-compiler ccache gcc ^liblz4-.* lzip rsync automake fastboot patch zip pngquant expat lzop libswitch-perl make libcap-dev adb libxml2 bison libxml-simple-perl zlib1g-dev libtool squashfs-tools gperf ^lzma.* libfl-dev ncurses-dev pwgen flex libtinfo-dev minicom liblz4-tool libmpfr-dev libssl-dev libbz2-dev libgmp-dev git dpkg-dev libmpc-dev lftp python3 rar git-lfs policycoreutils unrar bc ftp software-properties-common tar libgl1-mesa-dev texinfo schedtool curl libexpat1-dev llvm libc6-dev-i386 apt-utils cmake g++-multilib build-essential re2c axel maven xsltproc g++ libx11-dev libxml-sax-base-perl lld cpio gnupg bash
```

### 1. Clone Repositories

```bash
git clone https://github.com/unofficial-nashc-dev/android_kernel_realme_nashc.git
cd android_kernel_realme_nashc
```

### 2. Clone Clang

```bash
git clone --depth=1 https://gitlab.com/Jprimero15/lolz_clang.git -b main
```

### 3. Add KernelSU

```bash
wget https://raw.githubusercontent.com/sabamdarif/nashc-build-process/refs/heads/main/patchs/4.19/ksu-manual-hooks.patch
patch -p1 < ksu-manual-hooks.patch
```

#### Few More Optional Patches

**To fix YonoSBI zygisk detection:**

```bash
wget https://raw.githubusercontent.com/sabamdarif/nashc-build-process/refs/heads/main/patchs/4.19/BACKPORT-ptrace-Move-setting-clearing-ptrace_message.patch
```

**If pm command doesn't work, apply this patch:**

```bash
wget https://github.com/sabamdarif/nashc-build-process/blob/main/patchs/common/fix_pm_command.patch
```

**Setup KernelSU:**

```bash
curl -LSs "https://raw.githubusercontent.com/rsuntk/KernelSU/main/kernel/setup.sh" | bash -s main
```

### 4. Add Configuration to defconfig

- Keep in mind that on some devices, your defconfig may be in `arch/arm64/configs/your_defconfig` or in other cases `arch/arm64/configs/vendor/your_defconfig`
- In our case it's inside `arch/arm64/configs/nashc_defconfig`
- Add these properties:

```bash
# KernelSU
CONFIG_KSU=y
CONFIG_KSU_MANUAL_HOOK=y
```

### 5. Generate the Config

```bash
make O=out ARCH=arm64 nashc_defconfig
```

### 6. Build

```bash
ARCH=arm64 \
CROSS_COMPILE="${PWD}/lolz_clang/bin/aarch64-linux-gnu-" \
CROSS_COMPILE_COMPAT="${PWD}/lolz_clang/bin/arm-linux-gnueabi" \
CROSS_COMPILE_ARM32="${PWD}/lolz_clang/bin/arm-linux-gnueabi-" \
CLANG_TRIPLE=aarch64-linux-gnu- \
make -j$(nproc --all) LLVM=1 LLVM_IAS=1 \
     LD=ld.lld AR=llvm-ar NM=llvm-nm AS=llvm-as \
     OBJCOPY=llvm-objcopy OBJDUMP=llvm-objdump \
     STRIP=llvm-strip O=out
```

> This might depend on the clang you use. Also run it inside the main kernel folder, like here it is `android_kernel_realme_nashc`.

### 7. Clone AnyKernel and Zip It

```bash
git clone https://github.com/Neebe3289/AnyKernel3 -b begonia
cd AnyKernel3
nano anykernel.sh
```

In the `properties() {` line, add:

```bash
kernel.string=Astera v4.14.336 lolz-KSU-Next by sabamdarif<https://github.com/sabamdarif> for Redmi Note 8 Pro (begonia) | KernelSU Version: 12017
```

```bash
cp /home/username/android_kernel_realme_nashc/out/arch/arm64/boot/Image.gz .
zip -r9 ksunext-1.0.3.zip *
```

---

## If AnyKernel is Not Available

### Alternative Boot Image Editor Method

```bash
cd $HOME
```

**1. Download Boot Editor**

> Actual source: https://github.com/cfig/Android_boot_image_editor

```bash
wget https://github.com/sabamdarif/nashc-build-process/raw/refs/heads/main/patchs/others/boot_editor_v15_r1.zip
```

**2. Extract Boot Editor**

```bash
unzip boot_editor_v15_r1.zip
```

**3. Extract Your Current boot.img**

Extract your current boot.img and put it inside the `boot_editor_v15_r1` folder.

**4. Unpack the Boot Image**

```bash
./gradlew unpak
```

**5. Copy Kernel**

Go to the `android_kernel_realme_nashc` kernel source directory:

```bash
cd out/arch/arm64/boot
```

Copy the `Image.gz-dtb` to `boot_editor_v15_r1/build` folder and rename it to `kernel`.

**6. Pack the Boot Image**

```bash
./gradlew pack
```

This will create a file named `boot.img.signed` inside the `boot_editor_v15_r1` folder.

**7. Flash the New Boot Image**

Rename `boot.img.signed` to `new-boot.img` and flash it using:

```bash
fastboot flash boot new-boot.img
```
