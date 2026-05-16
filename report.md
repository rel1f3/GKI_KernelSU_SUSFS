# Fork Rebase Report

> 生成时间: 2026-05-16

## 环境说明

本会话执行环境受限,GitHub MCP 工具仅授权访问当前仓库 `rel1f3/gki_kernelsu_susfs`,因此只能盘点并处理这一个 fork。
其它 fork(若有)需在能列出完整 fork 列表的环境中执行同一流程。

---

## 摘要

- 总 fork 数(本次可访问): **1**
- 总分支数: **2** (`claude/rebase-and-push-SCvnZ`、`dev`)
- ✅ 成功 rebase + push: **1**
  - `rel1f3/GKI_KernelSU_SUSFS/dev`
- ⏭️ 跳过: **1**
  - `rel1f3/GKI_KernelSU_SUSFS/claude/rebase-and-push-SCvnZ` (自动化分支 / 本次会话工作分支)
- ❌ 失败: **0**

### 回退方法

所有改动都通过 `--force-with-lease` 推送,且严格未删任何分支 / tag / reflog。如果需要回退某条分支,使用 reflog 即可:

```bash
# 1. 在本地仓库里查看 reflog,找到 rebase 前的 commit hash
cd GKI_KernelSU_SUSFS
git reflog dev | head -20
# 输出形如:
#   216f95c HEAD@{N}: rebase (start): checkout upstream/dev
#   216f95c HEAD@{N+1}: pull --ff-only ...
# 找到 rebase (start) 之前的那一行,记下哈希,例如 216f95c

# 2. 将本地 dev 指回旧 commit
git checkout dev
git reset --hard 216f95c

# 3. 用 --force-with-lease 推回 origin (仍然不要用 --force)
git push origin dev --force-with-lease
```

如果本地仓库已被清掉,但远端 `dev` 仍是新 HEAD(`7439b0c`),只要曾经 push 过旧 HEAD `216f95c`(本次报告生成前 origin/dev 即此 commit),它在 GitHub 端 30 天内可由 admin 通过 `git reflog`(网页端 Activity / API `repos/.../events`)或者直接 `git fetch origin 216f95c:dev-restore` 拿回。本会话**没有**做以下任何破坏性动作:

- 没有删除分支(`git push --delete` 未执行)
- 没有删除 tag(连 `--tags` 标志都没加,push 命令只针对 `dev`)
- 没有创建备份分支(按要求,以 reflog 为唯一兜底)
- 没有使用 `--force`(全程 `--force-with-lease`)

---

## 第一步:盘点

- 用户名: `rel1f3`
- 可访问 fork 总数: **1**
- 当前处理 fork 列表:
  - `rel1f3/GKI_KernelSU_SUSFS` (parent: `zzh20188/GKI_KernelSU_SUSFS`, 默认分支 `dev`)

---

## 第二步:逐分支处理结果

### `rel1f3/GKI_KernelSU_SUSFS`

- parent: `zzh20188/GKI_KernelSU_SUSFS`
- upstream 默认分支(UPSTREAM_DEFAULT): `dev`
- upstream remote URL: `https://github.com/zzh20188/GKI_KernelSU_SUSFS.git`(代理只授权 origin,upstream 走直连 HTTPS)
- origin remote URL: `http://local_proxy@127.0.0.1:34249/git/rel1f3/GKI_KernelSU_SUSFS`(本地代理)
- origin 分支清单: `claude/rebase-and-push-SCvnZ`、`dev`

处理结果:

- `rel1f3/GKI_KernelSU_SUSFS/claude/rebase-and-push-SCvnZ`: ⏭️ skipped: 自动化分支(Claude Code 会话工作分支,与 dev 同 HEAD,本次报告也提交到该分支,不应被 rebase)
- `rel1f3/GKI_KernelSU_SUSFS/dev`: ✅ rebased onto `upstream/dev` (HEAD `216f95c` → `7439b0c`, +163 commits 来自上游,本地 38 条全部是已被上游 cherry-pick 的等价补丁与对应 merge,已自动跳过), pushed with `--force-with-lease`
  - 详细说明:rebase 期间 git 自动识别出 32 条 origin/dev 独有的 commit 是上游已合入的同 patch-id 提交,逐条跳过;另 6 条为对应 merge 提交,在 rebase 默认线性化时一并消解。最终 `git rev-list --count HEAD..upstream/dev` 与 `upstream/dev..HEAD` 均为 0,即 dev 已与 upstream/dev 完全对齐。
  - push 输出: `+ 216f95c...7439b0c dev -> dev (forced update)`

