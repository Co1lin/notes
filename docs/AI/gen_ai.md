# Generative AI

## Basis

[【生成式AI】ChatGPT 原理剖析 (3/3) — ChatGPT 所帶來的研究問題](https://youtu.be/UsaZhQ9bY2k)

ChatGPT 带来的研究问题

- prompt engineering
- neural editing: 修复答案
- AI-generated content detection
- privacy/machine unlearning

[【生成式AI】生成式學習的兩種策略：要各個擊破，還是要一次到位](https://youtu.be/AihBniegMKg)

生成式模型的两种策略：

- Auto-regressive 各个击破
    - 每次依赖于之前的 token 输出一个 token
    - 质量高，速度慢
    - 文字
- Non-auto-regressive 一次到位
    - 同时生成所有的 token
    - 质量低，速度快
        - 质量低的原因的直观理解：前后 sample 的结果「不搭」（不是已有词汇）
    - 图像

结合两种方式的融合方法：

- 先用 AR 生成中间产物决定大方向，再用 NAR 生成最终结果（e.g. 语音合成）
- 用 N 次到位而不是一次到位：每次产生模糊的图片，多次生成，让它变得越来越清楚（diffusion model 的思想）

## Fine-tuning

[【生成式AI】能夠使用工具的AI：New Bing, WebGPT, Toolformer](https://youtu.be/ZID220t_MpI)

Toolformer ：<u>无需大量人力</u>地教 LLM 使用工具

- 使用已有 LLM 产生训练数据（节省人力）：在已有的句子中插入 API 调用语句（利用了 LLM 的 in-context learning 能力）
- 验证上一步插入的调用是否「有助于」预测正确的 token ：比较有无 API 调用预测正确 token 的概率，设定一个阈值进行筛选，保留有提升的；用保留下的数据做 fine-tuning
- Note: 模型参数量较大时，使用工具的 benefits 才能显现
- Note: 推理时，将 API 调用指令和返回结果输入 LLM ，让其基于调用结果来预测之后的 token

[【生成式AI】Finetuning vs. Prompting：對於大型語言模型的不同期待所衍生的兩類使用方式 (1/3)](https://youtu.be/F58vJcGgjt0)

对 LLM 做专门化

- 外挂
- 微调（全部参数）
- adapter 插件模组 adapterhub.ml
    - 只需要存储一份基础 LLM 来应对很多任务
    - LoRA for diffusion: Low-Rank Adaptation

## Prompting

[【生成式AI】Finetuning vs. Prompting：對於大型語言模型的不同期待所衍生的兩類使用方式 (2/3)](https://youtu.be/aZ_jXZvxyVg)

In-context learning 并不是真的在 learning ； demonstration 的主要意义是告诉 LLM 现在要做什么

- demonstration 给错误的例子，但是测试时正确率并没有大幅下降
- 但有文章反驳，说 in-context learning 能达到类似 gradient descent 的效果（？
- 越大的模型越有 in-context "learning" 的能力，越会受到 demo 中错例的影响
- classification 实验在某些情况下 LLM 性能接近 SVM
- learning in-context learning

Instruction tuning: 不是从例子获知需要做什么，而是从遵从指令

- T0 (2021, Hugging Face), FLAN (2021)
- 将一些任务转化为让机器根据人类语言描述的指令输出结果，用转化后的数据进行训练；在测试时， LLM 可以展示出在指令上的泛化能力，即可以正确执行训练时未见过的指令。

[【生成式AI】Finetuning vs. Prompting：對於大型語言模型的不同期待所衍生的兩類使用方式 (3/3)](https://youtu.be/HnzDaEiN_eg)

Chain of Thought (CoT) Prompting

- 不让 LLM 直接给出答案，而是在 demo 中呈现分析和推导过程，让 LLM 通过 in-context learning 也先分析推导再给出答案
- zero-shot-CoT: demo 中不给推导过程，而是直接下达 "Let's think step by step" 的指令
- CoT 会使 LLM 输出的 diversity 上升，于是可以做 self-consistency check （多个答案投票）
- Least-to-most prompting: 通过 demo 让 LLM 通过 in-context learning 学习将一个难题拆解为多个子问题逐步回答

自动寻找 Prompt




