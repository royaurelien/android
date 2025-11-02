# Android

> Pixel 7a (lynx)

### Install the platform-tools

If you haven’t previously installed `adb` and `fastboot`, you can [download them from Google](https://dl.google.com/android/repository/platform-tools-latest-linux.zip). Extract it running:

```bash
unzip platform-tools-latest-linux.zip -d ~
```

### Configure and initialize the LineageOS source repository

```bash


sudo apt install python3-protobuf



mkdir -p ~/bin
mkdir -p ~/android/lineage
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
git lfs install
git config --global trailer.changeid.key "Change-Id"

export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache


cd ~/android/lineage
repo init -u https://github.com/LineageOS/android.git -b ${LINEAGEOS_VERSION} --git-lfs --no-clone-bundle

repo sync -j4

```
### Preparing the build environment

```bash
cd ~/android/lineage
source build/envsetup.sh
croot
breakfast lynx
```

Get the release date from https://download.lineageos.org/devices/lynx/builds
```bash
export LINEAGEOS_VERSION=lineage-22.2
export RELEASE=20251031
wget https://mirrorbits.lineageos.org/full/lynx/${RELEASE}/${LINEAGEOS_VERSION}-20251031-nightly-lynx-signed.zip -o ~/android/${LINEAGEOS_VERSION}-${RELEASE}.zip
```

Follow the guide: extracting proprietary blobs from [payload-based OTAs](https://wiki.lineageos.org/extracting_blobs_from_zips#extracting-proprietary-blobs-from-payload-based-otas)

```bash

mkdir ~/android/system_dump/
cd ~/android/system_dump/

git clone https://github.com/LineageOS/android_prebuilts_extract-tools android/prebuilts/extract-tools
git clone https://github.com/LineageOS/android_tools_extract-utils android/tools/extract-utils
git clone https://github.com/LineageOS/android_system_update_engine android/system/update_engine

unzip ~/android/${LINEAGEOS_VERSION}-${RELEASE}.zip

./android/prebuilts/extract-tools/linux-x86/bin/ota_extractor --payload payload.bin

mkdir system/
sudo mount -o ro system.img system/
sudo mount -o ro vendor.img system/vendor/
sudo mount -o ro odm.img system/odm/
sudo mount -o ro product.img system/product/
sudo mount -o ro system_ext.img system/system_ext/
```

Move to the root directory of the sources of your device and run`extract-files.py` as follows:

```bash
breakfast lynx
cd device/google/lynx
./extract-files.py ~/android/system_dump/
sudo umount -R ~/android/system_dump/system/
rm -rf ~/android/system_dump/
```

