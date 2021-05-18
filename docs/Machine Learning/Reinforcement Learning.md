

# Reinforcement Learning

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
## Mathematical Formulation

### Markov Decision Process

![Screen Shot 2021-04-25 at 8.02.02 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%208.02.02%20PM.png)

![Screen Shot 2021-04-25 at 8.02.26 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%208.02.26%20PM.png)

![Screen Shot 2021-04-25 at 8.09.23 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%208.09.23%20PM.png)

### Value function and Q-value function

![Screen Shot 2021-04-25 at 8.50.34 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%208.50.34%20PM.png)

![Screen Shot 2021-04-25 at 8.51.15 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%208.51.15%20PM.png)

### Q-learning

![Screen Shot 2021-04-25 at 8.43.46 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%208.43.46%20PM.png)

**Value iteration algorithm**: Use Bellman equation as an iterative update

![Screen Shot 2021-04-25 at 8.46.59 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%208.46.59%20PM.png)

Qi will converge to Q* as i -> infinity !

**What’s the problem with this?**

Not scalable. Must compute Q(s,a) **for every state-action pair**. If state is e.g. current game state pixels, computationally infeasible to compute for entire state space!

**Solution**: use a function **approximator** to estimate Q(s,a). E.g. a neural network!

### Deep Q-learning

![Screen Shot 2021-04-25 at 8.55.08 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%208.55.08%20PM.png)

![Screen Shot 2021-04-25 at 8.59.49 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%208.59.49%20PM.png)

#### Q-network

##### Architecture

![Screen Shot 2021-04-25 at 9.03.54 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%209.03.54%20PM.png)

##### Experience Replay

![Screen Shot 2021-04-25 at 9.22.57 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%209.22.57%20PM.png)

 

Example: a robot grasping an object has a very high-dimensional state => hard to learn exact value of every (state, action) pair

But the policy can be much simpler: just close your hand
**Can we learn a policy directly, e.g. finding the best policy from a collection of policies?**

## Policy Gradients & REINFORCE Algorithm

Find the optimal policy without estimating the Q-value.

![Screen Shot 2021-04-25 at 9.35.40 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%209.35.40%20PM.png)

![Screen Shot 2021-04-25 at 9.39.47 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%209.39.47%20PM.png)

![Screen Shot 2021-04-25 at 9.41.06 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%209.41.06%20PM.png)

???

![Screen Shot 2021-04-25 at 9.44.44 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%209.44.44%20PM.png)

## Variance reduction

![Screen Shot 2021-04-25 at 10.32.09 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%2010.32.09%20PM.png)

![Screen Shot 2021-04-25 at 10.32.30 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%2010.32.30%20PM.png)

## Actor-Critic Algorithm

![Screen Shot 2021-04-25 at 10.37.31 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%2010.37.31%20PM.png)

![Screen Shot 2021-04-25 at 10.43.52 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%2010.43.52%20PM.png)

## Recurrent Attention Model

![Screen Shot 2021-04-25 at 10.52.51 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%2010.52.51%20PM.png)

![Screen Shot 2021-04-25 at 11.02.00 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%2011.02.00%20PM.png)

## AlphaGo

![Screen Shot 2021-04-25 at 11.09.35 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%2011.09.35%20PM.png)

## Summary

![Screen Shot 2021-04-25 at 11.21.35 PM](Reinforcement%20Learning.assets/Screen%20Shot%202021-04-25%20at%2011.21.35%20PM.png)

