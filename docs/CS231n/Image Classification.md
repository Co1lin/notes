# Image Classification

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

## Some Basis of ML

### Validation set

The correct way to set hyperparameters is to split your training data into two: a training set and a **fake test set**, which we call **validation set**.

### Cross-validation

If the lack of training data is a concern

![crossval](Image Classification.assets/crossval.jpg)

e.g. 5 folds Cross-validation: 每次用一个作为validation set，轮换5次，取平均

![cvplot](Image%20Classification.assets/cvplot.png)

## KNN

k - Nearest Neighbor Classifier

### Pros and Cons

Pros:

- easy to build
- may sometimes be a good choice in some settings (especially if the data is **low-dimensional**)

Cons:

- too slow to get the result of tests

### Promotion

- Approximate Nearest Neighbor (ANN)
- k-means





