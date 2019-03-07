---
title: Registrasi Komponen
type: guide
order: 101
---

> Halaman ini menganggap anda telah membaca [Komponen Dasar](components.html). Baca itu dahulu jika anda baru pada komponen.

## Nama Komponen

Ketika mendaftarkan sebuah komponen, itu akan diberi sebuah nama. Sebagai contoh, didalam sebuah daftar global kita telah melihat:

```js
Vue.component('my-component-name', { /* ... */ })
```

Nama sebuah komponen adalah argumen pertama dari `Vue.component`.

Nama yang anda berikan ke komponen mungkin tergantung pada dimana anda akan menggunakanya. Ketika menggunakan komponen secara langsung didalam DOM ( sebagai lawan didalam *template string* atau [komponen *single-file*](single-file-components.html)), kita sangat merekomendasikan untuk mengikuti [aturan W3C](https://html.spec.whatwg.org/multipage/custom-elements.html#valid-custom-element-name) untuk membuat nama tag *custom* (semuanya huruf kecil, harus berisi tanda penghubung). Ini membantu anda untuk menghindari konflik dengan elemen HTML sekarang dan kedepanya.

Anda bisa melihat rekomendasi lainya untuk nama nama komponen di [Aturan Gaya](../style-guide/#Base-component-names-strongly-recommended).

### Bingkai Nama (Name Casing)

Anda mempunyai dua pilihan ketika mendefinisikan nama komponen:

#### Dengan *kebab-case*

```js
Vue.component('nama-komponen-saya', { /* ... */ })
```

Ketika mendefinisikan sebuah komponen dengan *kebab-case*, anda juga harus menggunakan *kebab-case* ketika memberi referensi ke elemen *custom*, seperti di `<nama-komponen-saya>`.

#### Dengan *PascalCase*

```js
Vue.component('NamaKomponenSaya', { /* ... */ })
```

Ketika mendefinisikan sebuah komponen dengan *PascalCase*, anda dapat menggunakan kedua hal ketika memberi referensi ke *custom* elemenya. Itu berarti, kedua `<nama-komponen-saya>` dan `<NamaKomponenSaya>` dapat diterima. Catatan, meskipun hanya nama *kebab-case* yang valid secara langsung di DOM (contoh : *templates non-string*).

## Registrasi Global

Sejauh ini, kita hanya membuat komponen menggunakan `Vue.component`:

```js
Vue.component('nama-komponen-saya', {
  // ... pilihan ...
})
```

Komponen ini **teregistrasi secara global**. Itu berarti mereka dapat digunakan didalam *template* dari semua *root Vue Instance* (`new Vue`) dibuat setelah diregistrasi. Sebagai contoh:

```js
Vue.component('component-a', { /* ... */ })
Vue.component('component-b', { /* ... */ })
Vue.component('component-c', { /* ... */ })

new Vue({ el: '#app' })
```

```html
<div id="app">
  <component-a></component-a>
  <component-b></component-b>
  <component-c></component-c>
</div>
```

Ini bahkan berlaku kepada semua subkomponen, berarti tiga komponen tersebut juga tersedia _didalam satu sama lain_.

## Registrasi Lokal

Registrasi global jarang menjadi ideal. Sebagai contoh, ketika anda menggunakan *build system* seperti Webpack, meregristasi global semua komponen berarti ketika anda berhenti menggunakan sebuah komponen, itu juga masih termasuk di *final build* anda. Ketidakperluan ini menambah banyak Javascript yang harus di unduh oleh pengguna anda.

Pada kasus ini, anda bisa menetapkan komponen anda sebagai objek Javascript sederhana:

```js
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```

Lalu tetapkan komponen komponen yang akan anda gunakan di pilihan `components`

```js
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

Untuk setiap properti didalam objek `components`, kuncinya akan menjadi nama dari elemen *custom*, ketika nilai akan berisi pilihan objek dari komponen tersebut.

Catat bahwa **komponen yang terdaftar secara lokal juga _tidak_ terdapat di dalam _subkomponen_**. Sebagai contoh, jika anda ingin `Component A` terdapat di dalam `Component B`, anda harus menggunakan:

```js
var ComponentA = { /* ... */ }

var ComponentB = {
  components: {
    'component-a': ComponentA
  },
  // ...
}
```

Atau jika anda tertarik menggunakan modul ES2015, seperti melewati Babel dan Webpack, itu akan lebih terlihat seperti:

```js
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  },
  // ...
}
```

Cata bahwa di ES2015+, menempatkan nama variabel seperti `ComponentA` didalam *shorthand* objek untuk`ComponentA: ComponentA`, berarti nama dari kedua variabel adalah:

- nama elemen *custom* yang digunakan di *template*, dan
- nama dari variabel berisi pilihan komponen

## Sistem Modul

Ketika anda tidak menggunakan sistem modul dengan `import`/`require`, anda mungkin bisa melewati bagian ini untuk sekarang. Jika anda menggunakanya, kita punya beberapa arahan spesial dan tips khusus untukmu.

### Registrasi Lokal didalam Sistem Modul

Jika anda masih disini, berarti sepertinya anda menggunakan sistem modul, seperti Babel dan Webpack. Pada kasus ini, kita merekomendasi membuat sebuah direktori `komponen`, dengan setiap komponen memiliki filenya sendiri.

Lalu anda harus memasukan setiap komponen yang akan anda gunakan, sebelum anda mendaftarkanya secara lokal. Sebagai contoh, di sebuah *hypothetical* `ComponentB.js` atau `ComponentB.vue` file:

```js
import ComponentA from './ComponentA'
import ComponentC from './ComponentC'

