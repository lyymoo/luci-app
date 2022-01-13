OpenWrt For MySelf
---


依赖
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
* kmod-tun(TUN模式)
* luci-compat(Luci-19.07)
* ip6tables-mod-nat(ipv6)


编译
---


从 OpenWrt 的 [SDK](http://wiki.openwrt.org/doc/howto/obtain.firmware.sdk) 编译
```bash
# 解压下载好的 SDK
tar xjf OpenWrt-SDK-ar71xx-for-linux-x86_64-gcc-4.8-linaro_uClibc-0.9.33.2.tar.bz2
cd OpenWrt-SDK-ar71xx-*

# Clone 项目
mkdir package/luci-app-openclash
cd package/luci-app-openclash
git init
git remote add -f origin https://github.com/vernesong/OpenClash.git
git config core.sparsecheckout true
echo "luci-app-openclash" >> .git/info/sparse-checkout
git pull --depth 1 origin master
git branch --set-upstream-to=origin/master master

# 编译 po2lmo (如果有po2lmo可跳过)
pushd luci-app-openclash/tools/po2lmo
make && sudo make install
popd

# 开始编译

# 先回退到SDK主目录
cd ../..
make package/luci-app-openclash/luci-app-openclash/compile V=99

# IPK文件位置
./bin/ar71xx/packages/base/luci-app-openclash_0.39.7-beta_all.ipk
```

```bash
# 同步源码
cd package/luci-app-openclash/luci-app-openclash
git pull

# 您也可以直接拷贝 `luci-app-openclash` 文件夹至其他 `OpenWrt` 项目的 `Package` 目录下随固件编译

make menuconfig
# 选择要编译的包 LuCI -> Applications -> luci-app-openclash

```
