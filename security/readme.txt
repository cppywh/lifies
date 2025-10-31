
## 一、先定“人设”：你更像哪种安全人？

用 4 个轴定位自己（打★是你现在的倾向）：

1. **理论 ↔ 工程**（喜欢证明/抽象？还是搭系统/写代码上手？）
2. **数学密度**（线代/数论/概率的舒适度）
3. **短期产出 ↔ 长期下注**（3 个月能出 demo/实习？还是愿意 1–2 年打磨论文？）
4. **就业导向**（工业研发/安全工程 ↔ 读博/学术）

> 现实提醒：全球网络安全缺口依然大（2024 年缺口**约 476 万**），AI 正在改变岗位技能结构，但“安全 + AI”的复合型人才仍稀缺，这是你的机会窗口。([ISC2][1])

---

## 二、热门方向快速对比（按你列的几个）

| 方向                               | 你每天在干啥                         | 门槛/前置                  | 3 个月可做的**小成果**                                                                                              | 就业/延展                                                                                                                       |
| -------------------------------- | ------------------------------ | ---------------------- | ----------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **应用/工程密码学（含 PQC 迁移）**           | 把算法落到协议/库：TLS、PKI、硬件/固件签名、密码模块 | C/C++/Rust、协议栈、一点编译/构建 | 用 **OpenSSL 3 + OQS Provider** 把 **ML‑KEM（Kyber）**和 **ML‑DSA（Dilithium）**接到 TLS1.3，做握手时延&证书体积评测             | PQ 迁移工程、密码库工程、云/安全厂商研究岗。NIST 已批准 **FIPS 203/204/205**；NSA 要求 2035 前完成过渡，窗口期明确。([NIST Computer Security Resource Center][2]) |
| **隐私计算 / 多方安全（MPC、HE、TEE、联邦学习）** | 设计与实现 MPC 协议、PSI、私有统计/广告归因     | 密码协议理解 + Python/C++    | 用 **MP‑SPDZ** 复现 PSI/安全求交计数；或基于 **FATE** 跑一个联邦二分类 baseline（含噪声/通信开销分析）                                      | 互联网/广告/金融合规场景需求真切（私有归因、跨域联合建模）。([mp-spdz.readthedocs.io][3])                                                                |
| **AI 安全（含隐私 ML）**                | 攻防评测、越狱与防御、数据/模型窃取、鲁棒训练、DP-SGD | PyTorch/数据管线、基本优化      | 用 **Opacus** 训练一个 DP-SGD 模型，给出 (ε, δ) 预算 & 任务精度权衡；对 LLM 场景参考 **OWASP Top10 for LLM**+ **MITRE ATLAS** 做红队清单 | “安全 + AI”缺口大、岗位新（红队、模型评测、安全平台）。([opacus.ai][4])                                                                             |
| **多方安全/理论密码学**                   | 证明安全性、构造协议、读/写 ePrint          | 高数理功底、论文阅读/写作          | 选一个 2PC/阈值签名的子问题，完成原型 + 安全性草稿，投 IACR ePrint（或做组会报告）                                                         | 读博/学术友好，也能反哺工程实现。([IACR][5])                                                                                                |
| **量子加密/计算（QKD/量子算法）**            | 偏硬件/实验或量子算法仿真；与 PQC 区分         | 量子信息/物理/工程背景最佳         | 做“**PQC 与现网迁移评测**”比纯 QKD 更可落地；如无量子硬件背景，不建议研一就 All‑in                                                        | 产业更偏设备/运营商/科研院所；短期落地性不如 PQC。**PQC 标准已落地，迁移在路上**。([NIST Computer Security Resource Center][2])                               |

> 快速判断：
>
> * **想要快产出 + 就业稳** → 先选 **PQC 迁移** 或 **MPC/隐私计算工程**。
> * **想做“安全+AI”** → 选 **AI 安全/隐私 ML**，用 DP、攻防评测建立护城河。
> * **强理论胃口、目标读博** → 选 **多方安全/理论密码**，同时做一个可运行的原型。

---

## 三、12 周“试跑”计划（边学边出活）

> 目标：**两条路线各做 1 个可展示项目**（代码 + 报告 + README），把盲目感变成可比较的数据。

