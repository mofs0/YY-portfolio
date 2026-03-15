# 用Python做面板数据回归：从入门到发表

## 为什么选Python？

- ✅ 免费开源，无需Stata许可证
- ✅ 可复现性强，代码数据图表一体化
- ✅ 与机器学习生态无缝集成
- ✅ 就业市场需求旺盛

---

## 完整代码示例

### 1. 数据准备

```python
import pandas as pd
df = pd.read_csv('data.csv')
df = df.dropna(subset=['y', 'x1', 'x2'])
```

### 2. 面板回归（固定效应）

```python
from linearmodels import PanelOLS

df_panel = df.set_index(['firm_id', 'year'])
mod = PanelOLS(df_panel['y'], df_panel[['x1', 'x2', 'x3']],
               entity_effects=True, time_effects=True)
res = mod.fit(cov_type='clustered', cluster_entity=True)
print(res.summary)
```

### 3. 导出LaTeX表格

```python
latex = res.summary.tables[1].as_latex_tabular()
with open('table.tex', 'w') as f:
    f.write(latex)
```

---

## 常见问题

**Q: 如何处理缺失值？**
A: 优先删除，仅在完全随机缺失时考虑填充。

**Q: VIF>10怎么办？**
A: 删除高VIF变量或使用主成分回归。

**Q: 如何做稳健性检验？**
A: 替换核心变量、剔除特殊样本、更换模型。

---

**需要定制分析？** 私信交流！
