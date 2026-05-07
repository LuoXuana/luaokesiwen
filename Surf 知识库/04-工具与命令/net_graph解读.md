---
tags:
  - surf
  - commands
  - network
---

# 📊 net_graph 解读

## 开关

配置中绑定了 **N 键** 切换 net_graph：

```
按 N → 显示  → 再按 N → 关闭
```

## 关键参数解读

```
FPS:  600    | 帧率，稳定在 fps_max 附近最好
VAR:  0.5ms  | 帧生成时间波动，越小越稳
ping: 20ms   | 延迟，越低越好
loss: 0%     | 丢包率，>0% 说明网络有问题
choke: 0%    | 阻塞率，>0% 说明带宽不够
tick: 100    | 服务器 tickrate，应与 cl_updaterate 匹配
```

### FPS

- 稳定在 `fps_max` 附近（600）说明性能充足
- 剧烈波动说明需要进一步优化

### VAR

- < 1ms：流畅
- 1~3ms：轻微卡顿
- > 3ms：明显卡顿，需要排查

### Ping

| ping | 体验 |
|------|------|
| < 30 | 极好 |
| 30~60 | 良好 |
| 60~100 | 可接受 |
| > 100 | 有明显延迟感 |

### Loss / Choke

> 两个最重要的指标，**必须为 0%**

- **loss**：服务器发往你的数据包丢失 → 人物瞬移、操作无响应
- **choke**：你发往服务器的数据包阻塞 → 按键延迟

如果出现 loss/choke：

```cfg
// 检查 rate 是否过高
rate 30000

// 确认 updaterate/cmdrate 匹配服务器
cl_updaterate 100
cl_cmdrate 100
```

---

## n 键快捷开关已绑定

所有配置中已加入 net_graph 开关，游戏中按 `N` 即可查看。
