# 基于 GitHub Actions 的展讯 TWRP 自动化云编译

> **English version** is available below.

本仓库专门为 **展讯（Unisoc）** 芯片设备优化，集成了 [rtyutechstudio/unisoc-twrp-sourcecode_patch](https://github.com/rtyutechstudio/unisoc-twrp-sourcecode_patch) 的 DRM 显示补丁，解决了展讯设备 TWRP 启动黑屏/无显示的问题。  
感谢 rtyutechstudio 的贡献！

---

## 特性

- ✅ 一键云编译 TWRP Recovery
- ✅ 自动应用展讯 DRM 修复补丁
- ✅ 支持多种 Manifest 分支（twrp-11、twrp-12.1 等）
- ✅ 自动生成 GitHub Release 下载
- ✅ 可选的 SSH 密钥支持（用于私有仓库）

---

## 注意事项

1. GitHub Actions 服务**并非无限**，请勿滥用，建议仅用于已稳定设备树的自动化编译。
2. 修改前请确保仓库属于你自己，**Fork** 以提交代码，或使用 **Use this template**。
3. Issues 和 Pull Requests 可能**不会**得到回复，如有必要请通过邮件联系。
4. Debian/Ubuntu 已**移除** Python 2，若编译 Android 8.1 及以下版本，请使用 *Legacy* 工作流。
5. 请不要询问源码相关问题，例如：
   - No rule to make ...
   - Image ... out of size

---

## 致谢

- [minimal-manifest-twrp](https://github.com/minimal-manifest-twrp)
- [rtyutechstudio/unisoc-twrp-sourcecode_patch](https://github.com/rtyutechstudio/unisoc-twrp-sourcecode_patch)（展讯 DRM 补丁）
- 所有贡献者

---

## 参数说明

| 名称                 | 说明                                                      | 示例                                                                 |
| -------------------- | --------------------------------------------------------- | -------------------------------------------------------------------- |
| `MANIFEST_URL`       | 源码清单仓库地址                                          | `https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp.git` |
| `MANIFEST_BRANCH`    | 源码分支                                                  | `twrp-11`（展讯安卓10/11推荐）或 `twrp-12.1`                        |
| `DEVICE_TREE_URL`    | 设备树仓库地址                                            | `https://github.com/你的用户名/你的展讯设备树`                      |
| `DEVICE_TREE_BRANCH` | 设备树分支                                                | `main`                                                               |
| `DEVICE_PATH`        | 设备树在源码中的路径                                      | `device/unisoc/sp9863a`（示例）                                     |
| `COMMON_TREE_URL`    | 通用树仓库地址（若无则留空）                              | `https://github.com/...`                                             |
| `COMMON_PATH`        | 通用树路径（若无则留空）                                  | `device/unisoc/common`                                               |
| `DEVICE_NAME`        | 设备代号（将用于输出文件名）                              | `sp9863a`                                                            |
| `MAKEFILE_NAME`      | Makefile 名称（通常为 `omni_设备代号`）                   | `omni_sp9863a`                                                       |
| `BUILD_TARGET`       | 编译目标分区（`recovery`、`boot` 或 `vendorboot`）        | `recovery`                                                           |

---

## 如何使用

> 示例：假设你的用户名为 `JohnSmith`，设备为展讯 SC9863A。

#### 0. 如果你要提交代码，请点击本仓库右上角的 'Fork'

![image](https://user-images.githubusercontent.com/37921907/177914706-c92476c5-7e14-4fb3-be94-0c8a11dae874.png)

#### 1. 如果只是简单使用，请点击 'Use this template'

![image](https://github.com/azwhikaru/Action-TWRP-Builder/assets/37921907/fae6ce3c-bd4c-4bbe-8050-5dd29dff2522)

#### 2. 等待自动跳转后，你将看到自己的用户名

![image](https://user-images.githubusercontent.com/37921907/177915106-5bde6fc9-303c-479e-b290-22b48efd1e4e.png)

#### 3. （可选）修改工作流中的 [用户名和邮箱](https://github.com/CaptainThrowback/Action-Recovery-Builder/blob/main/.github/workflows/Recovery%20Build.yml#L100-L101)

---

## 设置 SSH 密钥（可选）

#### 4. 进入仓库 Settings → Deploy keys → Add deploy key

#### 5. 在 Android 设备上安装 [Termux](https://github.com/termux/termux-app/releases)

#### 6. 在 Termux 中安装 openssh 并生成密钥（**不要使用密码短语**）

> 注意：创建部署密钥时，将 `git@github.com:owner/repo.git` 或 `https://github.com/owner/repo` 作为注释填入。（提示：`ssh-keygen ... -C "git@github.com:owner/repo.git"`）  
> `owner` 为你的 GitHub 用户名。

```bash
pkg install openssh
ssh-keygen -t ed25519 -C "git@github.com:JohnSmith/Action-Unisoc-TWRP.git"
