# CNN

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
![Screen Shot 2021-05-24 at 7.04.11 PM](CNN.assets/Screen%20Shot%202021-05-24%20at%207.04.11%20PM.png)



![Screen Shot 2021-05-24 at 7.05.03 PM](CNN.assets/Screen%20Shot%202021-05-24%20at%207.05.03%20PM.png)

总结：

- 每个输出通道对应一个卷积核。
- 对于每个卷积核，其通道数等于输入数据的通道数， i.e. 输入的每个通道用不同的参数卷积。
- 对每个卷积核，其最后输出的一个通道是每个输入的每个通道卷积之后做加法的结果。



## Visualizing and Understanding

### First Layer

Find edges between different colors.

### Last Layer: Dimensionality Reduction

t-SNE

