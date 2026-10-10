# 朱显银

**Python 数据采集 / 自动化** ｜ 现居惠州，接受深圳现场面试、可即时到岗

> 我做的项目都在强调同一件事：**能长期稳定跑，而不是跑通一次。**
> 限速、退避、断点续采、幂等落盘 —— 这些正是"能跑一次"和"能跑一个月"的区别。

---

## 我在做什么

把接口协议读懂，然后**稳定复现**：签名算法复算、动态门控处理、限流治理、
断点续采与幂等落盘。

做技术选型时我有一条固定偏好：**能纯算法复现就不去执行别人的 JS** ——
少一个运行时依赖，长期运行就少一个故障点。

## 项目

### [sign-lab](https://github.com/15186406708/sign-lab) · 签名复算靶场
自建签名校验服务端 + 自建四层混淆的下发脚本，客户端**全程不执行任何 JS** 完成静态还原：
用结构（而不是变量名）定位取值模式、识别异或解码逻辑、
以 **AST 白名单**对常量折叠表达式求值（而不是图省事用 `eval`）。

* 24 组用例在 Node 与 Python 两侧**逐字节**比对签名，结果完全一致
* 另 23 项端到端断言：限流、参数截断、随机串重放、退避
* 零第三方依赖 ｜ MIT

### [ratelimit-kit](https://github.com/15186406708/ratelimit-kit) · 限流与弹性重试库
把两个项目里反复写到的四个原语抽成独立库：

| 原语 | 解决的坑 |
|---|---|
| `limiter` | "每请求 sleep"在并发下会叠乘成 N 倍速度 → 全局令牌桶 |
| `retry` | 把 429 抛给上层框架会和它的自动重连叠成连点 → **原地退避** |
| `checkpoint` | 断点会倒退 / 参数变了不失效 → 原子写 + 只前进 + 身份校验 |
| `store` | 追加写产生重复 → 事实日志重建去重集 + 导出整体重写 |

* **72 项单元测试**，已被另一个项目共用
* 零第三方依赖 ｜ MIT

### 采集交付实践（代码未公开）
一个从"只能出 200 条"改造到**能稳定交付**的采集项目：

* **220 页共 11 000 条记录，去重后交付 10 867 条**，日期字段完整率 100%
* 229 次请求中 9 次被服务端限流，**全部原地退避成功、0 页失败**
* 断点续采实测有效：第二次运行从第 201 页继续，只跑 20 页
* 改进过程包括：消除框架派发乱序的**根因**（而不是继续打补丁）、
  凭据全部外移、以及修复一次**静默数据损坏**（字段解析失败导致整列空白，
  且因去重键退化连带静默丢失 294 行）

> 目标为真实站点，按合规原则未公开 —— **公开能力，不公开目标**。
> 可公开的那部分已经沉淀成上面的 `ratelimit-kit`。

## 我在意什么

这三条不是口号，每一条都是拿一次真实返工换来的：

1. **让失败变得吵闹。** 出错但程序不崩是最危险的 —— 空列和少掉的行会被当成正常结果交付出去。
   所以关键字段异常必须**主动告警**，而不是只写进日志。
2. **测试的输入是哪来的？** 如果测试语料是我自己写的，那它验证的是我的假设，不是现实。
   **验证"事实"必须用真实样本，验证"行为"才能用仿真。**
3. **消除根因，而不是打补丁。** 补丁依赖"我记得每次都做对"；
   结构上的约束不依赖任何人的记忆力。

## 技术栈

`Python` · `requests / httpx / Scrapy / feapder` · 接口协议分析 · 混淆 JS 还原
· 加密算法复算 · 幂等落盘与断点续采 · `unittest` · `Git` · Linux 常用命令

了解（跟练级）：jadx 反编译、Frida hook、Charles / Reqable 抓包与证书配置

## 联系

📧 **zhuxianyin@yeah.net** ｜ 目前**求职中**：Python 数据采集 / 自动化、IT 支持 / 运维

---

<details>
<summary><b>English summary</b></summary>

**Zhu Xianyin** — Python data collection & automation. Based in Huizhou, available for
on-site interviews in Shenzhen immediately.

I build collectors that are meant to **keep running**, not to work once: global rate
limiting, in-place 429 backoff, resumable checkpoints, and idempotent persistence.

* **[sign-lab](https://github.com/15186406708/sign-lab)** — a self-built signature
  playground. The server (and its four-layer obfuscation) is mine, so nothing
  third-party is involved. The client recovers the signing salt by **pure static
  analysis, executing no JavaScript**, using an AST allowlist instead of `eval`.
  24 golden-vector cases match byte-for-byte between Node and Python.
* **[ratelimit-kit](https://github.com/15186406708/ratelimit-kit)** — four primitives
  extracted from two real projects: global token-bucket limiting, in-place 429 backoff
  with separate retry budgets, monotonic atomic checkpoints, and an idempotent
  append-only store whose dedupe set is rebuilt from the log itself. 72 unit tests,
  zero dependencies, MIT.

Three things I care about, each learned from an actual rework:
make failures **loud**; know **where your test inputs come from** (self-authored
fixtures verify your assumptions, not reality); and **remove root causes** instead of
patching symptoms.

</details>
