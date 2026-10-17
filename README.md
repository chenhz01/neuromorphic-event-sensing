## English Summary

These are ten working notes on a single question: whether the frame-plus-global-clock paradigm still holds when intelligence has to stay on around the clock under milliwatt budgets. They cover first-principles energy accounting for always-on systems, spike-based encoding and its three training routes, event cameras as a different input primitive, the 2026 hardware landscape, what actually sits behind the advertised efficiency multipliers, the state of the software stack, two-tier hybrid architectures, and a closing list of cases where this approach should not be used at all. Every claim is stated with a boundary number, a source grade, and the specific evidence that would kill it. They are written for engineers and researchers evaluating this field, particularly those deciding whether to adopt neuromorphic silicon or to borrow only its design discipline.

> 中文简介见下 ↓

# 神经形态计算与事件驱动传感 · 十件研究笔记

这是对一个具体问题做了长期跟踪后留下的十件笔记：当智能必须 7×24 小时在线、且预算以毫瓦和分币计时，「帧 + 全局时钟」这套默认范式在物理上还成不成立。笔记从头到尾只做一件事——把架构层面的能耗主张拆成可核算的条件，并给每条主张配上能杀死它的证据。

## 两性费曼：怎么读这套笔记

每件笔记在标题之后、正文之前，都有两块固定结构：

- **性一（12 岁也能懂）**：这是什么（比喻 + 一句「哪里不像」）/ 与我何干（只能落在省钱、赚钱、避险、暂无四选一）/ 我该怎么办（一个 ≤2 小时、当天可启动的动作）。判据是不许出现未解释的术语。
- **性二（同行可验证）**：机制（推到物理 / 数学 / 经济学基座）/ 边界数（数字 + 来源等级）/ 反例（什么证据能推翻这条结论）/ 可复现（最小验证路径）。判据是不许出现无法验证的断言。

只过一层不算懂：大众层说不清，说明没真拆开；专业层给不出数，说明大众层只是好听。完整定义与自检协议见 [FEYNMAN.md](./FEYNMAN.md)。

## 十件笔记索引

| 编号 | 标题 | 一句话 | 关键边界数 | 证据等级 |
|---|---|---|---|---|
| 01 | [第一性原理：永远在线的智能为什么必须抛弃「帧」与「时钟」](./notes/01-first-principles-always-on.md) | 常在线系统的功耗是被「没变化时还在搬数据、还在跟着时钟翻转」吃掉的，不是被算错吃掉的 | Speck 空闲 <1mW；Akida 典型 30mW；TrueNorth 70mW；Loihi 2 芯片级 <1W | [二手] |
| 02 | [SNN：把信息编码进时间](./notes/02-snn-time-as-information.md) | 脉冲网络的信息不在幅度里，在「何时发放」里，换取的是低功耗与时序表达力 | Speck 328k 神经元、空闲 <1mW；Loihi 2 每芯片 100 万神经元 / 1.2 亿突触；Akida 1.2M 神经元 / 10M 突触、30mW | [二手] |
| 03 | [训练三条路线](./notes/03-training-three-routes.md) | 脉冲不可微，于是有三条绕路：转换、代理梯度、局部规则，代价各不相同 | 转换路线有延迟与精度损失；代理梯度训练更慢但时序任务常超越原 ANN；Loihi 2 支持可编程片上学习规则 | [二手] + [推算] |
| 04 | [事件相机 DVS：重新定义「输入」](./notes/04-event-cameras-dvs.md) | 像素只在亮度变化时输出事件，数据量跟着世界变，不跟着帧率走 | DVS 微秒级延迟、无空闲帧、无运动模糊；DAVIS240 于 2021 年随 Falcon Neuro 项目上过国际空间站 | [二手] + [推算] |
| 05 | [2026 硬件版图](./notes/05-hardware-map-2026.md) | 这个领域没有默认选项，选型的第一个变量是任务结构，不是芯片参数 | Loihi 2 <1W；Akida 2.0 30mW；Speck <1mW；SpiNNaker2 每芯片约 1W；TrueNorth 70mW | [二手] + [推算] |
| 06 | [能效真相](./notes/06-efficiency-truth.md) | 「省 100 倍」是三个乘数的积，缺一个条件乘积就塌 | 10×-1000× 仅当活动稀疏 + 事件驱动输入 + 容忍近似同时成立；三条件各打九折则总效率剩余约七成 | [二手] + [推算] |
| 07 | [软件生态现实](./notes/07-software-stack-reality.md) | 决定采用成本的是生态，不是芯片参数；这里的抽象层还在血泪期 | 无任何平台有接近 CUDA 成熟度的生态；入门以周计、上手以月计、量产以季度计 | [二手] + [推算] |
| 08 | [混合架构模式](./notes/08-hybrid-pattern.md) | 正确答案是两层：一层永远醒着但极省，一层睡着但极猛，中间由事件当信使 | Speck 空闲 <1mW、Akida 常开关键词检测 30mW；系统有效功耗由 99% 时间停在监听层决定 | [二手] + [推算] |
| 09 | [迁移到正明](./notes/09-transfer-to-zhengming.md) | 值得搬走的不是芯片，是「只报变化、容忍 80 分、学习发生在数据所在地」这条设计纪律 | 功耗锚点 <1mW / 30mW / 70mW / <1W；改造顺序为事件化→两层化→分层化 | [二手] + [推算] |
| 10 | [反例与证伪](./notes/10-falsification-when-not.md) | 这一整套思路的适用域由三条件划定，八条「不该用」比四条「该用」更值钱 | 密集 CNN 推理与 LLM 推理（内存带宽瓶颈）为原话级警告；当前画像下八条「不该」至少命中五条 | [二手] + [推算] |

