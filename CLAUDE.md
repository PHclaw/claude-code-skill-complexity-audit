# complexity-audit

代码复杂度评分与圈复杂度分析。

## 命令

```
/audit-complexity <目录或文件>
```

## 分析工具

| 语言 | 工具 | 安装 |
|------|------|------|
| Python | `radon cc -a -m 10` | `pip install radon` |
| JS/TS | ESLint `complexity` rule | `eslint --rule 'complexity: error'` |
| Go | `gocyclo -top 20` | `go install github.com/fzipp/gocyclo@latest` |

## 复杂度评级

| CC | 评级 | 建议 |
|----|------|------|
| 1-10 | 简单 | ✅ 可接受 |
| 11-20 | 中等 | ⚠️ 建议拆分 |
| 21-50 | 复杂 | 🔶 必须拆分 |
| > 50 | 极复杂 | 🔴 紧急重构 |

## 重构模式

- **卫语句**：把失败条件提前返回，减少嵌套
  ```python
  # 重构前：5层嵌套
  # 重构后：卫语句提前返回
  if not data: return None
  if is_invalid(data): return None
  ```
- **提取函数**：每个分支逻辑提取为独立函数
- **策略模式**：多分支 → 表驱动
- **命令模式**：操作步骤多 → 命令对象

## 输出

```
## 复杂度审计
### 概览
- 高复杂度(>10)：8 个
- 极高复杂度(>20)：2 个

### TOP 函数
| 文件 | 函数 | CC | 建议 |
|------|------|-----|------|
| orders.py | process_order | 15 | 卫语句 |
| auth.py | authenticate | 23 | 策略模式 |

### 预计优化
- 平均 CC：18 → 6
```

## 注意

- CC 工具测圈复杂度，不是行数
- 测试覆盖率要跟上高 CC 函数
- 不要为降 CC 过度拆分