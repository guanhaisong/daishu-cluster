# ⚔️ 踩坑手册 — AI Agent集群实战避坑（开源版）

> 来自一个真实运行的多Agent集群（1个AI总经理 + 4个AI兵，同住一台Windows机器）的真实bug。
> 格式：**症状 → 根因 → 解法 → 复现代码**。不谈理论，全是伤疤。

**⏳ 每周更新，Watch本仓库追更。英文版见 [cases/README.md](cases/README.md)**

## 当前收录（v1 · 5例，持续追加）

1. **PS5.1写UTF-8带BOM**——下游JSON解析全崩；`.trim()`救不了`\uFEFF`
2. **CDP调试端口重启即失联**——进程活着≠端口活着，派单前必须双检
3. **截图截错窗口**——CopyFromScreen截屏=截"屏幕上是什么"，不是"你要的那个窗口"；PrintWindow+PW_RENDERFULLCONTENT才是王道
4. **Electron的MainWindowHandle是56x56托盘图标**——按PID枚举全部顶层窗口挑最大可见的
5. **微信媒体桥连发4件触发会话休眠**——ret=-2逐条间隔发送，真实错误看errors.log不看err.json

## 完整版

100+案例（含复现脚本/监控配方/监控告警模板）持续更新中——见仓库主页联系方式。
