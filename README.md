
# SKU 断货风险评估（Streamlit 一键部署）

根据出货表（英文名称+SKU+尺码配比）、库存表（SKU编码+当前库存）、以及日均销量/增长系数，评估在不同物流到货天数下（A=8 天 / B=13 天，可自定义）是否存在断货风险，并导出结果 CSV。

---

## 本地运行

```bash
pip install -r requirements.txt
streamlit run app.py
```

---

## 一键部署到 Streamlit Community Cloud

> 前置条件：拥有 GitHub 账号，并能创建公开或私有仓库。

### 方案 A：从本地上传文件到 GitHub

1. 在本地下载这三个文件：
   - `app.py`
   - `requirements.txt`
   - `README.md`（当前文件）

2. 在 GitHub 创建一个新仓库（例如 `stockout-risk-streamlit`），将以上文件上传到仓库根目录。

3. 打开 https://share.streamlit.io （Streamlit Community Cloud），使用 GitHub 登录，点击 **“New app”**：
   - **Repository**：选择你刚创建的仓库
   - **Branch**：`main`（或你的默认分支）
   - **Main file path**：`app.py`
   - 其余保持默认，点击 **Deploy**

4. 等待几分钟完成构建后，将生成一个公网访问链接，形如：  
   `https://share.streamlit.io/<username>/<repo>/<branch>/app.py`

### 方案 B：使用模板仓库（更方便团队复制）

1. 在 GitHub 新建一个空仓库后，点击 **“Add file → Upload files”**，把本项目的 3 个文件上传即可；
   或者，先在本地 `git init` 并 `git push` 到远端。
2. 后续团队成员可通过 **“Use this template”** 复制你的仓库，直接在 Streamlit Cloud 里 **New app** 指向他们自己的副本。

---

## 使用说明

- 左侧上传三张表：
  - 出货表（CSV）：至少包含 `英文名称, SKU, S数量, M数量, L数量`
  - 库存表（CSV）：至少包含 `SKU编码, 当前库存`（SKU编码形如 `NPF001-S`）
  - 销量增长表（XLSX/CSV）：至少包含 `Variation Name, 日均销量, 增长系数`
- 选择评估渠道（A=8 天或 B=13 天，或自定义），点击 **“🚀 计算 … 天断货风险”**。
- 页面会显示 KPI 与明细表，并支持一键导出 CSV。

> **计算逻辑简述**：
> - 假设“增长系数”为**周**口径，自动换算为**日**增长因子并累计到指定到货天数；
> - 按出货表中的 S/M/L 数量占比分配到各尺码需求；若无，则默认三等分（可在侧边栏勾选）；
> - 逐日扣减库存：任一尺码在到货日前库存跌破 0 视为有断货风险。

---

## 常见问题

- **Python 版本**：Streamlit Cloud 会自动识别环境，`requirements.txt` 已指定必要依赖；如需固定版本，可在 `requirements.txt` 中指定更精确的版本范围。
- **私有仓库**：可在 Streamlit Cloud 连接私有仓库（需要授权）；团队协作时可在组织内设置访问权限。
- **数据安全**：应用不会把上传的文件外传；如涉及敏感数据，建议在私有仓库 + 私有部署下使用。

---

## 目录结构

```
.
├── app.py
├── requirements.txt
└── README.md
```

如需拓展（可选）：
- 增加 `.streamlit/config.toml` 自定义主题；
- 在 CI/CD 中用 GitHub Actions 做 lint/测试等。

---

## 授权许可

此项目默认以 MIT License 分发（可自行更改）。
