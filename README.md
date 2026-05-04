🚀 Mihomo 内核自动编译与压缩工作流
本项目内置了基于 GitHub Actions 的全自动构建脚本 (.github/workflows/build-meta.yml)，用于自动拉取 Mihomo 源码、交叉编译、极限压缩并推送到本仓库，专为 OpenWrt 路由器和 OpenClash 插件量身定制。

✨ 核心特性
⚡ 全自动跨平台编译：利用 GitHub Actions 的矩阵（Matrix）并发构建能力，同时编译适用于多种 OpenWrt 路由器架构的二进制文件，包含：

amd64 (x86_64 软路由)

arm64 (ARMv8，如树莓派、R2S/R4S)

armv7 (常见 ARM 硬路由)

mipsle-softfloat (常见 MIPS 老款路由器，如斐讯、MTK 设备)

📦 极限体积压缩 (UPX)：针对路由器 Flash 闪存空间有限的痛点，构建过程中集成了 UPX 壳压缩。默认开启 --lzma --best 极限压缩算法（对 MIPS 架构做了 --best 兼容性退让），使动辄十几兆的内核体积断崖式缩小至 5~8MB。

100% 适配 OpenClash：

内部二进制文件统一命名为 clash，外部打包为标准的 .tar.gz 格式。

完全符合 OpenClash 面板“内核更新”功能的识别标准，下载后可直接在 LuCI 界面一键上传替换，无需通过 SSH 手动解压配置。

🤖 自动化回推仓库：分为“并行构建”与“合并推送”两个阶段。编译完成后，会自动在当前仓库根目录创建 mihomo/ 文件夹，并将所有架构的压缩包静默提交并 Push，方便随时下载取用。

⚙️ 工作流程说明
手动触发：在仓库的 Actions 页面点击 Run workflow 启动。

拉取源码：自动从 MetaCubeX/mihomo 的 Meta 分支拉取最新代码。

环境准备：配置 Go 1.22 编译环境并安装 UPX 压缩工具。

构建与打包：按架构交叉编译 -> UPX 压缩 -> 打包为 clash-{arch}.tar.gz -> 暂存为 Artifacts。

提交与发布：汇总所有 Artifacts，利用 GitHub 机器人账号自动 Commit 并 Push 到本仓库的 mihomo 目录下。# core
