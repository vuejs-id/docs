---
title: Instalasi
type: guide
order: 1
vue_version: 2.5.16
gz_size: "30.90"
---

### Catatan Kompatibilitas

Vue **tidak** mendukung IE8 ke bawah, karena ini menggunakan fitur ECMAScript 5 yang tidak diduplikasi (*un-shimmable*) di IE8. Namun ini mendukung semua [peramban yang sesuai ECMAScript 5](https://caniuse.com/#feat=es5).

### Catatan Rilis

Versi stabil terakhir: {{vue_version}}

Catatan detail rilis untuk setiap versi tersedia di [GitHub](https://github.com/vuejs/vue/releases).

## Vue Devtools

Ketika menggunakan Vue, kami menyarankan untuk menginstal juga [Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools) di peramban Anda, memungkinkan Anda untuk menginspeksi dan *debug* aplikasi Vue Anda dengan antarmuka yang lebih *user-friendly*.

## Menyertakan langsung di `<script>`

Tinggal unduh dan sertakan dengan tag script. `Vue` akan teregistrasi sebagai variabel global.

<p class="tip">Jangan gunakan versi yang sudah dikecilkan selama pengembangan. Anda akan melewatkan semua peringatan untuk kesalahan umum!</p>

<div id="downloads">
  <a class="button" href="/js/vue.js" download>Versi Pengembang</a><span class="light info">Dengan peringatan penuh dan mode <i>debug</i></span>

  <a class="button" href="/js/vue.min.js" download>Versi Produksi</a><span class="light info">Peringatan dihilangkan, {{gz_size}}KB min+gzip</span>
</div>

### CDN

Untuk tujuan *prototyping* atau pembelajaran, anda bisa menggunakan versi terbaru dengan:

``` html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

Untuk produksi, kami merekomendasikan menautkan ke versi yang spesifik untuk menghindari kerusakan yang tidak diinginkan dari versi terbaru:

``` html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.0/dist/vue.js"></script>
```

Jika anda menggunakan ES Modules native, tersedia juga versi build untuk ES Modules:

``` html
<script type="module">
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.6.0/dist/vue.esm.browser.js'
</script>
```

Anda dapat menjelajahi *source* dari paket NPM di [cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue/).

Vue juga tersedia di [unpkg](https://unpkg.com/vue@{{vue_version}}/dist/vue.js) dan [cdnjs](https://cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.js) (cdnjs membutuhkan waktu untuk singkronisasi, rilis mungkin terakhir belum tersedia).

Pastikan untuk membaca tentang [perbedaan _build_ Vue](#Explanation-of-Different-Builds) dan gunakan **versi produksi** dalam situs yang Anda *publish*, mengganti `vue.js` dengan `vue.min.js`. Build ini lebih kecil dan dioptimasi untuk kecepatan dari pada pengalaman pengembangan (*development experience*).

## NPM

NPM adalah metode instalasi yang direkomendasikan ketika membangun aplikasi skala besar dengan Vue. Ini bersanding dengan baik dengan modul _bundler_ seperti [Webpack](https://webpack.js.org/) atau [Browserify](http://browserify.org/). Vue juga menyediakan perangkat pendamping untuk pemrograman [Komponen Berkas Tunggal](single-file-components.html).

``` bash
# stabil terakhir
$ npm install vue
```

## CLI

Vue menyediakan sebuah [CLI ofisial](https://github.com/vuejs/vue-cli) untuk *scaffold* untuk Aplikasi Laman Tunggal (*Single Page Application*). Juga menyediakan *build setup* untuk alur kerja _frontend_ modern. Hanya memerlukan beberapa menit untuk *setup* dan *running* dengan _hot-reload_, _lint-on-save_, dan _production-ready builds_. Lihat [dokument Vue CLI](https://cli.vuejs.org) untuk lebih detil.

<p class="tip">CLI mengasumsikan pengetahuan tentang Node.js sebelumnya dan <i>build tool</i> yang terkait. Jika Anda baru dalam Vue atau <i>build tool</i> <i>front-end</i>, kami sangat menyarankan mengikuti <a href="./">petunjuk</a> tanpa <i>build tool</i> apapun sebelum menggunakan CLI.</p>

<div class="vue-mastery"><a href="https://www.vuemastery.com/courses/real-world-vue-js/vue-cli" target="_blank" rel="noopener" title="Vue CLI">Watch a video explanation on Vue Mastery</a></div>

## Penjelasan dari Perbedaan *Build*

Di dalam [direktori `dist/` dari paket NPM](https://cdn.jsdelivr.net/npm/vue/dist/) Anda akan menemukan banyak *build* Vue.js yang berbeda. Ini gambaran umum dari perbedaan antara mereka:

| | UMD | CommonJS | ES Module (for bundlers) | ES Module (for browsers) |
| --- | --- | --- | --- | --- |
| **Full** | vue.js | vue.common.js | vue.esm.js | vue.esm.browser.js |
| **Runtime-only** | vue.runtime.js | vue.runtime.common.js | vue.runtime.esm.js | - |
| **Full (production)** | vue.min.js | - | - | vue.esm.browser.min.js |
| **Runtime-only (production)** | vue.runtime.min.js | - | - | - |

### Istilah

- **Full**: *build* yang berisi semua _compiler_ dan _runtime_.

- **Compiler**: kode yang bertanggung jawab untuk mengkompilasi string templat menjadi fungsi render _Javascript_.

- **Runtime**: kode yang bertanggung jawab untuk membuat instan Vue, _rendering_, dan _patching_ DOM virtual, dll. Pada dasarnya semua kecuali _compiler_.

- **[UMD](https://github.com/umdjs/umd)**: UMD *build* dapat langsung digunakan di browser via tag `<script>`. *Default* file dari jsDelivr CDN di [https://cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue) adalah *Runtime* + *Compiler* UMD *build* (`vue.js`).

- **[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)**: CommonJS builds are intended for use with older bundlers like [browserify](http://browserify.org/) or [webpack 1](https://webpack.github.io). The default file for these bundlers (`pkg.main`) is the Runtime only CommonJS build (`vue.runtime.common.js`).

- **[ES Module](http://exploringjs.com/es6/ch_modules.html)**: starting in 2.6 Vue provides two ES Modules (ESM) builds:

  - ESM for bundlers: intended for use with modern bundlers like [webpack 2](https://webpack.js.org) or [Rollup](https://rollupjs.org/). ESM format is designed to be statically analyzable so the bundlers can take advantage of that to perform "tree-shaking" and eliminate unused code from your final bundle. The default file for these bundlers (`pkg.module`) is the Runtime only ES Module build (`vue.runtime.esm.js`).

  - ESM for browsers (2.6+ only): intended for direct imports in modern browsers via `<script type="module">`.

### Runtime + Compiler vs. Runtime-only

If you need to compile templates on the client (e.g. passing a string to the `template` option, or mounting to an element using its in-DOM HTML as the template), you will need the compiler and thus the full build:

``` js
// this requires the compiler
new Vue({
  template: '<div>{{ hi }}</div>'
})

// this does not
new Vue({
  render (h) {
    return h('div', this.hi)
  }
})
```

When using `vue-loader` or `vueify`, templates inside `*.vue` files are pre-compiled into JavaScript at build time. You don't really need the compiler in the final bundle, and can therefore use the runtime-only build.

Since the runtime-only builds are roughly 30% lighter-weight than their full-build counterparts, you should use it whenever you can. If you still wish to use the full build instead, you need to configure an alias in your bundler:

#### Webpack

``` js
module.exports = {
  // ...
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js' for webpack 1
    }
  }
}
```

#### Rollup

``` js
const alias = require('rollup-plugin-alias')

