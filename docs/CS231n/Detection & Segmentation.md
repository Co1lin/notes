# Detection & Segmentation

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

## Classification & Localization

Difference between object detection and **classification & localization**:

- For classification & localization, you know there are objects that you are looking for, and you know the number of it

Basic structure to tackle the task of classification & localization:

![Screen Shot 2021-04-19 at 4.35.57 PM](Detection%20&%20Segmentation.assets/Screen%20Shot%202021-04-19%20at%204.35.57%20PM.png)

Question:

- Is it ok to do the two subtasks (classification & localization) together?
  - Some people may compute the loss for each class separately, but generally speaking it works well.
- Multi-task loss (two kinds of loss)
  - Use hyper parameters to generate weighted total loss (it is difficult)
  - Or, use some final performance metric rather than just the value of loss to make choices
- How to do it based on pre-trained models like ImageNet?
  - **Freeze** the pre-trained models first
  - Train your specific model
  - Train them together

## Object detection

### Sliding window

Object detection as classification

Big problem: how to choose the location to perform the classification

Brute force: computational expensive

### Region Proposals

![Screen Shot 2021-04-19 at 4.51.35 PM](Detection%20&%20Segmentation.assets/Screen%20Shot%202021-04-19%20at%204.51.35%20PM.png)

Use proposals instead of searching for all regions.

How to propose? 

#### R-CNN

![Screen Shot 2021-04-19 at 4.56.54 PM](Detection%20&%20Segmentation.assets/Screen%20Shot%202021-04-19%20at%204.56.54%20PM.png)

![Screen Shot 2021-04-19 at 5.05.10 PM](Detection%20&%20Segmentation.assets/Screen%20Shot%202021-04-19%20at%205.05.10%20PM.png)

Taking crops from the convolutional feature map.

![Screen Shot 2021-04-19 at 5.07.05 PM](Detection%20&%20Segmentation.assets/Screen%20Shot%202021-04-19%20at%205.07.05%20PM.png)

 ![Screen Shot 2021-04-19 at 5.14.32 PM](Detection%20&%20Segmentation.assets/Screen%20Shot%202021-04-19%20at%205.14.32%20PM.png)

#### YOLO / SSD

without proposals

Single-shot Detection: do all of the detections with a single forward pass (compared with performing detections for each proposals in R-CNN)

## Instance Segmentation

### Mask R-CNN



## Semantic Segmentation

Paired training data: for each training image, **each pixel** is labeled with a semantic category.

### Convolution

An intuitive idea: encode the entire image with conv net, and do semantic segmentation on top

Problem: classification architectures often reduce feature spatial sizes to go deeper, but semantic segmentation requires the output size to be the same as input size.

### Fully Convolutional

Design a network with only convolutional layers without downsampling operators to make predictions for pixels all at once!

**Problem**: convolutions at original image resolution will be very expensive ...

Solution: Design network as a bunch of convolutional layers, with downsampling and upsampling inside the network!

![Screen Shot 2021-04-19 at 5.54.15 PM](Detection%20&%20Segmentation.assets/Screen%20Shot%202021-04-19%20at%205.54.15%20PM.png)

Unpooling:

![Screen Shot 2021-04-19 at 6.01.33 PM](Detection%20&%20Segmentation.assets/Screen%20Shot%202021-04-19%20at%206.01.33%20PM.png)

![Screen Shot 2021-04-19 at 6.02.45 PM](Detection%20&%20Segmentation.assets/Screen%20Shot%202021-04-19%20at%206.02.45%20PM.png)

### Transpose Convolution (Learnable Sampling)

![Screen Shot 2021-04-19 at 6.26.34 PM](Detection%20&%20Segmentation.assets/Screen%20Shot%202021-04-19%20at%206.26.34%20PM.png)

Issue: checkerboard artifacts

