# Vue

## Basis

Ref: https://www.vuemastery.com/courses/intro-to-vue-3

### `index.html`

Import Vue.js in head tag via a CDN link to use Vue.

Import `main.js` in body tag to import Vue app.

Mount the app into DOM by a method `mount` that requires a DOM selector as an argument: 

```html
<script>
  const mountedApp = app.mount('#app')
</script>
```

Display the data by use div with specific id:

```html
<div id="app">
  <h1>{{ product }}</h1>
</div>
```

### `main.js`

Create a Vue app:

```js
const app = Vue.createApp({
    data() {
        return {
            product: 'Socks'
        }
    },
  	methods: {
      	update(id) {
          	this.cartList.push(id)
        }
    }
})
```

### Computed Properties

```js
data() {
  computed: {
    image() {
      return this.variants[this.selectedVariant].image
    }
  }
}
```

```html
<img v-bind:src="image">
```

### Components

#### Define

`components/ProductDisplay.js`

```js
app.component('product-display', {
	props: {
		premium: {
      type: Boolean,
      required: true
    }
	},
	
	template:
	/*html*/
	`
	<!-- ... -->
	`,
	
	data() {
		return {
			
		}
	},
	
	methods: {
		
	},
	
	computed: {
	
	}
	
})
```

#### Import

```html
<!-- Import Components -->
<script src="./components/ProductDisplay.js"></script>
```

#### Use

```html
<div id="app">
  <div class="nav-bar"></div>

  <div class="cart">Cart({{ cart }})</div>
  <product-display></product-display>	<!-- Use the component! -->
  <product-display></product-display>	<!-- Use again! -->
</div>
```

#### Props

`product-display` component needs access to data `premium` in `main.js` in order to render contents corresponding with variables in `main.js`.

In other words, it needs a custom attribute (a funnel) that we can feed this data into.

So, we add custom attribute onto the `product-display` component where we’re using it.

```html
<product-display :premium="premium"></product-display>
```

**Notice** how we’re using the shorthand for `v-bind` so we can reactively receive the new value of `premium` if it updates (from `true` to `false`).

![7.opt](Vue.assets/7.opt.jpg)

In `ProductDisplay.js`:

```js
app.component('product-display', {
  props: {
    premium: {
      type: Boolean,
      required: true
    }
  },
  // ...
```

#### Emitting and Listening

Something happened in one component, and we need to inform other components by emitting an event.

```js
methods: {
  addToCart() {
    this.$emit('add-to-cart')
  },
  // ...
 }
```

```html
<product-display :premium="premium" @add-to-cart="updateCart"></product-display>
```

### v-bind

Dynamically bind an attribute to an expression.

```html
<img v-bind:src="image"> <! -- src attribute bound to the image data -->
<img :src="image"> <! abbr./shorthand -->
```

### v-if / v-show

```html
<p v-if="inStock">In Stock</p>
<p v-else>Out of Stock</p>

<p v-show="inStock">In Stock</p>

<p v-if="inventory > 10">In Stock</p>
<p v-else-if="inventory <= 10 && inventory > 0">Almost sold out!</p>
<p v-else>Out of Stock</p>
```

### v-for

In `main.js` we have:

```js
const app = Vue.createApp({
    data() {
        return {
            ...
            details: ['50% cotton', '30% wool', '20% polyester']
        }
    }
})
```

Then,

```html
<ul>
  <li v-for="detail in details">{{ detail }}</li>
</ul>
```

#### key attribute

Bind each DOM element to corresponding list item.

```js
data() {
  return {
    ...
    variants: [
      { id: 2234, color: 'green' },
      { id: 2235, color: 'blue' }
    ]
  }
}
```

```html
<div v-for="variant in variants" :key="variant.id">{{ variant.color }}</div>
```

By saying `:key="variant.id"`, we’re using the shorthand for `v-bind` to bind the variant’s `id` to the `key` attribute.

### Event

#### Click

```html
<button class="button" v-on:click="logic to run">Add to Cart</button>

<button class="button" v-on:click="cart += 1">Add to Cart</button>

<button class="button" v-on:click="addToCart">Add to Cart</button>
```

```js
const app = Vue.createApp({
  data() {
    return {
      cart: 0,
      ...
    }
  },
  methods: {
    addToCart() {
      this.cart += 1
    }
  }
})
```

#### Mouseover

```html
<div v-for="variant in variants" :key="variant.id" @mouseover="updateImage(variant.image)">{{ variant.color }}</div>
```

### Style

Use `v-bind` to bind parameters of styles to variables:

```html
<div 
  v-for="variant in variants" 
  :key="variant.id" 
  @mouseover="updateImage(variant.image)" 
  class="color-circle" 
  :style="{ backgroundColor: variant.color }">
</div>
```

**Camel vs Kebab**

In JavaScript, `-` would have been interpreted by as a minus sign, so we can't use it as a variable's name.

```html
<div :style="{ backgroundColor: variant.color }></div>
<div :style="{ 'background-color': variant.color }></div>
```

Bind a style to an entire style object.

```html
<div> :style="styles"</div>
```

```js
data() {
	return {
		styles: {
			color: 'red'
		}
	}
}
```

### Class

```html
<button 
  class="button" 
  :class="{ disabledButton: !inStock }" 
  :disabled="!inStock" 
  @click="addToCart">
  Add to Cart
</button>
```

If `inStock == false`, then this element will have double classes, "button" and "disabledButton".

**Note**: The class that is defined later in CSS file can **override** the class that is defined before it when both of them are the classes of a single element.

Ref: https://www.zhihu.com/question/28976590

#### Ternary Operators (inline)

```html
<div> :class="[isActive ? activeClass : '']"</div>
```

(The second class is empty, no class.)

### v-model

When using a form, we need to bind from the template to the data. It is the reverse of `v-bind`.

We want to bind these input fields to their respective data properties so that when the user fills out the form, we store their data locally.

In `ReviewForm.js`, add the `v-model` directive:

```js
app.component('review-form', {
  template:
  /*html*/
  `<form class="review-form" @submit.prevent="onSubmit">
    <h3>Leave a review</h3>
    <label for="name">Name:</label>
    <input id="name" v-model="name">

    <label for="review">Review:</label>      
    <textarea id="review" v-model="review"></textarea>

    <label for="rating">Rating:</label>
    <select id="rating" v-model.number="rating">
      <option>5</option>
      <option>4</option>
      <option>3</option>
      <option>2</option>
      <option>1</option>
    </select>

    <input class="button" type="submit" value="Submit">  
  </form>`,
  data() {
    return {
      name: '',
      review: '',
      rating: null
  },
  methods: {
   onSubmit() {
     let productReview = {
       name: this.name,
       review: this.review,
       rating: this.rating,
     }
     this.$emit('review-submitted', productReview)

     this.name = ''
     this.review = ''
     this.rating = null
   }
 }
})
```

(Remember to import it.)

`ProductDisplay.js`

```js
template: 
  /*html*/
  `<div class="product-display">
    <div class="product-container">
     ...
    </div>
    <review-form @review-submitted="addReview"></review-form>
  </div>`
}),
data() {
  return {
    ...
    reviews: []
   }
 },
methods: {
  ...
  addReview(review) {
    this.reviews.push(review)
  }
},
```

**Validation**:

`ReviewForm.js`

```js
methods: {
    onSubmit() {
      if (this.name === '' || this.review === '' || this.rating === null || this.recommend === null) {
        alert('Review is incomplete. Please fill out every field.')
        return
      }

      let productReview = {
        name: this.name,
        review: this.review,
        rating: this.rating,
        recommend: this.recommend // solution

      }
      this.$emit('review-submitted', productReview)

      this.name = ''
      this.review = ''
      this.rating = null
      this.recommend = null // solution

    }
  }
```

## Vue Router

Ref: [Vue Router for Everyone](https://vueschool.io/courses/vue-router-for-everyone)

### Create a project with Vue Router using Vue CLI

Use nvm to install node.

Use npm to install vue-cli.

Use `vue ui` to create a new project with GUI.

![Screen Shot 2021-02-24 at 3.02.27 PM](Vue.assets/Screen%20Shot%202021-02-24%20at%203.02.27%20PM.png)

### Visual Structure

```
App.vue # Single page application
-- router-link
-- router-view
---- components
```

### Single File Components

#### Basic Structure of a `.vue` file

```js
<template>
  
</template>

<script>

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
```

### Create routes

Add nav-bar in main page:

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- use router-link component(already provided) for navigation. -->
    <!-- specify the link by passing the `to` prop. -->
    <!-- `<router-link>` will be rendered as an `<a>` tag by default -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- route outlet -->
  <!-- component matched by the route will render here -->
  <router-view></router-view>
</div>
```

Write routes in js:

First, we have components.

Second, we have a routes list, and third, we use it to make a router instance.

Fourth, let app use the router and mount it.

```js
// 0. If using a module system (e.g. via vue-cli), import Vue and VueRouter
// and then call `Vue.use(VueRouter)`.

// 1. Define route components.
// These can be imported from other files
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }

// 2. Define some routes
// Each route should map to a component. The "component" can
// either be an actual component constructor created via
// `Vue.extend()`, or just a component options object.
// We'll talk about nested routes later.
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

// 3. Create the router instance and pass the `routes` option
// You can pass in additional options here, but let's
// keep it simple for now.
const router = new VueRouter({
  routes // short for `routes: routes`
})

// 4. Create and mount the root instance.
// Make sure to inject the router with the router option to make the
// whole app router-aware.
const app = new Vue({
  router
}).$mount('#app')

// Now the app has started!
```

There are two ways to define routes of outsider components (like Single File Components, which can be identified by `.vue`):

In `@/router/index.js`:

```js
import Home from "../views/Home.vue";
const routes = [
  {
    path: "/",
    name: "Home",
    component: Home
  }
];
```

Recommended way (It is called lazy loading, which means the components will be loaded only when the user go to the link):

```js
const routes = [
  {
    path: "/about",
    name: "About",
    component: () =>
      import(/* webpackChunkName: "about" */ "../views/About.vue")
  }
];
```

### Named routes

If we define `name` prop., we can link routes derectly into HTML without specifying paths:

```html
<router-link :to="/about">About Page</router-link>
<!-- can be changed to -->
<router-link :to="{name: 'About'}">About Page</router-link>
```

So we can change paths later without refactoring HTML code.

### Operate `this.$router`

Create a GoBack component:

```vue
<template>
  <span class="go-back">
    <button @click="goBack">go back</button>
  </span>
</template>

<script>
export default {
  methods: {
    goBack() {
      return this.$router.go(-1);	// there are many methods provided by `this.$router`
    }
  }
};
</script>

<style scoped>
.go-back {
  display: flex;
  cursor: pointer;
}
button {
  border: 0;
}
</style>
```

### Pass Params into routed page

```html
<router-link :to="{ name: 'About', params: {id: data_list.id} }" >
About Page
</router-link>
```

In `About.vue`:

```html
<template>
	<p>The id is: {{ this.$route.params.id }}</p>
</template>
```



Before and after the snippet above, we complete the `index.js`:

```js
import { createRouter, createWebHistory } from "vue-router";

// snippet above

// History???
const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
});

// export echoes with import:
// in main.js, we have `import router from "./router";`
export default router;
// `export default {};` can be regarded as providing some interfaces in some js to other js programs
```

### Dynamic Routes

In HTML:

```html
<router-link
  :to="{
    name: 'DestinationDetails',
    params: { slug: destination.slug }
  }"
