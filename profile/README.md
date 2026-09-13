# ReconCheck

**Cross-document verification.** Point it at messy invoices, purchase orders and delivery notes — it tells you what doesn't match, by how much, and where in the original file.

把一叠格式混乱的单据拖进去，告诉你哪几处对不上、差多少钱、原文在哪一行。

The files are never clean — scans, skewed tables, multi-column PDFs, Excel exports with merged cells. Existing tools read them one at a time. ReconCheck reads them **against each other**, and every finding it produces can be clicked back to the exact spot it came from.

It is read-only by design. It never writes to your data and never touches your business systems.

---

### Repositories

| Repository | What it is | License |
|---|---|---|
| [core](https://github.com/ReconCheck/core) | The engine — parsing, alignment, differential rules, evidence chain | Apache 2.0 |
| [docs](https://github.com/ReconCheck/docs) | Documentation | — |
| [rules](https://github.com/ReconCheck/rules) | Industry-specific differential rule packs | Private |

---

### 关于本组织

引擎开源，规则库不开源。核对的难点不在模型，而在**版面还原、实体对齐、差异判定规则**——这三件事极度依赖真实世界的单据样本，所以引擎放在这里公开迭代。

规则才是商业部分：同一处差异，在不同行业、不同客户那里的结论可能完全相反。这部分按组织授权。
