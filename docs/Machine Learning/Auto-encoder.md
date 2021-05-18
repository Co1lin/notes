# Auto-encoder

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
## Overall

![Screen Shot 2021-05-11 at 11.31.24 AM](Auto-encoder.assets/Screen%20Shot%202021-05-11%20at%2011.31.24%20AM.png)

![Screen Shot 2021-05-11 at 11.35.49 AM](Auto-encoder.assets/Screen%20Shot%202021-05-11%20at%2011.35.49%20AM.png)

## Feature Disentangle

![Screen Shot 2021-05-11 at 11.38.35 AM](Auto-encoder.assets/Screen%20Shot%202021-05-11%20at%2011.38.35%20AM.png)

Application: Voice Conversion

![Screen Shot 2021-05-11 at 11.40.09 AM](Auto-encoder.assets/Screen%20Shot%202021-05-11%20at%2011.40.09%20AM.png)

## Discrete Latent Representation

![Screen Shot 2021-05-11 at 11.46.58 AM](Auto-encoder.assets/Screen%20Shot%202021-05-11%20at%2011.46.58%20AM.png)

- The discrete, finite options in the middle represent **basic features**.
- If we use other forms rather than vectors in the middle, we can get some human readable summaries!
- Use a **discriminator** to force the decoder to generate human readable summaries. (equipped with RL)

## Anomaly Detection

![Screen Shot 2021-05-11 at 11.57.39 AM](Auto-encoder.assets/Screen%20Shot%202021-05-11%20at%2011.57.39%20AM.png)

To address the problem that it is hard to collect abnormal data if we want to use classifiers.

![Screen Shot 2021-05-11 at 11.58.09 AM](Auto-encoder.assets/Screen%20Shot%202021-05-11%20at%2011.58.09%20AM.png)

![Screen Shot 2021-05-11 at 11.58.54 AM](Auto-encoder.assets/Screen%20Shot%202021-05-11%20at%2011.58.54%20AM.png)