export default {
  components: {
    ComponentA,
    ComponentC
  },
  // ...
}
```

Sekarang kedua `ComponentA` Dan `ComponentC` dapat digunakan didalam *template* `ComponentB`.

### Registrasi Otomatis Global dari Komponen Dasar

Banyak dari komponen anda akan generik secara relatif, mungkin hanya membungkus elemen seperti sebuah tombol atau *input*. Kita terkadang mengacu pada ini sebagai [komponen dasar](../style-guide/#Base-component-names-strongly-recommended) dan mereka cenderung sering digunakan di beberapa komponen anda.

Hasilnya adalah banyak komponen mungkin memasukan daftar panjang dari komponen dasar:

```js
import BaseButton from './BaseButton.vue'
import BaseIcon from './BaseIcon.vue'
import BaseInput from './BaseInput.vue'

export default {
  components: {
    BaseButton,
    BaseIcon,
    BaseInput
  }
}
```

Hanya untuk membantu sedikit *markup* yang sama di sebuah *template*:

```html
<BaseInput
  v-model="searchText"
  @keydown.enter="search"
/>
<BaseButton @click="search">
  <BaseIcon name="search"/>
</BaseButton>
```

Untungnya, jika anda menggunakan Webpack (atau [Vue CLI 3+](https://github.com/vuejs/vue-cli), yang menggunakan Webpack didalamnya), anda bisa menggunakan `require.context` untuk mendaftarkan secara global komponen dasar yang paling umum. Berikut ini adalah contoh dari kode yang mungkin anda gunakan secara global untuk memasukan komponen dasar di awalan file aplikasi milik anda (contohnya `src/main.js`):

```js
import Vue from 'vue'
import upperFirst from 'lodash/upperFirst'
import camelCase from 'lodash/camelCase'

const requireComponent = require.context(
  // The relative path of the components folder
  './components',
  // Whether or not to look in subfolders
  false,
  // The regular expression used to match base component filenames
  /Base[A-Z]\w+\.(vue|js)$/
)

requireComponent.keys().forEach(fileName => {
  // Get component config
  const componentConfig = requireComponent(fileName)

  // Get PascalCase name of component
  const componentName = upperFirst(
    camelCase(
      // Gets the file name regardless of folder depth
      fileName
        .split('/')
        .pop()
        .replace(/\.\w+$/, '')
    )
  )


  // Register component globally
  Vue.component(
    componentName,
    // Look for the component options on `.default`, which will
    // exist if the component was exported with `export default`,
    // otherwise fall back to module's root.
    componentConfig.default || componentConfig
  )
})
```

Harap diingat bahsaw **pendaftaran global harus terjadi sebelum _root instance Vue_ dibuat (dengan `new Vue`)**. [ Berikut contohnya](https://github.com/chrisvfritz/vue-enterprise-boilerplate/blob/master/src/components/_globals.js) dari pola ini di dalam konteks proyek sungguhan.
