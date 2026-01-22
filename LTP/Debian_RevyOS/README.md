# 在 RevyOS 上运行 LTP

## 依赖安装&编译

```shell
sudo apt update; sudo apt upgrade -y
sudo apt install -y autoconf automake build-essential debhelper devscripts clang curl jq gcc git iproute2 libc6-dev libtirpc-dev linux-libc-dev lsb-release pkg-config acl-dev libacl1-dev libaio-dev libcap-dev libkeyutils-dev libnuma-dev libmnl-dev libselinux1-dev libsepol-dev libssl-dev
git clone https://github.com/linux-test-project/ltp
#wget https://github.com/linux-test-project/ltp/releases/download/20250930/ltp-full-20250930.tar.xz
#tar xvf ltp-full-20250930.tar.xz
cd ltp
make autotools
./configure
make -j$(nproc)
sudo make install
```

Ref: https://github.com/linux-test-project/ltp/blob/master/ci/debian.sh

## 运行

运行所有用例：

```shell
#建议使用 Root 权限
sudo -i
cd /opt/ltp
./runltp
#或使用 Kirk: https://linux-test-project.readthedocs.io/en/latest/users/quick_start.html
```

强烈建议后台挂一个 `dmesg` 以方便后续失败用例分析。

## Tips

- 不要尝试在 TH1520 平台运行所有关核用例（cpuhotplug, cpuset_hotplug, 等等）。
    - CPU0 是不能关的，关了会出问题。
- LTP 设计上就是为了找到问题，或者换句话说，运行 LTP 就是会造成问题的。
    - e.g. Kernel Taint, 数据丢失，各种崩溃。
    - 不要在生产环境/正在使用的机器上运行。如果你这么干了，后果自负。
