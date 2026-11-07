# 交接文档 · ai-journey 仓库

> 最后更新：2026-11-08 01:50（Asia/Shanghai）
> 仓库：https://github.com/Conroy-X/ai-journey（公开）
> 本地主副本：WSL `~/ai-journey/`（git 仓库根目录即此处）
> 最新提交：`c3a28af`

---

## 一、这个项目是什么

**用户 李玉斌（rik）** 正在从传统 Web/前端 转向 **AI 应用开发工程师**。
这个仓库是他的 90 单元学习工程：每个 `unitNN-xxx/` 是一个学习单元，包含
`main.py`（离线自检）+ `README.md`（讲义）+ `notes.md`（个人笔记，留空给用户填）。

**重要**：单元是「学习单元」，不是「日历天」。没有「落后」的概念。

---

## 二、当前状态：全部就绪 ✅

| 范围 | 状态 |
| --- | --- |
| Unit 1–60 | 代码 + 文档齐全，批量自检全绿 |
| Unit 61–78 | 自检全绿（Docker 含真机 compose 验证） |
| Unit 79–90 | **按计划主题重做/补齐**，全部有 FastAPI app + HTTP 端点 |
| **61–90 全量自检** | **30 个单元、552 项断言，全部通过** |
| PII 扫描 | 干净 |
| 远端同步 | local == remote |

### 本轮整改完成的具体内容

| 单元 | 做了什么 |
| --- | --- |
| **61** | 接 PostgreSQL：compose 编排 app + db，`condition: service_healthy` 等就绪；可降级内存；真机实测 142 秒起、双服务 healthy、数据落库 |
| **70** | 评测集 4 → **30 条**（22 可答 + 8 应拒答）；四项指标：Recall@K / 忠实度 / 相关性 / 拒答双侧；加检索阈值 MIN_SCORE=0.4 |
| **79** | 重做为「工程化收尾」：RBAC + 日志中间件（request_id/耗时）+ 统一错误页 + 成本面板 |
| **80** | 重做为「项目 2 最终版」：企业知识库 RAG + 静态 UI + 交付清单 + 30 秒录 Demo 脚本 |
| **81** | 重做为「Research Agent 优化」：引用校验 / 去重 5→3 / 三段报告模板（结论·来源·局限） |
| **82** | 重做为「LangGraph 优化」：子图 / 并行节点（原始 state）/ 流式事件 |
| **83** | 重做为「MCP 集成」：Server 工具注册 + Client 发现调用留痕 + **挂进 Research Agent** |
| **84** | Memory（TTL）+ HITL（危险操作审批、不能自批自、审批只对本人有效） |
| **85** | 简历校验器：PII / 弱动词 / 量化结果 / 条目数，0–100 打分 |
| **86** | GitHub 卫生：README / .gitignore / 敏感文件 / 密钥扫描 / commit 规范 |
| **87–89** | 面试题库 8 → **各 30 题**，每题 3 要点作评分标准 |
| **90** | 求职投递：记录 + 状态只进不退 + 漏斗统计 + 渠道复盘 |

另外：79/80 目录改名对齐内容（`unit79-engineering-polish`、`unit80-project2-final`）。

---

## 三、环境与约束（重要，别踩坑）

### 网络

| 项 | 情况 |
| --- | --- |
| GitHub | **HTTPS 不通，SSH 22 可用**（`git push` 走 SSH） |
| GitHub API | `api.github.com` 可用（需 PAT） |
| PyPI | 走 USTC 镜像（已配） |
| Docker daemon | 已配代理 `/etc/systemd/system/docker.service.d/http-proxy.conf` → `http://172.19.64.1:7897` |
| Dockerfile 内 pip | **必须** `-i https://mirrors.ustc.edu.cn/pypi/simple`，否则卡死 |

### 标准流程

```bash
export PATH="$HOME/.local/bin:$PATH"    # 非交互 shell 需要

cd ~/ai-journey/unitNN-xxx
uv venv --python /usr/bin/python3 .venv
uv pip install -q -r requirements.txt
uv run python main.py                   # 必须看到「结果：N 通过 / 0 失败」
```

### 提交前

```bash
cd ~/ai-journey
python3 00-setup/check-pii.py           # 必须通过
```

**PII 扫描器会拦测试夹具**（手机号、邮箱样例）。豁免方式：行尾加
`# pii-check: allow`（Python）或 `<!-- pii-check: allow -->`（Markdown）。

### 提交规范

```bash
git -c user.name="Conroy-X" -c user.email="Conroy-X@users.noreply.github.com" \
  commit -m "feat(unitNN): 简述"
```

前缀：`feat` / `fix` / `docs` / `chore` / `refactor`。
仓库有 pre-commit 守门员钩子，自动拦截敏感文件/密钥/大文件。

### GitHub PAT

存在个人画像记忆里（`C:\Users\rik\.workbuddy\user-*/MEMORY.md` L44）。
⚠️ 权限接近全权（含 `delete_repo`/`admin:org`），**建议提醒用户降权**。

---

## 四、工作方式约定（用户偏好）

1. **直接动手，不要过度询问**
2. **简体中文**回复
3. **notes.md 里「我自己的话解释」段落必须留空**给用户填，不要代写
4. **README 里的命令必须真跑过**
5. **每个单元自检必须全绿**才算完成
6. 用户有「过度准备、迟迟不交付」的倾向，要推着他跑起来

---

## 五、如果还要继续做（可选增强）

当前所有审计问题已清零。若还想加强，这些是「锦上添花」而非「必做」：

| 方向 | 说明 |
| --- | --- |
| 真 API 模式 | 目前 61–90 全部离线（假模型/规则），可加真实 LLM 调用分支 |
| 更多评测样本 | 70 现在是 30 条，可扩到 100+ |
| UI 完善 | 目前只有 80 有静态 UI，可给 79/81 也加 |
| 单元 91+ | 计划只到 90，如需继续要新规划 |

---

## 六、验证命令（全量）

```bash
export PATH="$HOME/.local/bin:$PATH"
ROOT="$HOME/ai-journey"
for n in $(seq 61 90); do
  d=$(ls -d "$ROOT"/unit${n}-*/ 2>/dev/null | head -1)
  out=$( cd "${d%/}" && .venv/bin/python main.py 2>&1 | grep -oE '结果：[0-9]+ 通过 / [0-9]+ 失败' )
  echo "unit$n: $out"
done
```

期望：30 行全部 `0 失败`。

---

## 七、关键文件索引

| 文件 | 作用 |
| --- | --- |
| `README.md` | 仓库门面：进度、目录树、快速开始 |
| `docs/学习单元计划.md` | **主计划**：90 单元主题、资料、完成标准（含「整改完成」说明） |
| `docs/interview-questions.md` | 面试题库 |
| `00-setup/check-pii.py` | PII 扫描器（提交前必跑） |
| `unitNN-xxx/main.py` | 每单元自检入口 |

---

**质量基线**：每个单元离线自检全绿 + README 讲清「为什么」。
任何新单元都要达到这个基线，否则不如不写。