**A 线：PQC 迁移（工程密码学）**

* 第 1–2 周：读 NIST 公告 + NSA CNSA2.0 时间表，明确算法与合规背景。([NIST Computer Security Resource Center][2])
* 第 3–6 周：基于 **Open Quantum Safe** 的 **liboqs / oqs‑provider**，在 OpenSSL 3 上开启 **ML‑KEM(=Kyber)** 握手 + **ML‑DSA(=Dilithium)** 证书签名；可做 **混合握手**。([Open Quantum Safe][6])
* 第 7–8 周：对比 RSA/ECDHE vs PQC 的握手耗时、报文大小、证书链体积。
* 第 9–10 周：写迁移指南（编译参数、OpenSSL provider 配置、兼容性踩坑）。
* 第 11–12 周：整理技术报告 + Demo 视频（本地回归脚本/容器），可投开源 issue 或教程贴。

**B 线（二选一）：**

1. **隐私计算/MPC**

   * 用 **MP‑SPDZ** 复现 **PSI**（求交或交集计数），并在 10–100 万条数据上评测吞吐/带宽；或用 **FATE** 跑一个纵向/横向联邦学习示例。([mp-spdz.readthedocs.io][3])
   * 选 1 个真实场景（如“跨应用手机号去重”），梳理安全模型（半诚实/恶意）。
2. **AI 安全/隐私 ML**

   * 用 **Opacus**（或 TensorFlow Privacy）在 CIFAR‑10/文本分类上做 **DP‑SGD**，给出 ε‑δ–精度曲线；对 LLM 场景按 **OWASP LLM Top10** 设计 10 条红队用例，映射到 **MITRE ATLAS** 技术。([opacus.ai][4])

> 交付物模板：`repo（代码/容器） + 报告（问题→方法→评测→结论） + 5 分钟讲解视频`

---

## 四、你现在跟着的老师“太慢”怎么破？

* **自带节奏**：发一封“**双周 10 分钟站会**”邮件，附你的 2 周计划和阻塞点；让老师只需点头。
* **找“共同导师/日常导师”**：同校其他课题组、学院实验中心、或高年级学长姐，建立每周 code review。
* **成果牵引沟通**：先把上面的 A 线或 B 线做出雏形，再“拿成果谈资源”（服务器、数据、横向课题）。
* **外部生态**：跟着 **IACR ePrint** / **PETS（PoPETs）** 跑读书会或开源议题，降低对单一导师的依赖。([IACR][5])

---

## 五、学习与工具栈（精简但够用）

* **密码学基础书单**：

  * *Introduction to Modern Cryptography (3rd)* — Katz & Lindell（理论打底，含 PQC 概览）。([Taylor & Francis][7])
  * *A Graduate Course in Applied Cryptography* — Boneh & Shoup（免费电子版，工程贴近）。([toc.cryptobook.us][8])
* **工程与库**：Linux/Git/Docker；C/C++/Rust；OpenSSL；**liboqs / oqs‑provider**（PQC）；**MP‑SPDZ / FATE**（MPC/联邦）；PyTorch + **Opacus** / TensorFlow Privacy（DP）。([GitHub][9])
* **AI 安全基准与框架**：**OWASP LLM Top10**、**MITRE ATLAS**；NIST **AI RMF**（把安全风险管理讲清楚）。([OWASP][10])
* **跟进论文/社区**：IACR ePrint、PETS/PoPETs、arXiv 的 cs.CR。([IACR][5])

---

## 六、如何做“方向选择”的量化决策？

做一张 1–5 分的打分表（越高越好）：**兴趣强度、短期产出、就业需求、导师/资源匹配度、核心竞争力**。

* 若 **PQC 迁移** 与 **AI 安全**并列，建议**先做 PQC**（更快出 demo），**再把 AI 安全作为 T 型扩展**（“密码 + AI 安全”稀缺度更高）。
* 背景提示：NIST 已在 2024 年批准 **FIPS 203/204/205**（Kyber / Dilithium / SPHINCS+），NSA 要求到 **2035 年完成过渡**，产业侧迫切需要迁移和评测能力。([NIST Computer Security Resource Center][2])