rollup({
  // ...
  plugins: [
    alias({
      'vue': require.resolve('vue/dist/vue.esm.js')
    })
  ]
})
```

#### Browserify

Add to your project's `package.json`:

``` js
{
  // ...
  "browser": {
    "vue": "vue/dist/vue.common.js"
  }
}
```

#### Parcel

Add to your project's `package.json`:

``` js
{
  // ...
  "alias": {
    "vue" : "./node_modules/vue/dist/vue.common.js"
  }
}
```

### Development vs. Production Mode

Development/production modes are hard-coded for the UMD builds: the un-minified files are for development, and the minified files are for production.

CommonJS and ES Module builds are intended for bundlers, therefore we don't provide minified versions for them. You will be responsible for minifying the final bundle yourself.

CommonJS and ES Module builds also preserve raw checks for `process.env.NODE_ENV` to determine the mode they should run in. You should use appropriate bundler configurations to replace these environment variables in order to control which mode Vue will run in. Replacing `process.env.NODE_ENV` with string literals also allows minifiers like UglifyJS to completely drop the development-only code blocks, reducing final file size.

#### Webpack

In Webpack 4+, you can use the `mode` option:

``` js
module.exports = {
  mode: 'production'
}
```

But in Webpack 3 and earlier, you'll need to use [DefinePlugin](https://webpack.js.org/plugins/define-plugin/):

``` js
var webpack = require('webpack')

module.exports = {
  // ...
  plugins: [
    // ...
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: JSON.stringify('production')
      }
    })
  ]
}
```

#### Rollup

Use [rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace):

``` js
const replace = require('rollup-plugin-replace')

rollup({
  // ...
  plugins: [
    replace({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
}).then(...)
```

#### Browserify

Apply a global [envify](https://github.com/hughsk/envify) transform to your bundle.

``` bash
NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
```

Also see [Production Deployment Tips](deployment.html).

### CSP environments

Some environments, such as Google Chrome Apps, enforce Content Security Policy (CSP), which prohibits the use of `new Function()` for evaluating expressions. The full build depends on this feature to compile templates, so is unusable in these environments.

On the other hand, the runtime-only build is fully CSP-compliant. When using the runtime-only build with [Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple) or [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple), your templates will be precompiled into `render` functions which work perfectly in CSP environments.

## Dev Build

**Important**: the built files in GitHub's `/dist` folder are only checked-in during releases. To use Vue from the latest source code on GitHub, you will have to build it yourself!

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

Only UMD builds are available from Bower.

``` bash
# latest stable
$ bower install vue
```

## AMD Module Loaders

All UMD builds can be used directly as an AMD module.
