OpenWrt Luci app For MySelf
---


The meaning of this repository:
---
1. Remove potential privacy risks
2. Remove unwanted features
3. [upstream](https://github.com/vernesong/OpenClash)

Openclash Dependency
---

* luci
* luci-base
* iptables
* dnsmasq-full
* coreutils
* coreutils-nohup
* bash
* curl
* ca-certificates
* ipset
* ip-full
* iptables-mod-tproxy
* iptables-mod-extra
* libcap
* libcap-bin
* ruby
* ruby-yaml
* kmod-tun(TUN mode)
* luci-compat(Luci-19.07)
* ip6tables-mod-nat(ipv6)


Openclash compile
---


OpenWrt [SDK](http://wiki.openwrt.org/doc/howto/obtain.firmware.sdk) compile
```bash
sudo apt-get install gawk libncurses5-dev libz-dev zlib1g-dev  git ccache
# SDK
tar xjf OpenWrt-SDK-ar71xx-for-linux-x86_64-gcc-4.8-linaro_uClibc-0.9.33.2.tar.bz2
cd OpenWrt-SDK-ar71xx-*

# Clone
mkdir package/luci-app-openclash
cd package/luci-app-openclash
git init
git remote add -f origin https://github.com/vernesong/OpenClash.git
git config core.sparsecheckout true
echo "luci-app-openclash" >> .git/info/sparse-checkout
git pull --depth 1 origin master
git branch --set-upstream-to=origin/master master

pushd luci-app-openclash/tools/po2lmo
make && sudo make install
popd

# compile

# back SDK
cd ../..
make package/luci-app-openclash/luci-app-openclash/compile V=99
another app
make package/luci-app-clash/compile V=99

# IPK location
./bin/ar71xx/packages/base/luci-app-openclash_0.39.7-beta_all.ipk
```

```bash
# pull code
cd package/luci-app-openclash/luci-app-openclash
git pull

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

