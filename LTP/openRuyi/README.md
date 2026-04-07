# 在 openRuyi 上运行 LTP

openRuyi 默认打包了 LTP，直接通过 `dnf` 安装即可。

默认安装路径位于 `/opt`。

## 安装

```shell
sudo dnf install -y ltp jq
```

## 运行

> [!Warning]
> LTP 已正式移除 `runltp`，相关依赖也在清理中。
> 
> 目前打包的版本可能还有 `runltp`，但下个版本应该就没了。使用 `kirk` 替代。

> [!Tip]
> `kirk` 没有一键执行所有测试用例的能力。
>
> 需要逐个指定测试套。

运行所有用例：

```shell
#建议使用 Root 权限
sudo -i
cd /opt/ltp
#运行全部用例
./kirk -f `ls runtest` -o report.json
#移除了 net, input, s390x 
./kirk -f can capability commands containers controllers cpuhotplug crashme crypto \
 cve dio dma_thread_diotest fcntl-locktests fs fs_bind fs_perms_simple fs_readonly \
 hugetlb hyperthreading ima irq kernel_misc kvm ltp-aio-stress ltp-aiodio.part1 \
 ltp-aiodio.part2 ltp-aiodio.part3 ltp-aiodio.part4 math mm net.features nptl \
 numa power_management_tests power_management_tests_exclusive pty sched \
 scsi_debug.part1 smack smoketest staging syscalls syscalls-ipc tpm_tools \
 tracing uevent watchqueue -o report.json
```

强烈建议后台挂一个 `dmesg` 以方便后续失败用例分析。

如需并行，添加 `-w $THREAD` 参数即可。Kirk 会自动并行化运行测试。

## 结果处理

过滤 `fail` 用例（仅提取名称）：

```shell
jq '.results[] | select(.test.result == "fail") | .test_fqn' report.json
```

过滤 `fail` 用例（完整 json）：

```shell
jq '[.results[] | select(.test.result == "fail")]' report.json > failed.json
```

## Tips

- 不要尝试在 TH1520 平台运行所有关核用例（cpuhotplug, cpuset_hotplug, 等等）。
    - CPU0 是不能关的，关了会出问题。
- LTP 设计上就是为了找到问题，或者换句话说，运行 LTP 就是会造成问题的。
    - e.g. Kernel Taint, 数据丢失，各种崩溃。
    - 不要在生产环境/正在使用的机器上运行。如果你这么干了，后果自负。

## 参考

- 使用 Kirk: https://linux-test-project.readthedocs.io/en/latest/users/quick_start.html
