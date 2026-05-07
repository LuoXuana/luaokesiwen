---
tags:
  - surf
  - commands
  - reference
---

# 📋 sm 命令速查

## 常用命令

| 命令 | 缩写键 | 功能 |
|------|--------|------|
| `sm_teleport` | **R** | 回到最后保存的位置 |
| `sm_saveloc` | **C** | 保存当前位置 |
| `sm_r` | **X** | 重试/回到起点 |
| `sm_goto` | **Z** | 传送到其他玩家位置 |
| `sm_spec` | **T** | 进入旁观模式 |
| `sm_stuck` | **Mouse3** | 卡住时解卡 |
| `sm_restart` | **Mouse4** | 重新开始 |
| `sm_block` | trikz 的 R | 阻挡（trikz 模式专用） |
| `sm_tele` | trikz 的 F | 传送（trikz 模式专用） |

## 配置中的 sm alias

为防止服务器覆盖，配置中预设了 alias：

```cfg
alias "sm_r" "say /r"
alias "sm_saveloc" "say /saveloc"
alias "sm_teleport" "say /teleport"
alias "sm_goto" "say /goto"
alias "sm_spec" "say /spec"
alias "sm_stuck" "say /stuck"
alias "sm_restart" "say /restart"
```

## 更多命令

一般在服里输入 `/help` 或 `!help` 可以查看该服支持的所有 sm_ 命令。常见的还有：

| 命令 | 功能 |
|------|------|
| `sm_rank` | 查询排名 |
| `sm_top` | 查看排行榜 |
| `sm_bonus` | 跳 bonus 路线 |
| `sm_stage` | 跳指定 stage |
| `sm_wrc` | 查看世界纪录 |
| `sm_avg` | 查看平均时间 |
| `sm_stuck` | 解卡 |
| `sm_hide` | 隐藏玩家 |
| `sm_maptime` | 查看地图时间 |

---

> 数据解读 → [[net_graph解读]]
