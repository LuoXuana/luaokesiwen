---
tags:
  - css
  - surf
  - config
related:
  - "[[game configs/surf110.cfg]]"
---

# surf110.cfg 优化说明

> 基于洛轩原版配置重构，修复 3 个 bug，增强 2 个功能

## 🔧 修复的 Bug

### 1. Restore 系统形同虚设（核心 bug）

```cfg
// 原版：restore alias 全是空的，等于没做
alias +mTurnRightHigh_restore ""    ← 空的！
alias +mTurnLeftHigh_restore  ""    ← 空的！
```

**场景复现**：按住 `Q`（左转），再按住 `E`（右转），然后松开 `Q` → 此时 `E` 应该继续左转，但 restore 为空导致**卡方向**。

**修复**：释放时用 `-left; -right` 双清，保证状态干净。高低速切换改为 bind 级别的 alias，不再依赖 restore。

### 2. R 切高速后无法回到低速

```cfg
// 原版：R 绑了高速就回不来了
bind "r" "sm_teleport; bind q +mTurnLeftHigh; bind e +mTurnRightHigh"
```

**修复**：增加 `V` 键切回低速，用 alias 统一管理：

```cfg
alias "turn_speed_low"  "bind q +mTurnLeftLow; bind e +mTurnRightLow"
alias "turn_speed_high" "bind q +mTurnLeftHigh; bind e +mTurnRightHigh"
bind "r" "sm_teleport; turn_speed_high"
bind "v" "turn_speed_low"
```

### 3. `rate 786432` 是 CS:GO 的值

CS:S 原生的 rate 上限是 **30000**（CS:GO 才支持 786432）。改回 30000 避免潜在的网络问题。如果你的服支持高 rate，再改回去即可。

---

## ✨ 新增功能

| 按键 | 功能 | 说明 |
|------|------|------|
| `V` | 切回低速转向 | 原版按了 R 后没法回去 |
| `N` | net_graph 开关 | 按一次显示，再按关闭 |

---

## ⚡ 其他调整

| 项目 | 原版 | 优化版 | 原因 |
|------|------|--------|------|
| `fps_max` | 1100 | 600 | CS:S 物理引擎在 >600 FPS 时可能抖动 |
| `rate` | 786432 | 30000 | CS:S 原生上限，786432 是 CS:GO 的值 |
| 文件 | 单文件无备份 | `surf110_original.cfg` 已保留 | 方便回滚 |

---

## 📁 文件位置

- **优化版**: `[[game configs/surf110.cfg]]`
- **原版备份**: `[[game configs/surf110_original.cfg]]`
