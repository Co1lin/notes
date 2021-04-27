

# Basis

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
## General Guidelines

![Screen Shot 2021-04-27 at 10.59.51 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%2010.59.51%20AM.png)

### Issues of model complexity and optimization

How to realize the optimization issue?

![Screen Shot 2021-04-27 at 11.22.45 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%2011.22.45%20AM.png)

### Tackle overfitting

- Less parameters, sharing parameters

- Less features

- Early stopping

- Regularization

- Dropout

- ### Validation set

  The correct way to set hyperparameters is to split your training data into two: a training set and a **fake test set**, which we call **validation set**.

  ### Cross-validation

  If the lack of training data is a concern

  ![crossval](Basis.assets/crossval.jpg)

  e.g. 5 folds Cross-validation: 每次用一个作为validation set，轮换5次，取平均

  ![cvplot](Image%20Classification.assets/cvplot.png)


### Validation set

The correct way to set hyperparameters is to split your training data into two: a training set and a **fake test set**, which we call **validation set**.

#### Cross-validation

If the lack of training data is a concern

![crossval](Basis.assets/crossval-9495595.jpg)

e.g. 5 folds Cross-validation: 每次用一个作为validation set，轮换5次，取平均

![cvplot](Basis.assets/cvplot-9495595.png)



## Data Preprocessing

**Note**:

> It is very important to zero-center the data, and it is common to see normalization of every pixel as well.

> **Common pitfall.** An important point to make about the preprocessing is that any preprocessing statistics (e.g. the data mean) must only be computed on the training data, and then applied to the validation / test data.
> E.g. computing the mean and subtracting it from every image across the entire dataset and then splitting the data into train/val/test splits would be a mistake.
> Instead, the mean must be computed only over the training data and then subtracted equally from all splits (train/val/test).

### Mean subtraction

`X -= np.mean(X, axis = 0)`

### Normalization

After zero-centered, we can: `X /= np.std(X, axis = 0)`

### PCA and Whitening

### Data Augmentation

Clipping, rotating, ...

## Weight Initialization

**Pitfall: all zero initialization**.

> This turns out to be a mistake, because if every neuron in the network computes the same output, then they will also all compute the same gradients during backpropagation and undergo the exact same parameter updates. In other words, there is no source of asymmetry between neurons if their weights are initialized to be the same.

### Small random numbers

`W = 0.01* np.random.randn(D,H)`

, where `randn` samples from a **zero mean, unit standard deviation gaussian**.

**Warning: small is not always good!**

> For example, a Neural Network layer that has very small weights will during backpropagation compute very small gradients on its data (since this gradient is proportional to the value of the weights). This could greatly **diminish** the “gradient signal” flowing backward through a network, and could **become a concern for deep networks**.

### Xavier Initialization - Calibrating the variances

Common: `w = np.random.randn(n) / sqrt(n)`, where `n` is the number of its inputs.

`w = np.random.randn(n) * sqrt(2.0/n)` is the current recommendation for use in practice in the specific case of neural networks with **ReLU neurons**.

![Screen Shot 2021-04-27 at 10.06.42 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%2010.06.42%20AM.png)

#### For ReLU

![Screen Shot 2021-04-27 at 10.09.59 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%2010.09.59%20AM.png)

## Loss function

### Multiclass SVM Loss (Hinge Loss)

![Screen Shot 2021-04-27 at 12.46.07 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%2012.46.07%20AM.png)

"1" can be replaced by other values.

The essence of SVM loss is that the score of the correct label needs to be greater than other scores by 1.

### Softmax and Cross-entropy

![Screen Shot 2021-04-27 at 2.41.08 PM](Basis.assets/Screen%20Shot%202021-04-27%20at%202.41.08%20PM.png)

### Regularization



## Optimization

### SGD

![Screen Shot 2021-04-27 at 1.31.31 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%201.31.31%20AM.png)

### Local minima and Saddle point

![Screen Shot 2021-04-27 at 12.09.24 PM](Basis.assets/Screen%20Shot%202021-04-27%20at%2012.09.24%20PM.png)

A naive way to escape saddle point. Seldom used!

![Screen Shot 2021-04-27 at 12.12.41 PM](Basis.assets/Screen%20Shot%202021-04-27%20at%2012.12.41%20PM.png)

![Screen Shot 2021-04-27 at 12.53.09 PM](Basis.assets/Screen%20Shot%202021-04-27%20at%2012.53.09%20PM.png)

### Minibatch

epoch: see all the batches once

shuffle for every epoch

![Screen Shot 2021-04-27 at 1.21.15 PM](Basis.assets/Screen%20Shot%202021-04-27%20at%201.21.15%20PM.png)

### Momentum

![Screen Shot 2021-04-27 at 1.45.03 PM](Basis.assets/Screen%20Shot%202021-04-27%20at%201.45.03%20PM.png)

### Learning rate

**Learning rate cannot be one-size-fits-all!**

#### Adam Optimizer: RMSProp + Momentum

![Screen Shot 2021-04-27 at 2.05.58 PM](Basis.assets/Screen%20Shot%202021-04-27%20at%202.05.58%20PM.png)

#### Learning rate scheduling

##### Learning rate decay

![Screen Shot 2021-04-27 at 2.09.30 PM](Basis.assets/Screen%20Shot%202021-04-27%20at%202.09.30%20PM.png)

##### Warm up

![Screen Shot 2021-04-27 at 2.20.53 PM](Basis.assets/Screen%20Shot%202021-04-27%20at%202.20.53%20PM.png)

## Activation

### ReLU

![Screen Shot 2021-04-27 at 1.43.45 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%201.43.45%20AM.png)

So we want input data with mean 0!

### Sigmoid

![Screen Shot 2021-04-27 at 1.44.15 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%201.44.15%20AM.png)

### Leaky ReLU

![Screen Shot 2021-04-27 at 1.48.48 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%201.48.48%20AM.png)

### PReLU

$\alpha$ is not hard-coded! It can be learned!

### ELU

![Screen Shot 2021-04-27 at 9.20.03 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%209.20.03%20AM.png)



### SELU

![Screen Shot 2021-04-27 at 9.20.23 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%209.20.23%20AM.png)

### Maxout

![Screen Shot 2021-04-27 at 9.21.06 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%209.21.06%20AM.png)

### Swish

![Screen Shot 2021-04-27 at 9.23.14 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%209.23.14%20AM.png)

## Batch Normalization

![Screen Shot 2021-04-27 at 2.57.17 PM](Basis.assets/Screen%20Shot%202021-04-27%20at%202.57.17%20PM.png)

Summary:

![Screen Shot 2021-04-27 at 10.30.27 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%2010.30.27%20AM.png)

![Screen Shot 2021-04-27 at 10.31.05 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%2010.31.05%20AM.png)

![Screen Shot 2021-04-27 at 10.31.19 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%2010.31.19%20AM.png)

## Transfer Learning

![Screen Shot 2021-04-27 at 10.43.52 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%2010.43.52%20AM.png)

![Screen Shot 2021-04-27 at 10.45.01 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%2010.45.01%20AM.png)

![Screen Shot 2021-04-27 at 10.45.51 AM](Basis.assets/Screen%20Shot%202021-04-27%20at%2010.45.51%20AM.png)