---

## 七、1 周内可以马上做的 6 件事

1. 选 **A 线 + B 线**各 1 个题目，建两个 GitHub 私仓；写 `README-目标&里程碑`。
2. 用 **oqs‑provider** 跑通本机 PQC TLS1.3 自签证书握手（留好命令行和日志）。([GitHub][11])
3. 装好 **MP‑SPDZ** 或 **FATE**，把示例 pipeline 跑通并记录吞吐。([mp-spdz.readthedocs.io][3])
4. 用 **Opacus** 跑一个 DP‑SGD 最小例子，输出 ε‑δ。([opacus.ai][4])
5. 订阅 **IACR ePrint** RSS、收藏 PETS/PoPETs；每周挑 1 篇做 10 分钟读后总结。([IACR][5])
6. 发出 2 封外部联系邮件（样例在下面），争取一个**共同指导**或**企业导师**。

**冷邮件模板（可直接改名发送）**

> 老师/工程师您好：
> 我是暨南大学网安硕士研一，方向意向是 **PQC 迁移 / MPC / AI 安全**。
> 我已在做 **[简单描述你的小项目，给 GitHub 链接]**。
> 想请教 **[一个具体问题]**，并希望能在 **[每两周一次 15 分钟]** 的频率请您指点路线与实验设计。
> 我能提供 **[能付出的贡献：代码、文档、测评、开源维护等]**。
> 若方便，您本/下周有 15 分钟能聊一下吗？谢谢！

---

## 最后给你的建议（直话直说）

* **别等老师给方向**：先用上面 12 周计划跑起来，拿可见产出去“吸引”指导。
* **聚焦一个锚点 + T 型拓展**：

  * 锚点建议选 **PQC 迁移** 或 **MPC 工程**（产出快、岗位多）。
  * T 型拓展到 **AI 安全/隐私 ML**（把项目换个数据域就能复用）。
* **把成果外显**：技术报告、复现实验、开源 PR、讲解视频，一样都很加分。
* **心态**：你不是“盲目”，只是还没把兴趣映射到“可验证的实验”。先小步快跑，再决定大方向。

如果你愿意，我可以**按你的课程安排和时间**，把上面 12 周计划细化成每周任务清单（命令行、评测指标、报告模板），并且给你挑一篇 ePrint 或 PoPETs 的论文做“第一周共读”。

[1]: https://www.isc2.org/Insights/2024/10/ISC2-2024-Cybersecurity-Workforce-Study "2024 ISC2 Cybersecurity Workforce Study"
[2]: https://csrc.nist.gov/News/2024/postquantum-cryptography-fips-approved "Post-Quantum Cryptography FIPS Approved | CSRC"
[3]: https://mp-spdz.readthedocs.io/en/latest/index.html?utm_source=chatgpt.com "Welcome to MP-SPDZ’s documentation! — MP-SPDZ documentation"
[4]: https://opacus.ai/?utm_source=chatgpt.com "Opacus · Train PyTorch models with Differential Privacy"
[5]: https://iacr.org/publications/preprint.html?utm_source=chatgpt.com "Cryptography Preprint Servers - IACR"
[6]: https://openquantumsafe.org/?utm_source=chatgpt.com "Home | Open Quantum Safe"
[7]: https://www.taylorfrancis.com/books/mono/10.1201/9781351133036/introduction-modern-cryptography-yehuda-lindell-jonathan-katz?utm_source=chatgpt.com "Introduction to Modern Cryptography | Jonathan Katz, Yehuda Lindell"
[8]: https://toc.cryptobook.us/?utm_source=chatgpt.com "A Graduate Course in Applied Cryptography"
[9]: https://github.com/open-quantum-safe/liboqs?utm_source=chatgpt.com "GitHub - open-quantum-safe/liboqs: C library for prototyping and ..."
[10]: https://owasp.org/www-project-top-10-for-large-language-model-applications/?utm_source=chatgpt.com "OWASP Top 10 for Large Language Model Applications"
[11]: https://github.com/open-quantum-safe/oqs-provider?utm_source=chatgpt.com "GitHub - open-quantum-safe/oqs-provider: OpenSSL 3 provider containing ..."
