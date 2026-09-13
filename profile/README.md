# ReconCheck

**把一叠单据交叉核对，告诉你哪几处对不上、差多少钱、原文在哪一行。**
**Cross-document verification: what doesn't match, by how much, and where in the original file.**

![CI](https://github.com/ReconCheck/core/actions/workflows/ci.yml/badge.svg) · Python 3.10+ (Linux / macOS / Windows) · Apache-2.0

---

## 为什么存在

月底对账靠人肉：把发票、采购订单、送货单逐行比，判断每处差异是**真错误还是正常尾差**（汇率、四舍五入、单位换算），写报告交给财务。慢、枯燥，漏一次就是钱。

ERP 不管这件事——它记录发生了什么，不检查这些单据之间是否**自洽**。ReconCheck 补上这一环，而且：

- **只读**：不改数据、不碰业务系统
- **可审计**：每条判定带双侧坐标 `cell://`，点回原文那一格
- **可内网部署**：默认不联网，数据不出你的网络

## 能做什么（当前构建）

| 层 | 能力 |
|---|---|
| **解析** | CSV / TSV / TXT、XLSX/XLSM（只读流式）、**PDF 文本层**（线框表格 + 无框版面兜底）；编码 UTF-8 / GB18030 / UTF-16 / Latin-1 + 乱码拦截 |
| **对齐** | 按业务键精确匹配；料号/实体归一化（`A-012` → `a12`）、单位保留（`5000 g`） |
| **判定** | YAML 差分规则：相对+绝对容差，例外 **单位换算、四舍五入、日期容差、忽略大小写文本相等**；内置 auto 规则兜底且不重复报 |
| **证据** | 每条 finding 双侧坐标 + `cell://` 链接（PDF 带页码），报告 JSON 即契约 |
| **三方核对** | `compare3`：采购订单 + 送货单 + 发票，逐对报告 + **consensus/outlier**（"哪一方偏离，如发票 vs 采购单+送货单"） |
| **作业层** | Web API + 前端：批处理自动配对、文档库、报告归档、企业数据源（`file`/`records` + probe/list/fetch）、点击证据高亮原文 |
| **安全/运维** | 可选 `X-API-Key` 鉴权（未配启动告警）、64MB 上传 / 50MB 拉取上限、TTL 归档清理、重启重放、id 白名单防路径穿越 |

## 工作原理

```
 Parse  →  Align  →  Judge  →  Cite
 读文件    对上行    判差异    指回原文
```

输出不是风险分，而是**一条条可复核的具体 claim**。

## 五分钟上手

```bash
git clone https://github.com/ReconCheck/core && cd core
python -m venv .venv && source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -e ".[web,pdf]"

reconcheck compare examples/po.csv examples/invoice.csv --match-on 料号
reconcheck compare3 examples/po.csv examples/dn.csv examples/invoice.csv --match-on 料号
reconcheck-api      # → http://127.0.0.1:8765
```

## 仓库

| 仓库 | 是什么 | 许可 |
|---|---|---|
| [core](https://github.com/ReconCheck/core) | 引擎：解析 / 对齐 / 判定规则 / 证据链 / Web API / 前端 | Apache-2.0 |
| [docs](https://github.com/ReconCheck/docs) | 文档：使用指南、能力清单、规则编写、部署、ERP 接入（中英双语，13 篇） | — |
| [rules](https://github.com/ReconCheck/rules) | 行业差异化判定规则包（按组织授权） | 私有 |

## 参与

- **最能帮到我们的事 ①**：**真实单据样本对**——能弄坏现有工具的那种（歪表、无框多栏 PDF、合并单元格 XLSX、怪编码 CSV）。脱敏后开 issue。版面还原和实体对齐最缺样本，你"破坏"得越狠我们越高兴。
- **报 bug / 提需求 ②**：用 [issue 模板](https://github.com/ReconCheck/.github/tree/main/ISSUE_TEMPLATE)（bug 报告请附可复现的最小单据对）。
- **贡献政策 ③**：[CONTRIBUTING.md](https://github.com/ReconCheck/.github/blob/main/CONTRIBUTING.md) —— 接口仍在演进，v1 冻结前以 issue 协作优先。

## Roadmap

- ✅ 已实现：表格 + PDF 文本层解析，两方/三方核对，规则 + 四类例外，批处理/文档库/数据源，安全与运维加固
- 🧭 设计已定：**LLM 参与**（对齐消歧 + 差异解释；接口在 `reconcheck/llm` 预留，opt-in、OpenAI 兼容、失败回退确定性结果）
- 🕔 下一步：扫描件 OCR、实体解析、规则格式 v1 冻结

---

### 为什么引擎开源、规则库不开

核对的难点不在模型，在**版面还原、实体对齐、差异判定规则**——三件事极度依赖真实世界的单据样本。引擎放在这里公开迭代，样本和规则才能进来。

规则是商业部分：同一处差异，在快消是汇率尾差可忽略，在贵金属是重大异常。这部分不公开，按组织授权。

**Docs 双语入口**：[中文](https://github.com/ReconCheck/docs/blob/main/README.md) · [English](https://github.com/ReconCheck/docs/blob/main/README.md)