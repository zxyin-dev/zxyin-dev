# 朱显银 · zxyin-dev

**Python 数据采集 / 接口协议还原 / 长期运行版采集器** ｜ 远程协作 · 惠州

> 我交付的东西只有一个标准：**能长期稳定跑，而不是跑通一次。**
> 限速、退避、断点续采、幂等落盘——这些正是「跑一次」和「跑一个月」的区别。

📧 zhuxianyin@yeah.net　｜　远程协作，先给样例再付款

---

## 服务范围与参考区间

| 类型 | 说明 | 参考价（元） |
|---|---|---|
| 单站列表 / 详情采集 | 无登录、无签名、无风控 | 200 - 500 |
| 带登录态采集 | Cookie / Token 持久化、掉线重登 | 500 - 1200 |
| 接口参数还原 | 混淆 JS 里的签名算法还原成纯 Python | 800 - 3000 |
| 强风控站点适配 | 动态 cookie、指纹校验、补环境 | 1500 - 5000 |
| 长期运行版 | 断点续采 + 限速 + 定时 + 告警 | 原价 +50% - 100% |
| 数据按月订阅 | 每周 / 每日更新指定字段 | 300 - 2000 / 月 |
| 已有采集器维护 | 加字段、改站点、修 bug | 100 - 500 / 次 |

小需求按件起步价 **199 元**（单页字段提取、表格清洗、一次性导出）。

**合作流程**：说清「哪个站、要哪些字段」→ 我免费跑 20 条样例 → 确认字段 → 50% 定金 → 交付 → 30 天内小改动免费修。

**不接**：个人信息 / 账号密码类数据、抢购秒杀、刷量、批量注册、付费内容转载。

---

## 能交付什么

一个采集系统做到「能交付」需要的东西，我都单独做过：
列表 / 详情抓取、登录态与 Cookie 持久化、接口参数还原（四层混淆 JS → 纯 Python 复算）、
全局限流与 429 原地退避、断点续采、幂等落盘、字段级质量校验、异常主动告警。

交付习惯：源码含注释 + 运行说明 + 依赖清单 + 实测数据样例。

---

## 项目

### [sign-lab](https://github.com/zxyin-dev/sign-lab) · 签名复算靶场

自建签名校验服务端 + 自建四层混淆的下发脚本，客户端**全程不执行任何 JS** 完成静态还原：
用结构（而不是变量名）定位取值模式、识别异或解码逻辑、
以 **AST 白名单**对常量折叠表达式求值（而不是图省事用 `eval`）。

- 24 组用例在 Node 与 Python 两侧**逐字节**比对签名，结果完全一致
- 另 26 项端到端断言：限流、参数截断、随机串重放、退避、状态表回收
- 零第三方依赖 ｜ MIT

> 目标服务器是我自己写的，数据是生成的。**不含任何针对第三方的绕过代码。**

### [ratelimit-kit](https://github.com/zxyin-dev/ratelimit-kit) · 限流与弹性重试库

| 原语 | 解决的坑 |
|---|---|
| `limiter` | 「每请求 sleep」在并发下叠乘成 N 倍速度 → 全局令牌桶 |
| `retry` | 把 429 抛给上层框架会和自动重连叠成连点 → **原地退避** |
| `checkpoint` | 断点会倒退 / 参数变了不失效 → 原子写 + 只前进 + 身份校验 |
| `store` | 追加写产生重复 → 事实日志重建去重集 + 导出整体重写 |

- **72 项单元测试**，已被另一个项目共用
- 零第三方依赖 ｜ MIT

### [crawler-kit](https://github.com/zxyin-dev/crawler-kit) · 采集框架

把「跑一次」变成「跑一个月」的骨架：全局令牌桶限速、429 原地退避、
断点续采（只前进、参数变则失效）、幂等落盘（事实日志重建去重集）、插件式签名与传输。
核心零第三方依赖，71 项测试跑在本地 mock 站点上。

### 采集交付实践（代码未公开）

一个招标公告类站点的长期采集项目，交付文件现可核对：

- **10 867 条记录，主键与标题重复 0 条**，发布日期字段完整率 100%，覆盖 2026-09-05 至 2026-10-11
- 全局限速 + 429 原地退避：限流按服务端提示等待，恢复后接着跑，**未丢页**
- 断点续采实测有效：第二次运行从第 201 页继续，只跑 20 页
- 修复过一次**静默数据损坏**：字段解析失败导致整列空白，程序不报错，靠校验才发现

> 目标为真实站点，按合规原则未公开——**公开能力，不公开目标**。

---

## 我在意什么

三条，每条都是拿一次真实返工换来的：

1. **让失败变得吵闹。** 出错但程序不崩最危险——空列和少掉的行会被当成正常结果交付。
2. **测试的输入是哪来的？** 自造语料验证的是假设，真实验证的是事实。
3. **消除根因，而不是打补丁。** 补丁依赖「我记得每次都做对」。

---

## 技术栈

`Python` · `requests / httpx / Scrapy` · 接口协议分析 · 混淆 JS 还原
· 加密算法复算 · 幂等落盘与断点续采 · `unittest` · `Git` · Linux 常用命令

了解：jadx 反编译、Frida hook、Charles / Reqable 抓包与证书配置

---

<details>
<summary><b>English summary</b></summary>

**Zhu Xianyin** — Python data collection, protocol recovery & automation. Based in Huizhou,
available for remote contract work.

I build collectors meant to keep running, not to work once: global rate limiting,
in-place 429 backoff, resumable checkpoints, and idempotent persistence.

- **[sign-lab](https://github.com/zxyin-dev/sign-lab)** — a self-built signature playground.
  The server and its four-layer obfuscation are mine; the client recovers the signing salt by
  pure static analysis, executing no JavaScript. 24 golden vectors match byte-for-byte.
- **[ratelimit-kit](https://github.com/zxyin-dev/ratelimit-kit)** — four primitives, 72 tests,
  zero dependencies, MIT.
- **[crawler-kit](https://github.com/zxyin-dev/crawler-kit)** — the skeleton that turns a one-shot
  script into a long-running collector: rate limit, in-place backoff, resume, idempotent store.

Contact: zhuxianyin@yeah.net

</details>
