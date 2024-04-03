OpenWrt Luci app For MySelf
---


The meaning of this repository:
---
1. Remove potential privacy risks
2. Remove unwanted features
3. [clash-upstream1](https://github.com/vernesong/OpenClash/releases)
4. [clash-upstream2](https://github.com/frainzy1477/luci-app-clash)
5. [xray-upstream3](https://github.com/yichya/luci-app-xray)
5. [xray-upstream4](https://github.com/BI7PRK/luci-app-xray)
6. [trojan app](https://github.com/frainzy1477/luci-app-trojan)
7. [lean's openwrt-needbuild](https://github.com/coolsnowwolf/lede)
8. [Lienol's openwrt-needbuild](https://github.com/Lienol/openwrt)
9. [lean app helloworld-needbuild](https://github.com/fw876/helloworld)
10. [Lienol app passwall-needbuild](https://github.com/xiaorouji/openwrt-passwall)

Openclash Dependency
---

* luci
* luci-base
* dnsmasq-full
* coreutils
* coreutils-nohup
* bash
* curl
* ca-certificates
* ipset
* ip-full
* libcap
* libcap-bin
* ruby
* ruby-yaml
* unzip
* iptables(iptables)
* kmod-ipt-nat(iptables)
* iptables-mod-tproxy(iptables)
* iptables-mod-extra(iptables)
* kmod-tun(TUN模式)
* luci-compat(Luci >= 19.07)
* ip6tables-mod-nat(iptables-ipv6)
* kmod-inet-diag(PROCESS-NAME)
* kmod-nft-tproxy(Firewall4)

```bash
opkg update
opkg install luci luci-base dnsmasq-full coreutils coreutils-nohup bash \
curl ca-certificates ipset ip-full libcap libcap-bin ruby ruby-yaml unzip \
iptables kmod-ipt-nat iptables-mod-tproxy iptables-mod-extra kmod-tun \
luci-compat ip6tables-mod-nat kmod-inet-diag kmod-nft-tproxy

# install openclash
opkg install --nodeps /tmp/luci-app-openclash_xxx.ipk
```


Openclash compile
---


OpenWrt [SDK](http://wiki.openwrt.org/doc/howto/obtain.firmware.sdk) compile and
[SDK Download Link](https://archive.openwrt.org/snapshots/trunk/ar71xx/generic/OpenWrt-SDK-ar71xx-generic_gcc-5.3.0_musl-1.1.16.Linux-x86_64.tar.bz2)


- compile OS can use Debian 11 or Ubuntu LTS

```bash
# install system depandence
sudo apt update -y
sudo apt full-upgrade -y
sudo apt install -y ack antlr3 aria2 asciidoc autoconf automake autopoint binutils bison build-essential \
bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pip libpython3-dev qemu-utils \
rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
```

- Compile with Openwrt SDK
```bash
# system install depandence
sudo apt-get install gawk libncurses5-dev libz-dev zlib1g-dev  git ccache

# unzip SDK
tar xjf OpenWrt-SDK-ar71xx-for-linux-x86_64-gcc-4.8-linaro_uClibc-0.9.33.2.tar.bz2
cd OpenWrt-SDK-ar71xx-*

# Clone this project
mkdir package/luci-app-openclash
cd package/luci-app-openclash
git init
git remote add -f origin https://github.com/lyymoo/luci-app.git
git config core.sparsecheckout true
echo "luci-app-openclash" >> .git/info/sparse-checkout
git pull --depth 1 origin master
git branch --set-upstream-to=origin/master master

# compile po2lmo
pushd luci-app-openclash/tools/po2lmo
make && sudo make install
popd

# compile

# back SDK
cd ../..
make package/luci-app-openclash/luci-app-openclash/compile V=99

# IPK location
./bin/ar71xx/packages/base/luci-app-openclash_0.39.7-beta_all.ipk
```

- Compile with Openwrt source code

```bash
# pull code
cd package/luci-app-openclash/luci-app-openclash
git pull

# You can also directly copy the `luci-app-openclash` folder to
# the `Package` directory of other `OpenWrt` projects to
# compile with the firmware

# select LuCI -> Applications -> luci-app-openclash
make menuconfig
```

Soft link:
---


development environment soft link on openwrt x86 virtual machine
```bash
ln -s ~/luci-app/luci-app-openclash/luasrc/openclash.lua /usr/lib/lua/luci/
ln -s ~/luci-app/luci-app-openclash/luasrc/controller/openclash.lua /usr/lib/lua/luci/controller/
ln -s ~/luci-app/luci-app-openclash/luasrc/model/cbi/openclash /usr/lib/lua/luci/model/cbi/
ln -s /root/luci-app/luci-app-openclash/luasrc/view/openclash /usr/lib/lua/luci/view/

ln -s /root/luci-app/luci-app-openclash/root/etc/config/openclash /etc/config/openclash
ln -s /root/luci-app/luci-app-openclash/root/etc/init.d/openclash /etc/init.d/openclash
ln -s /root/luci-app/luci-app-openclash/root/etc/uci-defaults/luci-openclash /etc/uci-defaults/luci-openclash

ln -s /root/luci-app/luci-app-openclash/root/usr/share/rpcd/acl.d/luci-app-openclash.json /usr/share/rpcd/acl.d
ln -s /root/luci-app/luci-app-openclash/root/usr/share/openclash /usr/share/

ln -s /root/luci-app/luci-app-openclash/root/www/luci-static/resources/openclash /www/luci-static/resources/
```

About OS
---

My device has hiwifi-hc5962 respberry-pi-4b and NanoPi-R2S etc.  

official openwrt:  
https://downloads.openwrt.org/releases/22.03.2/targets/ramips/mt7621/openwrt-22.03.2-ramips-mt7621-hiwifi_hc5962-squashfs-factory.bin  

official breed:  
https://breed.hackpascal.net/breed-mt7621-hiwifi-hc5962.bin  

info:  
1. use ssh teriminal to upgrade breed  
  > mtd -r write /tmp/breed-mt7621-hiwifi-hc5962.bin u-boot  
2. use breed web console (192.168.1.1) to upgrade openwrt  
  > Unplug the router, press and hold the reset button, then plug in  
  > the power, press and hold for about 5 seconds, the indicator light  
  > will flash for a while, at this time, the computer will re-acquire  
  > the ip address of the 192.168.1.0 network segment, and the browser  
  > will enter the breed gateway address 12.168.1.1 to enter  
  > the breed console. 

#### Version Update

```bash
# Global Settings - Version Update
luci-app-openclash/luasrc/view/openclash/update.htm
Core path:/etc/openclash/core/clash
Core path:/etc/openclash/core/clash_tun
Core path:/etc/openclash/core/clash_meta
Client Update
# Check And Update
core_update(this,'Dev')
core_update(this,'TUN')
core_update(this,'Meta')
op_update(this) -> forbbiden update
# Download
ma_core_update(this,'Dev')
ma_core_update(this,'TUN')
ma_core_update(this,'Meta')
ma_op_update(this) -> forbbiden download

```

