# Kubernetes v1.35 生产运维排障手册

## 1. Pod 处于 Pending 状态排查
Pod 处于 Pending 状态通常意味着调度器无法将其绑定到工作节点。
排查步骤：
1. 执行 kubectl describe pod <pod-name> 检查 Events 输出。
2. 常见根因包含：节点资源耗尽（CPU/Memory Insufficient）、污点不容忍（Node Taints/Tolerations）、未绑定的存储卷 PVC（Unbound PVC）。

## 2. CrashLoopBackOff 故障排查
该状态表示容器启动后立即异常退出。
排查步骤：
1. 查看实时或崩溃容器日志：kubectl logs <pod-name> --previous。
2. 检查探针配置（Liveness Probe）超时或端口是否错误。
3. 检查容器入口命令与内存配额是否触碰 OOMKilled（退出码 137）。
