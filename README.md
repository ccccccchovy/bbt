# BBT — 本地语音转文字（开源）

**开源交付、非商业 MSI 必达。**

本仓库是 **GitHub 开源项目**（不做应用商店 / 商业上架）。正式用户面默认是：

1. **源码仓**（本 README / `LICENSE` / `NOTICE`）
2. **GitHub Release 附件**：正式便携 zip（**embed · Python 3.12 / cp312**；口径见 PACK-RELEASE-FORMAL）

**MSI / Authenticode 签名 = 可选、非默认**；无证书或未启用签名时诚实写「未启用」——**禁止**把已签 MSI 当成开源必达或默认下载入口。

| 项 | 链接 |
|----|------|
| 根许可 | [`LICENSE`](LICENSE)（**SPDX: Apache-2.0**） |
| 第三方归属 | [`NOTICE`](NOTICE)（不吞掉 upstream 版权） |
| 定案 | [`docs/DOC-OSS-GITHUB-开源GitHub发布.md`](docs/DOC-OSS-GITHUB-开源GitHub发布.md)（ADR-045 / SRS R43） |
| 本卡实现 | [`docs/OSS-GITHUB-CORE-开源GitHub实现.md`](docs/OSS-GITHUB-CORE-开源GitHub实现.md) |
| R1 验收 | [`docs/OSS-GITHUB-R1-验收收口.md`](docs/OSS-GITHUB-R1-验收收口.md)（有条件通过；未上传 Release 附件） |
| 壳开发说明 | [`poc/app_shell/README.md`](poc/app_shell/README.md) |

---

## 安装 / 运行（两条入口，勿混淆）

### A. 源码开发入口（贡献者 / 本机调试）

适合改代码、跑测试。**不是**「唯一正式用户包」。

```text
# 建议使用 Python 3.12 虚拟环境后安装依赖（以仓库 requirements 为准）
python -m poc.app_shell.run
```

更多开关与模块落点见 `poc/app_shell/README.md`。开发入口 **不会**自动装 Interception 驱动、**不会**预装全部 ASR 权重、**不会**默认带 CUDA。

### B. Release 便携 zip（正式用户面）

从 **GitHub Release** 下载正式便携 zip（用户面 `--audience user --embed-venv`）。

- **强制 embed**：包内自带解释器树；**Python 3.12 / cp312**（与公网 CUDA wheel ABI 对齐；ADR-044）
- 解压后按包内 `README-PORTABLE.md` / `run_bbt.bat` 启动
- 精确字节与 sha256 以 **PACK-RELEASE-FORMAL** 证据为准（见下方「Release 附件」）；大 zip **不进 Git**

```text
# 打包机示例（不改默认行为；本卡不上传数 GB）
python packaging/build_portable_pack.py --flavor unified --audience user \
  --embed-venv path\to\cp312-venv --version 0.1.0
```

---

## 权重与 `pack_ready`

- 默认 **不**宣称预装全部 ASR 权重。
- 听写就绪须对应模型通过 **`pack_ready` / `verify_pack`**（残缺包禁止标「已就绪」）。
- 大权重按需下载（产品内下载器或文档指引）；**不进 Git**。

---

## Interception（知情安装 · 禁静默）

出字注入依赖 **Interception** 驱动：

- 先读 `third_party/interception/知情同意.html`
- 应用内「关于 → Interception 知情安装…」确认后，以**管理员 / UAC** 运行安装器
- **禁止**静默 / 解压后自动装驱动
- **解压便携包 ≠ 驱动已进系统**（通常还需重启）

---

## CUDA 可选（数 GB）

- CUDA / Ultra 为 **可选组件**（常见再大 **数 GB**）；主体默认 **不**默塞
- 首次可联网拉公网 wheel（方案 B）或密封旁路；装完 **≠** 假 L1 / Ultra ready
- NVIDIA 驱动 **≥551** 另计，且须**知情安装**（禁静默）
- **禁止**把**事后 pip**（含 `pip install`）当作**唯一**产品安装路径（产品路径用自有下载/校验/落盘或预带）

---

## 禁假开箱（下载 zip ≠ 可听写）

请分条理解：

| 步骤 | 不等于 |
|------|--------|
| 下载 / 解压 Release zip | 可听写 |
| 有壳 / 有 embed 运行时 | 权重 `pack_ready` |
| 权重已落盘 | Interception 驱动已装进系统 |
| Interception 已装 | NVIDIA 驱动 ≥551 / CUDA 已装 |
| CUDA 组件已装 | L1 / Ultra ready |

**假开箱禁止**：解压即宣称可听写 / 已加速 / 驱动已就绪。

---

## Release 附件（正式 zip + sha256）

本卡提供**说明脚本/模板**，指导如何挂正式 zip 并公布 sha256（复用 FORMAL 已钉证据）。**本卡不实际上传数 GB 到 GitHub。**

```text
# 生成 Release 正文（读 artifacts/pack_release_formal.json；不上传）
python scripts/oss_github_release_notes.py --print
python scripts/oss_github_release_notes.py --out artifacts/oss_github_release_body.md
```

模板：`docs/templates/OSS-GITHUB-Release附件说明.md`  
证据卡：`docs/PACK-RELEASE-FORMAL-正式包与签名MSI.md`（`body_bytes` / `zip_bytes` / `sha256`）

| 诚实口径 | 说明 |
|----------|------|
| 正式便携 zip | FORMAL 已钉；挂 Release 时附同 sha |
| MSI / 签名 | **可选非默认**；未启用签名 → **不得**假称发版就绪 MSI |
| 数 GB CUDA / 权重 | **不进 Git**；可选另挂 Release / 外链 / 产品内下载 |

---

## 许可与归属

- 本仓库根许可：**Apache-2.0**（`LICENSE`）
- 第三方各自保留上游版权与许可（`NOTICE` + `third_party/*/LICENSE`）
- 兼容 `third_party/FireRedASR`（Apache-2.0）；根许可 **不**吞掉上游声明

---

## 开发与 CI

- 护栏 / 壳与布局包为主；**不以**全量数 GB 下载/上传为绿门硬门槛
- 推荐：`python -m pytest tests/test_oss_github_r1.py tests/test_oss_github_core.py tests/test_doc_oss_github.py -q`

回滚 tag：`pre-oss-github`。工单：**§6.30**（DOC/CORE/R1 Done；有条件）。**不**抬 SRS-15。