>
```

In router creating:

```
{
  path: "/destination/:slug",
  name: "DestinationDetails",
  props: true,
  component: () =>
    import(/* webpackChunkName: "DestinationDetails"*/ "./views/DestinationDetails"),
},
```

In requested component's `.vue` file:

```js
export default {
  props: {
    slug: {
      type: String,
      required: true
    }
  },
  computed: {
    destination() {
      return store.destinations.find(
        destination => destination.slug === this.slug
      );
    }
	}
}
```

If we visit router-link which is rendered to URL  `/destination/somewhere`, `/destination/:slug` will be matched, and `somewhere` will be passed to variable `slug` through **props**. Then the computed variable `destination` will be the one which has the same `slug` value.

Notice that at first we use a direct way like  `{{ this.$route.params.slug }}` to use the params, but here we use **props** to pass it.

#### Reload Issue of Dynamic Routed Components

If we load a components by dynamic routes, there will be some routers leading to the same components with different params. So if the user switch between these routes, Vue Router can't see the difference because they share the same component, and as a result the contents of `<router-view>` will not change.

So we need to add a key to ensure the contents will be reloaded accordingly:

```html
<router-view :key="$route.path" />
```

### Avoid `#` in URL

```js
const router = new Router({
  mode: "history",
  // ...
```

Use router's history mode.

### Nested Routes

In `FatherComponent.vue`:

```vue
<router-link
  :to="{
    name: 'experienceDetails',
    params: { experienceSlug: experience.slug },
    hash: '#experience'
  }"
>
{{ experience.name }}
</router-link>
<router-view :key="$route.path" />
```

In routes list in js file:

```js
{
  path: "/destination/:slug",
  name: "DestinationDetails",
  props: true,
  component: () =>
    import(/* webpackChunkName: "DestinationDetails"*/ "./views/DestinationDetails"),
  children: [
    {
      path: ":experienceSlug", /* attached behind the father's path */
      name: "experienceDetails",
      props: true,
      component: () =>
        import(/*webpackChunkName: "ExperienceDetails"*/ "./views/ExperienceDetails")
    }
  ],
  // ...
```

In `ChildrenComponent.vue`:

```vue
<script>
import store from "@/store.js";
export default {
  props: {
    slug: {
      type: String,
      required: true
    },
    experienceSlug: {
      type: String,
      required: true
    }
  },
  
  computed: {
    destination() {
      return store.destinations.find(
        destination => destination.slug === this.slug
      );
    },
    experience() {
      return this.destination.experiences.find(
        experience => experience.slug === this.experienceSlug
      );
    }
  }
};
</script>
```

### Transition

Encapsulate the router-view:

```vue
<transition name="fade" mode="out-in">
  <router-view :key="$route.path" />
</transition>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
```

### 404 Page

Start with creating a component `NotFound.vue`:

```vue
<template>
  <div>
    <h1>Not Found</h1>
    <p>
      Oops we couldn't find that page. Try going
      <router-link :to="{ name: 'home' }">home</router-link>
    </p>
  </div>
</template>
```

Add it to router:

```js
{
  path: "*", // Use asterisks to match any paths that is not matched by previous paths
  name: "notFound",
  component: () =>
    import(/* webpackChunkName: "NotFound" */
    "./views/NotFound")
}
```

We need to put it to the end, because the preceding paths have higher priority.

To aviod a warning, we should use the code below.

```
{
  path: "/404",
  alias: "*",
  name: "notFound",
  component: () =>
    import(/* webpackChunkName: "NotFound" */
    "./views/NotFound")
}
```

#### Navigation Guards

```js
path: "/destination/:slug",
name: "DestinationDetails",
// ...
beforeEnter: (to, from, next) => {
  const exists = store.destinations.find(
    destination => destination.slug === to.params.slug
  );
  if (exists) {
    next();
  } else {
    next({ name: "notFound" });
  }
}
```

In case the usr request a non-existed path of a dynamic router, we need to use the Navigation Guards to redirect it to 404.

More details: [Navigation Guards](https://router.vuejs.org/guide/advanced/navigation-guards.html)



