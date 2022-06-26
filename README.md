OpenWrt For MySelf
---


Dependency
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


compile
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

# IPK文件位置
./bin/ar71xx/packages/base/luci-app-openclash_0.39.7-beta_all.ipk
```

```bash
# pull code
cd package/luci-app-openclash/luci-app-openclash
git pull

make menuconfig
```

# 开发环境构建
```
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
