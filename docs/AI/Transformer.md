# Transformer

_Click on a tile to change the color scheme_:

<div class="tx-switch">
  <button data-md-color-scheme="default"><code>default</code></button>
  <button data-md-color-scheme="slate"><code>slate</code></button>
</div>

<script>
  var buttons = document.querySelectorAll("button[data-md-color-scheme]")
  buttons.forEach(function(button) {
    button.addEventListener("click", function() {
      var attr = this.getAttribute("data-md-color-scheme")
      document.body.setAttribute("data-md-color-scheme", attr)
      var name = document.querySelector("#__code_0 code span:nth-child(7)")
      name.textContent = attr
    })
  })
</script>
(Ref: [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) [图解 Transformer](https://www.cnblogs.com/d0main/p/10164192.html))

## Seq2seq

Input a sequence, output a sequence.

**The output length is determined by model.**

Applications: too many. Selected:

syntactic parsing, multi-label classification, even object detection

## Encoder

Summary: 

- 输入，进行 positional encoding ；
- 经过多个相同结构的特征提取「模块」，包括：
    - multi-head self-attention
    - residual addition & norm
    - feed forward
    - residual addition & norm
- 最后得到输出

![Screen Shot 2021-05-10 at 7.00.04 PM](Transformer.assets/Screen%20Shot%202021-05-10%20at%207.00.04%20PM.png)

Structure of encoder:

![Screen Shot 2021-05-10 at 6.55.19 PM](Transformer.assets/Screen%20Shot%202021-05-10%20at%206.55.19%20PM.png)

Detail of a **block** in an encoder:

![Screen Shot 2021-05-10 at 6.55.38 PM](Transformer.assets/Screen%20Shot%202021-05-10%20at%206.55.38%20PM.png)

- Self-attention: build connections between input layers
- FC: **increase** the dim and them **decrease** (recover) the dim, to increase the ability of expression
- Better design: change the place of Layer Normalization

![Screen Shot 2021-05-10 at 8.40.49 PM](Transformer.assets/Screen%20Shot%202021-05-10%20at%208.40.49%20PM.png)

## Decoder

Summary:

- 输入是「顺序迭代型」的，即一开始底部输入 \<BOS\> ，输出一个 token ，然后把这个 token 作为第二个输入。
- 输入，首先是 positional encoding 。
- 然后是几层结构相同的「模块」，包括：
    - masked multi-head attention (with residual addition & norm)
    - multi-head **cross**-attention (with residual addition & norm)
    - FFN (with residual addition & norm)
- 最后通过 softmax 输出每个 token 可能性的预测。

### Autoregressive

#### Overall

![Screen Shot 2021-05-10 at 9.29.42 PM](Transformer.assets/Screen%20Shot%202021-05-10%20at%209.29.42%20PM.png)

#### Masked Self-attention

![Screen Shot 2021-05-10 at 9.43.19 PM](Transformer.assets/Screen%20Shot%202021-05-10%20at%209.43.19%20PM.png)

### Non-autoregressive

![Screen Shot 2021-05-10 at 9.45.23 PM](Transformer.assets/Screen%20Shot%202021-05-10%20at%209.45.23%20PM.png)

NAT can control the output length.

Multi-modality:

## Encoder-Decoder

![Screen Shot 2021-05-10 at 10.34.57 PM](Transformer.assets/Screen%20Shot%202021-05-10%20at%2010.34.57%20PM.png)

### Cross Attention

![Screen Shot 2021-05-10 at 10.36.04 PM](Transformer.assets/Screen%20Shot%202021-05-10%20at%2010.36.04%20PM.png)

## Training

### Teacher Forcing

using the ground truth as input.

![Screen Shot 2021-05-10 at 10.54.22 PM](Transformer.assets/Screen%20Shot%202021-05-10%20at%2010.54.22%20PM.png)

- loss: sum of cross entropy

### Copy Mechanism

Copy something from the input to the output.

chat-bot, summarization of articles, ...

### Guided Attention

Guide the way of attention to avoid stupid mistakes.

- Monotonic attention
- Location-aware attention

### Beam Search

![Screen Shot 2021-05-10 at 11.14.36 PM](Transformer.assets/Screen%20Shot%202021-05-10%20at%2011.14.36%20PM.png)

**Note**: **Randomness** is needed for **decoder** when generating sequence in some (creative) tasks.

### Optimizing Evaluation Metrics

#### BLEU score

![equation](Transformer.assets/equation.svg)

- 分子：在给定的candidate中有多少个n-gram词语出现在reference中。
- 分母：所有的candidate中n-gram的个数

We can use BLEU score in validation stage to select the best model.

**However**, it cannot be used to train because it is not differential so we can't optimize it as a loss function.

Rule: When you don’t know how to optimize, just use reinforcement learning (RL)!

### Scheduled Sampling

#### Exposure Bias

There is a **mismatch** due to the **teacher forcing** mechanism!

Scheduled Sampling: