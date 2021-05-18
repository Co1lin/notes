# Policy Gradient

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
![Screen Shot 2021-05-12 at 2.42.35 PM](Policy%20Gradient.assets/Screen%20Shot%202021-05-12%20at%202.42.35%20PM.png)


## Definitions

- Given an actor $\pi_\theta(s)$  with network parameter $\theta$
- Use the actor to play the game to gain rewards
- Due to the **randomness** in the environment, even with the same actor, total reward is different each time
- $R_\theta$: total rewards
- $\overline{R_\theta}$: expected value of total rewards; **evaluates the goodness of an actor**

## Formulation of Average Reward

We use sampling to estimate average reward:

![Screen Shot 2021-05-12 at 2.50.01 PM](Policy%20Gradient.assets/Screen%20Shot%202021-05-12%20at%202.50.01%20PM.png)

## Optimization: Gradient Ascent

We need to optimize $\theta$ to get an optimal reward:

![Screen Shot 2021-05-12 at 2.52.56 PM](Policy%20Gradient.assets/Screen%20Shot%202021-05-12%20at%202.52.56%20PM.png)

Actually, gradient of $R$ to $\theta$ is only related to:

![Screen Shot 2021-05-12 at 2.56.01 PM](Policy%20Gradient.assets/Screen%20Shot%202021-05-12%20at%202.56.01%20PM.png)

In the trajectory, (s, a) -> (r, s')

![Screen Shot 2021-05-12 at 2.58.23 PM](Policy%20Gradient.assets/Screen%20Shot%202021-05-12%20at%202.58.23%20PM.png)

Finally, 

![Screen Shot 2021-05-12 at 3.00.48 PM](Policy%20Gradient.assets/Screen%20Shot%202021-05-12%20at%203.00.48%20PM.png)

Because 

![Screen Shot 2021-05-12 at 3.01.43 PM](Policy%20Gradient.assets/Screen%20Shot%202021-05-12%20at%203.01.43%20PM.png)

So

![Screen Shot 2021-05-12 at 3.02.08 PM](Policy%20Gradient.assets/Screen%20Shot%202021-05-12%20at%203.02.08%20PM.png)

## Summary

![Screen Shot 2021-05-12 at 3.03.01 PM](Policy%20Gradient.assets/Screen%20Shot%202021-05-12%20at%203.03.01%20PM.png)

## Tips

### Baseline

Use **average** value as baseline.

![Screen Shot 2021-05-12 at 5.34.11 PM](Policy%20Gradient.assets/Screen%20Shot%202021-05-12%20at%205.34.11%20PM.png)

### Assign Suitable Credit

It's not very fair to assign the same credit to all of the actions in a trajectory. Optimizations:

- Suffix sum of rewards
- Discount factor

![Screen Shot 2021-05-12 at 5.53.09 PM](Policy%20Gradient.assets/Screen%20Shot%202021-05-12%20at%205.53.09%20PM.png)

### Estimate by Network

We can use **advantage function** to represent $R(\tau^n) - b$ , which can be estimated by a network!

