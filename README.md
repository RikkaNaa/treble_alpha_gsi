# treble alphadroid gsi for diting
diting最新的AlphaDroid无法选择刷新率，Display Mode只有60Hz，所以，别跑  
### To get started with building AlphaDroid GSI,
you'll need to get familiar with [Git and Repo](https://source.android.com/source/using-repo.html) as well as [How to build a GSI](https://github.com/phhusson/treble_experimentations/wiki/How-to-build-a-GSI%3F).v


### Install dependencies

```
sudo apt-get install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev xattr openjdk-11-jdk jq android-sdk-libsparse-utils python3
```

### Set git identity

```
git config --global user.email "xxxx@xxx.com"
git config --global user.name "xxxx"
```

### Create repo

```
mkdir ~/bin
PATH=~/bin:$PATH
curl https://mirrors.tuna.tsinghua.edu.cn/git/git-repo > ~/bin/repo
chmod a+x ~/bin/repo
```

If you want to use it often, you can add export PATH=~/.bin:$PATH to the PATH environment variable in the shell startup file (for example, the Bash startup file is ~/.bashrc)


### Create the directories

As a first step, you'll have to create and enter a folder with the appropriate name.
To do that, run these commands:

```bash
mkdir alpha
cd alpha
```

### To initialize your local repository, run this command:

```bash
repo init -u https://github.com/alphadroid-project/manifest -b alpha-13 --git-lfs
```
### Want to save some space ? Then use this:

```bash
repo init --depth=1 -u https://github.com/alphadroid-project/manifest -b alpha-13 --git-lfs
```

### Clone this repo:

```bash
git clone https://github.com/Rikkaawa/treble_alpha_gsi.git -b 13
```

### Preparing local manifest:

```bash
mkdir -p .repo/local_manifests
cp treble_alpha_gsi/manifest.xml .repo/local_manifests/alpha.xml
```

### Afterwards, sync the source by running this command:

```bash
repo sync -c --no-clone-bundle --optimized-fetch --prune --force-sync -j4
```

### After syncing, apply the patches:

Copy the patches folder to rom folder and in rom folder

```
cp treble_alpha_gsi/* . -r
bash patches/apply-patches.sh .
```

## Generating Rom Makefile

 In rom folder,
 
 ```
 cd device/phh/treble
 bash generate.sh ./lineage.mk
 ```

### Turn on caching to speed up build

You can speed up subsequent builds by adding these lines to your ~/.bashrc OR ~/.zshrc file:

```
export USE_CCACHE=1
export CCACHE_COMPRESS=1
export CCACHE_MAXSIZE=50G # 50 GB
```

## Compilation 

In rom folder,

 ```
 . build/envsetup.sh
 ccache -M 50G -F 0
 lunch treble_arm64_bvN-userdebug 
 make systemimage -j$(nproc --all)
 ```

## Compress

After compilation,
If you want to compress the build.
In rom folder,

```
cd out/target/product/tdgsi_arm64_ab
xz -z -v -T0 system.img 
```


## Troubleshoot
 
If you face any conflicts while applying patches, apply the patch manually.



## Notes
- If bluetooth calls or bluetooth media do not work well for you, make sure you have the "Use System Wide BT HAL" checkbox enabled on the Misc page of Treble App. If not, enable and reboot.
- If you have a non-Samsung device with a Qualcomm chipset and VoLTE isn't working with the default IMS package provided by Treble App, try installing this [alternative IMS package](https://treble.phh.me/stable/ims-caf-s.apk).
- Only 60Hz on diting.



## Credits
These people have helped this project in some way or another, so they should be the ones who receive all the credit:
- [KoysX](https://github.com/KoysX)
- [AlpahaDroid](https://github.com/AlphaDroid-Project)
- [Phhusson](https://github.com/phhusson)
- [Naz664](https://github.com/naz664)
- [AndyYan](https://github.com/AndyCGYan)
- [Ponces](https://github.com/ponces)
- [ChonDoit](https://github.com/ChonDoit)
- [xiaoleGun](https://github.com/xiaoleGun)
