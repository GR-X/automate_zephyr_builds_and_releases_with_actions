# Zephyr Actions test

每次 push 自动编译 `hello_world`，也可在 Actions 页面手动运行。

构建使用官方 [action-zephyr-setup](https://github.com/zephyrproject-rtos/action-zephyr-setup)：

- Ubuntu 24.04、Python 3.12
- Zephyr SDK 1.0.1，工具链 `x86_64-zephyr-elf`
- Zephyr 版本固定在 `west.yml` 中，与本地 4.4.99 工作区的提交一致
- 目标 `qemu_x86`，编译不需要开发板

工作流将仓库检出到 `app/`，在外层创建 West 工作区，执行：

```sh
west build -b qemu_x86 app/hello_world -d build
```

构建成功后，打开 **Actions → Zephyr Hello World → 对应运行记录 → Artifacts**，
下载 `hello-world-qemu-x86`，其中包含：

- `zephyr.bin`
- `zephyr.hex`
- `zephyr.elf`

产物保留 14 天。这些文件针对 QEMU x86；用于实际硬件时，需要修改编译目标及对应工具链。

推送 `v` 开头的版本标签后，构建成功会自动创建 GitHub Release，并将上述三个文件
分别上传到 Release 的 Assets 中，可以直接下载，无需解压 Artifacts。

```sh
git tag v1.0.0
git push origin v1.0.0
```

标签名带 `-`（例如 `v0.0.0-test.20261101`）时，发布为测试版，不设为 Latest。
正式版标签（例如 `v1.0.0`）设为 Latest。

私有仓库的 Release 和附件仅对有仓库读取权限的用户开放。浏览器下载时需要登录
对应的 GitHub 账号；通过 API 下载时需要具有仓库访问权限的令牌。
