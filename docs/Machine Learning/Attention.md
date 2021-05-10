# Self-Attention

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
![Screen Shot 2021-05-10 at 7.16.44 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%207.16.44%20PM.png)

Objective:

![Screen Shot 2021-05-10 at 7.27.03 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%207.27.03%20PM.png)

## Structure

### Attention score

Compute attention score $\alpha$:

![Screen Shot 2021-05-10 at 7.32.26 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%207.32.26%20PM.png)

- query, key and value
- use **query** to perform a query, i.e. query multiplies each of the **key** of others, to produce a weight **parameter $\alpha$** which is then used to "weigh" the **values**.

### Extract info

![Screen Shot 2021-05-10 at 7.33.18 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%207.33.18%20PM.png)

### Presented by Matrix multiplication

![Screen Shot 2021-05-10 at 7.49.05 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%207.49.05%20PM.png)

(From input I to output O.)

## Multi-head Self-attention

Concept: different types of relevance.

![Screen Shot 2021-05-10 at 7.51.44 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%207.51.44%20PM.png)

![Screen Shot 2021-05-10 at 7.52.17 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%207.52.17%20PM.png)

### Positional Encoding

![Screen Shot 2021-05-10 at 7.55.46 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%207.55.46%20PM.png)

Add a vector to each input embedding, which helps the model to:

- determine the position of each word
- determine the distance between different words in the sequence

![Screen Shot 2021-05-10 at 8.59.10 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%208.59.10%20PM.png)

## Relationship with Other models

### CNN

CNN is a subset of self-attention. So we can select the better one according to **the size of dataset**!

![Screen Shot 2021-05-10 at 8.05.30 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%208.05.30%20PM.png)

### RNN

Self-attention can replace RNN nowadays due to its advantages:

- RNN(LSTM) cannot **remember** effectively when the sequence is relatively long. However, self-attention builds connections between every input vectors.
- Though self-attention seems more computational expensive, it is parallel! So actually it is faster than RNN (which is not parallel) with GPUs.

![Screen Shot 2021-05-10 at 8.10.33 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%208.10.33%20PM.png)

### GNN

![Screen Shot 2021-05-10 at 8.18.18 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%208.18.18%20PM.png)

### More

![Screen Shot 2021-05-10 at 8.29.25 PM](Attention.assets/Screen%20Shot%202021-05-10%20at%208.29.25%20PM.png)





