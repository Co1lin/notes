# GPT

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
## Training

![Screen Shot 2021-05-11 at 11.12.23 AM](GPT.assets/Screen%20Shot%202021-05-11%20at%2011.12.23%20AM.png)

## Usage

### X-shot Learning

“Incontext” Learning

![Screen Shot 2021-05-11 at 11.15.52 AM](GPT.assets/Screen%20Shot%202021-05-11%20at%2011.15.52%20AM.png)

### Beyond Text

![Screen Shot 2021-05-11 at 11.18.00 AM](GPT.assets/Screen%20Shot%202021-05-11%20at%2011.18.00%20AM.png)

