---
title: Berkas Komponen Tunggal
type: guide
order: 401
---

## Pengantar

Dikebanyakan projek Vue, Komponen global akan didefinisikan menggunakan `Vue.component`, diikuti dengan `new Vue({ el: '#container' })` untuk mengarahkan ke elemen kontainer pada body setiap halaman.

Hal ini berjalan dengan sangat baik untuk projek skala kecil hingga menengah, dimana JavaScript hanya untuk meningkatkan tampilan bagian tertentu saja. Namun untuk projek yang lebih komplek, atau ketika *frontend* anda diatur sepenuhnya oleh JavaScript, masalah-masalah berikut akan muncul:

- **Definisi Global** memaksa setiap komponen mempunyai nama yang unik
- **String templates** tidak adanya *syntax highlighting* dan untuk template yang panjang (multiline HTML) membutuhkan karakter garis miring (slash) yang jelek
- **Tidak ada dukungan CSS** HTML and JavaScript dapat di-modularisasi-kan ke komponent, namun tidak dengan CSS
- **Tidak ada build step** membatasi kita hanya menggunakan HTML dan JavaScript ES5, daripada *preprocessor* seperti Pug (dulunya Jade) dan Babel

Semua masalah tersebut dapat teratasi oleh **berkas komponen tunggal** (***single-file components***) ber-ektensi `.vue`, memungkinkan kita menggunakan *build tool* seperti Webpack atau Browserify.

Berikut ini contoh dari file `Hello.vue`:

<a href="https://gist.github.com/chrisvfritz/e2b6a6110e0829d78fa4aedf7cf6b235" target="_blank" rel="noopener noreferrer"><img src="/images/vue-component.png" alt="Single-file component example (click for code as text)" style="display: block; margin: 30px auto;"></a>

Sekarang kita mendapatkan:

- [Complete syntax highlighting](https://github.com/vuejs/awesome-vue#source-code-editing)
- [CommonJS modules](https://webpack.js.org/concepts/modules/#what-is-a-webpack-module)
- [Component-scoped CSS](https://vue-loader.vuejs.org/en/features/scoped-css.html)

Sesuai yang dijanjikan, kita juga dapat menggunakan *preprocessor* seperti Pug, Babel (dengan modul ES2015), dan Stylus untuk komponen yang lebih rapi dan lebih kaya akan fitur.

<a href="https://gist.github.com/chrisvfritz/1c9f2daea9bc078dcb47e9a82e5f7587" target="_blank" rel="noopener noreferrer"><img src="/images/vue-component-with-preprocessors.png" alt="Single-file component example with preprocessors (click for code as text)" style="display: block; margin: 30px auto;"></a>

Bahasa tersebut hanya sebagai contoh. Anda dapat menggunakan Bublé, TypeScript, SCSS, PostCSS - atau *preprocessor* lainnya yang membantu produktifitas Anda. Jika menggunakan Webpack dengan `vue-loader`, juga akan mendapatkan dukungan terbaik untuk Modul CSS.

### What About Separation of Concerns?

One important thing to note is that **separation of concerns is not equal to separation of file types.** In modern UI development, we have found that instead of dividing the codebase into three huge layers that interweave with one another, it makes much more sense to divide them into loosely-coupled components and compose them. Inside a component, its template, logic and styles are inherently coupled, and collocating them actually makes the component more cohesive and maintainable.

Even if you don't like the idea of Single-File Components, you can still leverage its hot-reloading and pre-compilation features by separating your JavaScript and CSS into separate files:

``` html
<!-- my-component.vue -->
<template>
  <div>This will be pre-compiled</div>
</template>
<script src="./my-component.js"></script>
<style src="./my-component.css"></style>
```

## Getting Started

### Example Sandbox

If you want to dive right in and start playing with single-file components, check out [this simple todo app](https://codesandbox.io/s/o29j95wx9) on CodeSandbox.

### For Users New to Module Build Systems in JavaScript

With `.vue` components, we're entering the realm of advanced JavaScript applications. That means learning to use a few additional tools if you haven't already:

- **Node Package Manager (NPM)**: Read the [Getting Started guide](https://docs.npmjs.com/getting-started/what-is-npm) through section _10: Uninstalling global packages_.

- **Modern JavaScript with ES2015/16**: Read through Babel's [Learn ES2015 guide](https://babeljs.io/docs/learn-es2015/). You don't have to memorize every feature right now, but keep this page as a reference you can come back to.

After you've taken a day to dive into these resources, we recommend checking out [Vue CLI 3](https://cli.vuejs.org/). Follow the instructions and you should have a Vue project with `.vue` components, ES2015, Webpack and hot-reloading in no time!

### For Advanced Users

The CLI takes care of most of the tooling configurations for you, but also allows fine-grained customization through its own [config options](https://cli.vuejs.org/config/).

In case you prefer setting up your own build setup from scratch, you will need to manually configure webpack with [vue-loader](https://vue-loader.vuejs.org). To learn more about webpack itself, check out [their official docs](https://webpack.js.org/configuration/) and [Webpack Academy](https://webpack.academy/p/the-core-concepts).
