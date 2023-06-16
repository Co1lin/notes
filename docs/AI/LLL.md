# Life Long Learning

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
## Knowledge Retention but NOT Intransigence

Catastrophic Forgetting

### Elastic Weight Consolidation

![Screen Shot 2021-05-11 at 8.24.40 PM](LLL.assets/Screen%20Shot%202021-05-11%20at%208.24.40%20PM.png)

Guard $b_i$ is what?

Possible one: 2nd derivative

![Screen Shot 2021-05-11 at 8.31.55 PM](LLL.assets/Screen%20Shot%202021-05-11%20at%208.31.55%20PM.png)

