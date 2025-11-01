# android
Android

### Install the platform-tools[](https://wiki.lineageos.org/devices/lynx/build/#install-the-platform-tools)

If you haven’t previously installed `adb` and `fastboot`, you can [download them from Google](https://dl.google.com/android/repository/platform-tools-latest-linux.zip). Extract it running:

```bash
unzip platform-tools-latest-linux.zip -d ~
```

---

```bash
mkdir -p ~/bin
mkdir -p ~/android/lineage
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
git lfs install
git config --global trailer.changeid.key "Change-Id"

```