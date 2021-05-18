# BERT

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

## Self-supervised Learning

The system learns to predict part of its input from **other parts of its input**. A portion of the input is used as a supervisory signal.

## Masking Input

 ![Screen Shot 2021-05-11 at 12.12.20 AM](BERT.assets/Screen%20Shot%202021-05-11%20at%2012.12.20%20AM.png)

## Next Sentence Prediction & Sentence Order Prediction

NSP is not helpful.

SOP works.

![Screen Shot 2021-05-11 at 12.16.03 AM](BERT.assets/Screen%20Shot%202021-05-11%20at%2012.16.03%20AM.png)

**Note**: CLS: a token used for classification

## Pre-train & Fine-tune

![Screen Shot 2021-05-11 at 12.17.19 AM](BERT.assets/Screen%20Shot%202021-05-11%20at%2012.17.19%20AM.png)

How to use?

### Text Classification

![Screen Shot 2021-05-11 at 12.24.54 AM](BERT.assets/Screen%20Shot%202021-05-11%20at%2012.24.54%20AM.png)

### Extraction-based Q&A

![Screen Shot 2021-05-11 at 12.39.04 AM](BERT.assets/Screen%20Shot%202021-05-11%20at%2012.39.04%20AM.png)

![Screen Shot 2021-05-11 at 12.39.20 AM](BERT.assets/Screen%20Shot%202021-05-11%20at%2012.39.20%20AM.png)

- Only need to randomly initialize two vectors (for beginning and ending).

### seq2seq

![Screen Shot 2021-05-11 at 12.52.38 AM](BERT.assets/Screen%20Shot%202021-05-11%20at%2012.52.38%20AM.png)

Ways of corruption:

![Screen Shot 2021-05-11 at 12.53.18 AM](BERT.assets/Screen%20Shot%202021-05-11%20at%2012.53.18%20AM.png)

Comparison of these ways: T5 & C4

## General Language Understanding Evaluation

https://gluebenchmark.com/

## BERT Embryology

When does BERT know POS tagging, syntactic parsing, semantics?

## Features & Interesting things

### Contextualized word embedding



### Protein

![Screen Shot 2021-05-11 at 9.52.06 AM](BERT.assets/Screen%20Shot%202021-05-11%20at%209.52.06%20AM.png)

### Multi-lingual BERT

Training a BERT model by many different languages.

![Screen Shot 2021-05-11 at 9.56.58 AM](BERT.assets/Screen%20Shot%202021-05-11%20at%209.56.58%20AM.png)

#### Language Information

![Screen Shot 2021-05-11 at 10.08.09 AM](BERT.assets/Screen%20Shot%202021-05-11%20at%2010.08.09%20AM.png)

