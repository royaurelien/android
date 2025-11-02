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

**Define the variables:**
```bash
export CODENAME=lynx
export LINEAGEOS_VERSION=lineage-22.2
export RELEASE=20251031
```


```bash
cd ~/android/lineage
source build/envsetup.sh
croot
breakfast lynx
```

Get the release date from https://download.lineageos.org/devices/lynx/builds
```bash
curl -L -o ~/android/${LINEAGEOS_VERSION}-${RELEASE}.zip "https://mirrorbits.lineageos.org/full/${CODENAME}/${RELEASE}/${LINEAGEOS_VERSION}-${RELEASE}-nightly-${CODENAME}-signed.zip"
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
sudo mount -o rw system.img system/
sudo mount -o rw product.img system/product/
sudo mount -o rw system_ext.img system/system_ext/
```

Move to the root directory of the sources of your device and run`extract-files.py` as follows:

```bash

cd device/google/lynx
./extract-files.py ~/android/system_dump/
sudo umount -R ~/android/system_dump/system/
rm -rf ~/android/system_dump/

breakfast lynx
```


https://github.com/TheMuppets/proprietary_vendor_google_lynx/tree/lineage-22.2

From https://luk1337.github.io/muppets/

```bash
lineage/scripts/add-repo/add-repo.py add-project \
    --name TheMuppets/proprietary_vendor_google_lynx \
    --path vendor/google/lynx \
    --remote github \
    --file .repo/local_manifests/muppets.xml
rm -rf vendor/google/lynx
repo sync vendor/google/lynx
```
