# RNN

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
Process sequences and build models with various inputs or outputs:

- 1 to many
- many to 1
- many to many

## Basis

![Screen Shot 2021-04-18 at 1.16.41 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%201.16.41%20AM.png)



![Screen Shot 2021-04-18 at 1.23.28 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%201.23.28%20AM.png)

- Process entries of vector x as a sequence
- Add dense layer taking all of the h as input to yield an output

### vanilla RNN

![Screen Shot 2021-04-18 at 1.25.57 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%201.25.57%20AM.png)

#### Character-level Language Model

![Screen Shot 2021-04-18 at 1.30.18 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%201.30.18%20AM.png)

During tests:

![Screen Shot 2021-04-18 at 1.33.30 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%201.33.30%20AM.png)

Question:

- Why <u>sampling according to the probability distribution given by softmax</u>, instead of taking the letter with the highest score?
  - Softmax sampling increases the diversity of the outputs

#### Truncated BP through time

![Screen Shot 2021-04-18 at 1.43.05 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%201.43.05%20AM.png)

This is like a sliding window mechanism. We only focus on the data in a window during an iteration.

### Case study

We want the model to learn to predict the following characters, but it also learns many things about the **structural** features of the input data.

#### Image to description



![Screen Shot 2021-04-18 at 9.45.58 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%209.45.58%20AM.png)

- Input: image
- Output: description of the image
- image -> CNN -> summary vector (4096 dim vector $\vec{v}$, instead of softmax) -> RNN -> words
- add image info by adding v and a 3rd weight matrix into the RNN model![Screen Shot 2021-04-18 at 9.49.57 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%209.49.57%20AM.png)

- Get a distribution of every words in the vocabulary, and sample from it.
  - The input of the 1st step is a START token.
  - The sample result serves as the input at the next step.
  - Once sampling a END token, stop generation.
- Available dataset: COCO from Microsoft

#### Attention

RNN focuses its attention at a **different** spatial location when generating **each** word.

![Screen Shot 2021-04-18 at 10.06.11 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%2010.06.11%20AM.png)

- Input: Weighted features & sampled word
- Output: distribution over locations & distribution over vocab
- $a_i$: vectors of attention, telling the model where to focus (generating weighted features)
- Soft attention: weighted distribution over all locations
- Hard attention: force the model to select exactly **one** location
  - Problem: **NOT a differentiable function**
  - <u>Solution</u>: 
- Pros: the model can focus on the meaningful part in the image

#### Visual Question Answering: RNN with attention

![Screen Shot 2021-04-18 at 10.19.56 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%2010.19.56%20AM.png)

- How to combine the encoded image vector with encoded question vector?
  - Most common: Connect / concatenate them together directly and feed them into fully connected layers
  - Sometimes: Vector multiplication
- Input?
- Output?

### Multilayer RNN

![Screen Shot 2021-04-18 at 10.21.54 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%2010.21.54%20AM.png)

Usually 2~4 layers for RNN is good enough.

### Gradient flow

Problem: too many W in the gradient! (especially for $h_0$)

#### Gradient explosion and vanishing gradient

![Screen Shot 2021-04-18 at 10.32.31 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%2010.32.31%20AM.png)

#### LSTM (Long Short Term Memory)

Four gates:

![Screen Shot 2021-04-18 at 10.40.06 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%2010.40.06%20AM.png)

$c_t$: Cell state

$h_t$: Hidden state

![Screen Shot 2021-04-18 at 10.44.42 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%2010.44.42%20AM.png)

Pros compared with vanilla RNN:

![Screen Shot 2021-04-18 at 10.50.38 AM](RNN.assets/Screen%20Shot%202021-04-18%20at%2010.50.38%20AM.png)

- Forget gate can vary from each time step, unlike the W is consistent in the vanilla RNN. So the model can avoid gradient explosion or vanishing.
- Sigmoid for f, so the output falls in $(0, 1)$.

[漫谈LSTM系列的梯度问题](https://zhuanlan.zhihu.com/p/36101196)

[LSTM单元梯度的详细的数学推导](https://blog.csdn.net/deephub/article/details/107033684)

#### GRU

Use only one gate to balance the history and the new data.

Performs similarly to LSTM but is computationally cheaper.

![image-20210418161549195](RNN.assets/image-20210418161549195.png)

