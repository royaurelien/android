# Android

### Install the platform-tools

If you haven’t previously installed `adb` and `fastboot`, you can [download them from Google](https://dl.google.com/android/repository/platform-tools-latest-linux.zip). Extract it running:

```bash
unzip platform-tools-latest-linux.zip -d ~
```

### Configure and initialize the LineageOS source repository

```bash
mkdir -p ~/bin
mkdir -p ~/android/lineage
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
git lfs install
git config --global trailer.changeid.key "Change-Id"

export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache


cd ~/android/lineage
repo init -u https://github.com/LineageOS/android.git -b lineage-22.2 --git-lfs --no-clone-bundle

repo sync -j4

```
### Preparing the build environment

```bash
cd ~/android/lineage
source build/envsetup.sh
croot
```