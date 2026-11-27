# cloud-hypervisor 学习记录

## 仓库结构

- 上游：`cloud-hypervisor/cloud-hypervisor`（Rust 实现的 VMM，基于 KVM / MSHV）
- 我的 fork：`aspiration-W/cloud-hypervisor`
- 本地克隆：`D:\code\cloud-hypervisor`

本地 remote 配置（fork 的标准结构）：

```
origin    https://github.com/aspiration-W/cloud-hypervisor.git   # 我的 fork，日常推这里
upstream  https://github.com/cloud-hypervisor/cloud-hypervisor.git  # 官方，只读，用来越同步
```

## 分支约定

- `main` 只跟随上游，不直接在上面提交
- 每次改动开独立分支，例如 `notes/study-log`、`fix-xxx`
- 给上游提 PR 时：base 选 `cloud-hypervisor/cloud-hypervisor:main`，head 选自己 fork 的分支；
  上游要求 DCO，提交必须带 `Signed-off-by`，用 `git commit -s` 生成

## 日常操作

```powershell
# 同步上游
git fetch upstream
git checkout main
git merge --ff-only upstream/main
git push origin main

# 干活
git checkout -b fix-xxx
git commit -s -m "xxx: 说明"
git push -u origin fix-xxx
```

## 待办：把 VMM 跑起来

准备目录：`D:\code\ch-test`（约 600MB，不纳入版本控制）

- `cloud-hypervisor-static`：v55.0 官方预编译，ELF 已校验
- `hypervisor-fw`：启动固件
- `noble-server-cloudimg-amd64.img`：Ubuntu 24.04 cloud image，592.5MB，SHA256 已核验
- `setup.sh`：check / prepare / run 三段

运行前提，必须先过这三关：

1. 宿主内核 ≥ 6.8（`uname -r`）
2. `/dev/kvm` 存在（`ls -l /dev/kvm`）
3. 当前用户在 kvm 组（`groups` 里有 kvm）

三项通过后，把镜像转 raw 并生成 cloud-init 盘，再按 `setup.sh` 的 run 启动。

## 笔记

（从这里往下自己写）