## 这套笔记不主张什么

1. **不主张神经形态会取代 GPU。** 若你的任务是密集静态的（大批量图片分类），本套笔记里没有任何一颗芯片是正确答案；无一家厂商瞄准通用训练或密集推理。
2. **不主张「省 100 倍」是无条件的。** 若活动稀疏、事件驱动输入、容忍近似三条中任一不成立，能效优势即衰减；三者全不成立，优势归零。
3. **不主张神经形态能改善大模型推理能效。** LLM 推理的瓶颈是内存带宽（权重需全量流式通过计算单元），稀疏 + 存内计算的省能逻辑在这个数据流形态下施展不开。
4. **不主张 DVS 是通用相机的替代品。** 它只报变化、不报绝对亮度；纹理、颜色、文字读取类任务必须回到传统相机。
5. **不主张脉冲训练已经成熟。** 转换路线有延迟与精度损失，代理梯度训练更慢，局部规则没有全局误差信号因而精度上限不可预期；若主流框架原生支持脉冲训练且社区基准追平 ANN，三条路线的分野失去意义。
6. **不主张软件层已经可用。** 若无任何平台达到接近 CUDA 的生态成熟度，且始终没有出现跨硬件中间层，采用成本不会下台阶。
7. **不主张混合架构一定划算。** 若单次唤醒的能耗与延迟高于让主处理器低载等待，混合设计就是负收益；若端侧低功耗 NPU 持续改进到小模型常开也够省，神经形态这一层可被整体跳过。
8. **不主张这套迁移对每个人都成立。** 若当前调用量与时间开销本就极低，事件化改造的收益低于改造本身的时间成本——先量化，再动手。
9. **不主张供应商是稳定的。** 该市场已发生过整合，量产硅片为保密报价；任何把产品核心押在单一芯片上的方案，都需自行承担存续风险。
10. **不主张读者应该去采购。** 以笔记作者当前阶段（系统纯软件、任务以文本与业务逻辑为主、无常开硬件监听场景），八条「不该用」中至少五条直接命中，四条「该用」中至少三条不满足——正确动作是跟踪与思维迁移，不是立项。

## 证据纪律

- [一手]：原始论文、厂商官方文档与数据手册、本人实测。
- [二手]：综述、报道、经转述的材料，以及本套笔记标注的「交叉验证事实包」条目。
- [推算]：由已知数按明确关系推导得出，推导过程须在文中可见。
- **无源数字不得进入对外文本。** 查不到的空格保持空白，留空比编造诚实。
- 每个关键数字后必须紧跟来源等级，等级标记裸写，不用代码符号包裹。
- 被推翻是预期内的正常结果，不是失败；本套笔记的每条主张都自带证伪条件。

## 许可

MIT License. 作者署名 chenhz01。

## 同系列笔记（五个领域）

| 领域 | 仓库 | 一句话 |
|---|---|---|
| 神经形态计算与事件驱动传感 | [neuromorphic-event-sensing](https://github.com/chenhz01/neuromorphic-event-sensing) | 永远在线的智能为什么必须抛弃「帧」与全局时钟，以及省 100 倍的前提条件 |
| AI 老药新用与计算重定位 | [ai-drug-repurposing](https://github.com/chenhz01/ai-drug-repurposing) | 用计算手段给已上市分子找新适应症，以及 10-30% 的前瞻重复率边界 |
| 本地优先软件与 CRDT 同步 | [local-first-crdt](https://github.com/chenhz01/local-first-crdt) | 把数据主副本放回用户设备，冲突由数学保证收敛 |
| 后量子密码迁移 | [post-quantum-migration](https://github.com/chenhz01/post-quantum-migration) | 从 Shor 的威胁模型走到一人公司能执行的换算法顺序 |
| 软体与折纸机器人 | [soft-origami-robotics](https://github.com/chenhz01/soft-origami-robotics) | 柔性结构能否把安全、顺应与几何编程做成工程能力 |

五套笔记各自独立，互不依赖，可任选入口进入。

## 费曼速览：60 秒判断这个领域值不值得你花时间

**一句话**：像门铃——只有有人按的时候才响，不像摄像头那样每秒拍几十张照片等着你去翻；哪里不像：门铃只会告诉你「有变化了」，说不出是谁、穿什么颜色，事件相机同样只报亮度变化、不报绝对亮度。

**三个数**：

1. SynSense Speck 空闲功耗 <1mW [二手]
2. BrainChip Akida 典型功耗 30mW [二手]
3. 10×-1000× 的能效提升仅在「活动稀疏 + 事件驱动输入 + 容忍近似计算」三条件同时成立时出现 [二手] + [推算]

**不成立的三个条件**：

1. 若你的任务是密集静态推理（大批量图片分类）或 LLM 推理（瓶颈在内存带宽），本套笔记里没有一颗芯片是正确答案，能效优势归零。
2. 若出现接近 CUDA 成熟度的跨硬件软件生态与中间层，「采用成本以周计入门、以季度计量产」的现实判断失效。
3. 若端侧低功耗 NPU 持续改进到小模型常开也够省，混合架构里的神经形态这一层可被整体跳过。

这套笔记的每篇都有完整两层费曼拆解，方法见 [FEYNMAN.md](./FEYNMAN.md)。
