# Generative Models

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


![Screen Shot 2021-04-25 at 3.44.33 PM](Generative%20Models.assets/Screen%20Shot%202021-04-25%20at%203.44.33%20PM.png)

Formulate as **density estimation** problems**:**

- **Explicit density estimation**: explicitly define and solve for $p_{\mathrm{model}}(x)$
- **Implicit density estimation**: learn model that can sample from $p_{\mathrm{model}}(x)$ **without explicitly defining it.**

## Explicit density

### PixelRNN

Use a chain rule to estimate a pixel based on previous pixels.

![Screen Shot 2021-04-25 at 4.04.36 PM](Generative%20Models.assets/Screen%20Shot%202021-04-25%20at%204.04.36%20PM.png)

Note that there's no labels. Just use the input data to train the probability model.

![Screen Shot 2021-04-25 at 4.06.40 PM](Generative%20Models.assets/Screen%20Shot%202021-04-25%20at%204.06.40%20PM.png)

### PixelCNN

![Screen Shot 2021-04-25 at 4.07.26 PM](Generative%20Models.assets/Screen%20Shot%202021-04-25%20at%204.07.26%20PM.png)

### Summary

![Screen Shot 2021-04-25 at 4.08.02 PM](Generative%20Models.assets/Screen%20Shot%202021-04-25%20at%204.08.02%20PM.png)

## Implicit Density

### Background: Autoencoders

![Screen Shot 2021-04-25 at 4.19.21 PM](Generative%20Models.assets/Screen%20Shot%202021-04-25%20at%204.19.21%20PM.png)

Decoder and reconstructed input data are just used to compute **loss** to train the Autoencoder.

Significance: Encoder can be used to initialize a **supervised** model.

We can use a large amount of unlabeled data to train an unsupervised model which have learned some **universal features**.

Then, we can use it to **initialize** a **supervised** model.

![Screen Shot 2021-04-25 at 4.27.05 PM](Generative%20Models.assets/Screen%20Shot%202021-04-25%20at%204.27.05%20PM.png)

### Variational Autoencoders (VAE)

Autoencoders can reconstruct data, and can learn features to initialize a supervised model

Features capture factors of variation in training data.

But we can’t generate new images from an autoencoder because we don’t know the space of z. ???

How do we make autoencoder a **generative model**?

### Generative Adversarial Networks

![Screen Shot 2021-04-25 at 5.22.52 PM](Generative%20Models.assets/Screen%20Shot%202021-04-25%20at%205.22.52%20PM.png)

![Screen Shot 2021-04-25 at 5.23.48 PM](Generative%20Models.assets/Screen%20Shot%202021-04-25%20at%205.23.48%20PM.png)

![Screen Shot 2021-04-25 at 5.28.00 PM](Generative%20Models.assets/Screen%20Shot%202021-04-25%20at%205.28.00%20PM.png)

![Screen Shot 2021-04-25 at 5.33.40 PM](Generative%20Models.assets/Screen%20Shot%202021-04-25%20at%205.33.40%20PM.png)

