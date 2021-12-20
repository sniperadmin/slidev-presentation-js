---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/8680465/1920x1080
# apply any windi css classes to the current slide
class: 'text-center bg-blend-darken'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: true
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---
<h1 class="text-amber-500 bg-gradient-to-r from-gray-700 rounded-lg">
  Why are there JS frameworks?
</h1>

<div v-motion-slide-visible-right grid="~ cols-1" class="place-items-auto bg-gradient-to-r rounded-lg from-gray-800 p-2">
  <div class="col-span-1">
    Presentation slides for developers
  </div>

  <div grid="~ cols-1" class="mt-4 mb-4">
    <img
      src="https://raw.githubusercontent.com/sniperadmin/vue-compilation-performance/master/src/assets/nash.jpg"
      class="rounded-1/2 justify-self-center"
      width="50"
    />
  </div>

  <div>
    By: Nasr Galal
  </div>
</div>


<div
  v-if="$slidev.nav.currentPage === 1"
  v-motion
  :initial="{ x: -80, opacity: 0 }"
  :enter="final"
  class="pt-12"
>
  <span
    @click="$slidev.nav.next"
    class="px-2 py-1 rounded cursor-pointer bg-gradient-to-r rounded-lg from-gray-800"
    hover="bg-white bg-opacity-10"
  >
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/sniperadmin" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<script setup lang="ts">
const final = {
  x: 0,
  y: 0,
  rotate: 0,
  scale: 1,
  opacity: 1,
  transition: {
    delay: 100,
    duration: 1000,
  }
}
</script>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# Modern front-end?

To create a web app nowadays, the concept became more complicated. We, the developers must follow the standards of SPA, SSR, routing and navigation, performance, .... etc

That's why the idea of creating a framework came up! let's explore the following features:

- 📝 **Hot Module Replacement (HMR)** - Auto-reflects the DOM changes and updates.
- 🎨 **Component design pattern (component system)** - Helps for the separation of concerns during development process
- 🧑‍💻 **Single Page Application (SPA) Routing** - consumes the browser router API for faster and efficient navigation
- 🤹 **Large scale state management** - For handling the data inputs and outputs in a single place and achieve client side data persistence
- 📤 **Build system** - For scaffolding the app for production version

<br>
<br>

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: two-cols
clicks: 8
---

# SPA Routing (JS)

<div class="pr-4">

HTML

```html {1|2-6|none}
<section id="app" class="card"></section>
<nav>
  <a href="#/">Home</a> -
  <a href="#/page1">Page 1</a> -
  <a href="#/page2">Page 2</a> - 
</nav>
```

<span class="text-gray-500">
Home Component
</span>

```js {none|1,10|2-9|none}
const HomeComponent = {
  render: () => {
    return `
      <section>
        <h1>Home</h1>
        <p>This is Home page</p>
      </section>
    `;
  }
};
```

</div>

::right::

<div class="pt-12">

```js {none|1-10|3-8|12-21|14-19}
const Page1Component = {
  render: () => {
    return `
      <section>
        <h1>Page 1</h1>
        <p>This is Page 1</p>
      </section>
    `;
  }
};

const Page2Component = {
  render: () => {
    return `
      <section>
        <h1>Page 2</h1>
        <p>This is Page 2</p>
      </section>
    `;
  }
};
```

</div>

<style>
.footnotes-sep {
  @apply mt-20 opacity-10;
}
.footnotes {
  @apply text-sm opacity-75;
}
.footnote-backref {
  display: none;
}
</style>
---

# SPA routing result

<div grid="~ cols-3 gap-4">
<div class="col-span-2">


  ```js
  // Routes
  const routes = [
    { path: "/", component: HomeComponent },
    { path: "/page1", component: Page1Component },
    { path: "/page2", component: Page2Component }
  ];
  // Using Hash approach
  const parseLocation = hash => hash.slice(1).toLowerCase() || "/";

  const findComponentByPath = path =>
    routes.find(r => r.path.match(new RegExp(`^\\${path}$`, "gm"))) || false;

  const router = () => {
    console.log(document.location)
    const path = parseLocation(document.location.hash);
    const { component = ErrorComponent } =
      findComponentByPath(path, routes) || {};
    document.getElementById("app").innerHTML = component.render();
  };

  window.addEventListener("hashchange", router);
  window.addEventListener("load", router) || router();
  ```
</div>
<div>

<iframe src="https://codepen.io/nasr3090/full/KKdRJrQ" width="100%" height="100%" />
</div>
</div>

---
layout: two-cols
clicks: 4
---

# vDOM

Our DOM
```js {none|1-3|5|none}
const vdom = h('div', { class: 'red' }, [
  h('span', null, 'hello')
])

mount(vdom, document.getElementById('app'))
```

The render function
```js {none|1,3|2|all}
function h(tag, props, children) {
  return { tag, props, children }
}
```


::right::

<div class="pt-12 h-auto pl-2" grid="~ cols-2 gap-3">
  <div class="col-span-2 mb-5">
  Mounting

  ```js
  function mount(vnode, container) {}
  ```
  </div>

  <div class="text-red-500 border self-center col-span-2 p-5 mt-20 bg-white-500">
    <span>hello</span>
  </div>
</div>

---
layout: center
clicks: 11
---

# The Mount

```js {1,23|1,2,22,23|1-5,11,22,23|1-6,10,11,22,23|1-7,10,11,22,23|1-11,22,23|1,2,22,23|1,2,12,13,21-23|1,2,12,13,14,16,20-23|1,2,12-16,20-23|1,2,12-23|all}
function mount(vnode, container) {
  const el = vnode.el = document.createElement(vnode.tag)

  // Props
  if (vnode.props) {
    for (const key in vnode.props) {
      const value = vnode.props[key]
      // Assuming that all props are attributes for simplicity
      el.setAttribute(key, value)
    }
  }
  //  Children
  if (vnode.children) {
    if (typeof vnode.children === 'string') {
      el.textContent = vnode.children
    } else {
      vnode.children.forEach(child => {
        mount(child, el)
      })
    }
  }
  container.appendChild(el)
}
```

::right::

# meow

---
layout: center
preload: false
---

# Presentation Powered by

<div class="w-60 relative mt-6">
  <div class="relative w-40 h-40">
    <img
      v-motion
      :initial="{ x: 800, y: -100, scale: 1.5, rotate: -50 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-square.png"
    />
    <img
      v-motion
      :initial="{ y: 500, x: -100, scale: 2 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-circle.png"
    />
    <img
      v-motion
      :initial="{ x: 600, y: 400, scale: 2, rotate: 100 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-triangle.png"
    />
  </div>

  <div
    class="text-5xl absolute top-14 left-40 text-[#2B90B6] -z-1"
    v-motion
    :initial="{ x: -80, opacity: 0}"
    :enter="{ x: 0, opacity: 1, transition: { delay: 2000, duration: 1000 } }">
    Slidev
  </div>
</div>

<!-- vue script setup scripts can be directly used in markdown, and will only affects current page -->
<script setup lang="ts">
const final = {
  x: 0,
  y: 0,
  rotate: 0,
  scale: 1,
  transition: {
    type: 'spring',
    damping: 10,
    stiffness: 20,
    mass: 2
  }
}
</script>

