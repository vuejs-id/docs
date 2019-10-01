---
title: API
type: api
---

## Global Config

`Vue.config` adalah objek yang berisi konfigurasi global Vue. Anda dapat memodifikasi propertinya yang terdaftar di bawah ini sebelum mem-*bootstrap* aplikasi Anda:

### silent

- **Tipe:** `boolean`

- **Nilai Anggapan:** `false`

- **Penggunaan:**

  ``` js
  Vue.config.silent = true
  ```

  Memunculkan semua log dan peringatan Vue.

### optionMergeStrategies

- **Tipe:** `{ [key: string]: Function }`

- **Nilai Anggapan:** `{}`

- **Penggunaan:**

  ``` js
  Vue.config.optionMergeStrategies._my_option = function (parent, child, vm) {
    return child + 1
  }

  const Profile = Vue.extend({
    _my_option: 1
  })

  // Profile.options._my_option = 2
  ```

  Menetapkan strategi penggabungan kustom untuk opsi.

  Strategi penggabungan menerima nilai dari opsi yang didefinisikan pada *instance* induk dan anak sebagai argumen pertama dan kedua. Konteks dari Vue *instance* dioper sebagai argumen ketiga.

- **Lihat juga:** [Strategi Penggabungan pada Opsi Khusus](../guide/mixins.html#Strategi-Penggabungan-pada-Opsi-Khusus)

### devtools

- **Tipe:** `boolean`

- **Nilai Anggapan:** `true` (`false` pada build produksi)

- **Penggunaan:**

  ``` js
  // make sure to set this synchronously immediately after loading Vue
  Vue.config.devtools = true
  ```

  Konfigurasi untuk mengizinkan [vue-devtools](https://github.com/vuejs/vue-devtools) inspeksi. Nilai anggapan dari opsi ini adalah `true` dalam build pengembangan. Dan` false` di build produksi. Anda dapat mengaturnya ke `true` untuk mengaktifkan inspeksi untuk produksi.

### errorHandler

- **Tipe:** `Function`

- **Nilai Anggapan:** `undefined`

- **Penggunaan:**

  ``` js
  Vue.config.errorHandler = function (err, vm, info) {
    // penanganan galat
    // `info` adalah informasi galat yang spesifik pada Vue, seperti pada kait siklus hidup
    // galat ditemukan di. Hanya tersedia di 2.2.0+
  }
  ```

  Tetapkan handler untuk kesalahan yang tidak tertangkap selama fungsi render komponen dan pengamat. Handler dipanggil dengan galat dan Vue instance.

  > In 2.2.0+, this hook also captures errors in component lifecycle hooks. Also, when this hook is `undefined`, captured errors will be logged with `console.error` instead of crashing the app.

  > In 2.4.0+, this hook also captures errors thrown inside Vue custom event handlers.

  > In 2.6.0+, this hook also captures errors thrown inside `v-on` DOM listeners. In addition, if any of the covered hooks or handlers returns a Promise chain (e.g. async functions), the error from that Promise chain will also be handled.

  > Di 2.2.0+, kait ini juga menangkap kesalahan dalam kait siklus hidup komponen. Juga, ketika kait ini `undefined`, kesalahan yang ditangkap akan dicatat dengan` console.error` alih-alih merusak aplikasi.

  > Di 2.4.0+, hook ini juga menangkap kesalahan yang dilemparkan di dalam *Vue custom event handlers*.

  > Di 2.6.0+, kait ini juga menangkap kesalahan yang dilemparkan di dalam `v-on` DOM listener. Selain itu, jika ada kait atau handler yang mengembalikan rantai *Promise* (mis. Fungsi async), kesalahan dari rantai *Promise* itu juga akan ditangani.

  > Servis pelacak galat [Sentry](https://sentry.io/for/vue/) dan [Bugsnag](https://docs.bugsnag.com/platforms/browsers/vue/) menyediakan intergasi resmi dengan opsi ini.

### warnHandler

> Baru di 2.4.0+

- **Tipe:** `Function`

- **Nilai Anggapan:** `undefined`

- **Penggunaan:**

  ``` js
  Vue.config.warnHandler = function (msg, vm, trace) {
    // `trace` adalah jejak hierarki komponen
  }
  ```

  Menetapkan handler khusus untuk peringatan pada *runtime Vue*. Perhatikan ini hanya berfungsi selama mode pengembangan dan diabaikan dalam produksi.

### ignoredElements

- **Tipe:** `Array<string | RegExp>`

- **Nilai Anggapan:** `[]`

- **Penggunaan:**

  ``` js
  Vue.config.ignoredElements = [
    'my-custom-web-component',
    'another-web-component',
    // Gunakan `RegExp` untuk mengabaikan semua elemen yang dimulai dengan "ion-"
    // hanya 2.5+
    /^ion-/
  ]
  ```

  Membuat Vue mengabaikan elemen khusus yang didefinisikan di luar Vue (mis., Menggunakan Web Components APIs). Jika tidak, itu akan memunculkan peringatan tentang `Unknown custom element`, dengan asumsi Anda lupa mendaftarkan komponen global atau salah mengeja nama komponen.

### keyCodes

- **Tipe:** `{ [key: string]: number | Array<number> }`

- **Nilai Anggapan:** `{}`

- **Penggunaan:**

  ``` js
  Vue.config.keyCodes = {
    v: 86,
    f1: 112,
    // camelCase tidak akan bekerja
    mediaPlayPause: 179,
    // bandingkan menggunakan kebab-case dengan tanda kutip ganda
    "media-play-pause": 179,
    up: [38, 87]
  }
  ```

  ```html
  <input type="text" @keyup.media-play-pause="method">
  ```

  Menetapkan alias key kustom untuk `v-on`.

### performance

> Baru di 2.2.0+

- **Tipe:** `boolean`

- **Nilai Anggapan:** `false (from 2.2.3+)`

- **Penggunaan**:

  Setel ini menjadi `true` untuk mengaktifkan pelacakan inisialisasi komponen, kompilasi, render, dan palacakan performa patch pada browser devtool panel performance/timeline. Hanya berfungsi dalam mode pengembangan dan di browser yang mendukung [performance.mark] (https://developer.mozilla.org/en-US/docs/Web/API/Performance/mark) API.

### productionTip

> Baru di 2.2.0+

- **Tipe:** `boolean`

- **Nilai Anggapan:** `true`

- **Penggunaan**:

  Setel ini menjadi `false` untuk mencegah tip produksi pada memulai Vue.

## Global API

### Vue.extend( options )

- **Argumen:**
  - `{Object} options`

- **Penggunaan:**

  Membuat sebuah "subclass" dari konstruktor. Argumen harus berisi objek yang berisi opsi komponen.

  Kasus spesial untuk dicatat disini adalah opsi `data` - ini harus berupa fungsi ketika digunakan dengan `Vue.extend()`.

  ``` html
  <div id="mount-point"></div>
  ```

  ``` js
  // buat konstruktor
  var Profile = Vue.extend({
    template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
    data: function () {
      return {
        firstName: 'Walter',
        lastName: 'White',
        alias: 'Heisenberg'
      }
    }
  })
  // buat sebuah instance dari profil dan pasang ke dalam elemen
  new Profile().$mount('#mount-point')
  ```

  Akan menghasilkan:

  ``` html
  <p>Walter White aka Heisenberg</p>
  ```

- **Lihat juga:** [Components](../guide/components.html)

### Vue.nextTick( [callback, context] )

- **Argumen:**
  - `{Function} [callback]`
  - `{Object} [context]`

- **Penggunaan:**

  Menangguhkan panggilan balik untuk dieksekusi setelah siklus pembaruan DOM selanjutnya. Gunakan ini segera setelah Anda mengubah beberapa data untuk menunggu untuk pembaruan DOM.

  ``` js
  // modifikasi data
  vm.msg = 'Hello'
  // DOM belum diperbarui
  Vue.nextTick(function () {
    // DOM telah diperbarui
  })

  // gunakan sebagai promise (2.1.0+, lihat catatan di bawah)
  Vue.nextTick()
    .then(function () {
      // DOM telah diperbarui
    })
  ```

  > Baru di 2.1.0+: mengembalikan sebuah _Promise_ jika tidak tersedia panggilan balikdan _Promise_ didukung di lingkungan eksekusi. Tolong catat bahwa Vue tidak datang dengan _Promise_ _polyfill_, jadi jika Anda menargetkan peramban yang tidak mendukung _Promise_ secara bawaan (melihat Anda, IE). Anda harus menyediakan _polyfill_ Anda sendiri.

- **Lihat juga:** [Async Update Queue](../guide/reactivity.html#Async-Update-Queue)

### Vue.set( target, key, value )

- **Argumen:**
  - `{Object | Array} target`
  - `{string | number} key`
  - `{any} value`

- **Returns:** set nilai.

- **Penggunaan:**

  Menambahkan properti ke dalam objek reaktif, menjamin properti baru untuk reaktif juga, jadi memicu pembaruan _view_. Ini harus digunakan untuk menambah properti baru untuk objek reaktif, karena Vue tidak dapat mendeteksi penambahan properti normal (sebagai contoh `this.myObject.newProperty = 'hi'`).

  <p class="tip">Objek target tidak dapat menjadi _instance_ Vue, atau objek data root dari _instance_ Vue.</p>

- **Lihat juga:** [Reactivity in Depth](../guide/reactivity.html)

### Vue.delete( target, key )

- **Argumen:**
  - `{Object | Array} target`
  - `{string | number} key/index`

  > Hanya di 2.2.0+: Juga bekerja dengan Array + index.

- **Penggunaan:**

  Menghapus properti dalam objek. Jika objek reaktif, menjamin pembaruan pemicu penghapusan _view_. Ini terutama digunakan untuk mengatasi batasan dimana Vue tidak dapat mendeteksi penghapusan properti, tapi Anda jarang harus menggunakannya.

  <p class="tip">Objek target tidak dapat menjadi _instance_ Vue, atau objek data root dari _instance_ Vue.</p>

- **Lihat juga:** [Reactivity in Depth](../guide/reactivity.html)

### Vue.directive( id, [definition] )

- **Argumen:**
  - `{string} id`
  - `{Function | Object} [definition]`

- **Penggunaan:**

  Mendaftarkan atau mengambil directif global.

  ``` js
  // register
  Vue.directive('my-directive', {
    bind: function () {},
    inserted: function () {},
    update: function () {},
    componentUpdated: function () {},
    unbind: function () {}
  })

  // register (function directive)
  Vue.directive('my-directive', function () {
    // ini akan dipanggil sebagai `bind` dan `update`
  })

  // getter, mengembalikan definisi direktif jika terdaftar
  var myDirective = Vue.directive('my-directive')
  ```

- **Lihat juga:** [Custom Directives](../guide/custom-directive.html)

### Vue.filter( id, [definition] )

- **Argumen:**
  - `{string} id`
  - `{Function} [definition]`

- **Penggunaan:**

  Mendaftarkan atau mengambil filter global.

  ``` js
  // register
  Vue.filter('my-filter', function (value) {
    // return processed value
  })

  // getter, mengembalikan filter jika terdaftar
  var myFilter = Vue.filter('my-filter')
  ```

- **Lihat juga:** [Filters](../guide/filters.html)

### Vue.component( id, [definition] )

- **Argumen:**
  - `{string} id`
  - `{Function | Object} [definition]`

- **Penggunaan:**

  Mendaftarkan atau mengambil komponen global. Pendaftaran juga secara otomatis menset komponen `name` dengan `id` yang diberikan.

  ``` js
  // mendaftarkan sebuah perpanjangan konstruktor
  Vue.component('my-component', Vue.extend({ /* ... */ }))

  // mendaftarkan opsi objek (secara otomatis memanggil Vue.extend)
  Vue.component('my-component', { /* ... */ })

  // mengambil komponen yang telah terdaftar (selalu mengembalikan konstruktor)
  var MyComponent = Vue.component('my-component')
  ```

- **Lihat juga:** [Components](../guide/components.html)

### Vue.use( plugin )

- **Argumen:**
  - `{Object | Function} plugin`

- **Penggunaan:**

  Memasang plugin Vue.js. Jika plugin adalah sebuah objek, objek tersebut harus membuka metode `install`. Jika ini adalah fungsinya sendiri, ini akan diperlukan sebagai metode pemasangan. Metode pemasangan akan dipanggil dengan Vue sebagai argumen.

  Metode ini harus dipanggil sebelum memanggil `new Vue()`

  Ketika metode ini dipanggil dalam plugin yang sama beberapa kali, plugin hanya akan dipasang sekali.

- **Lihat juga:** [Plugins](../guide/plugins.html)

### Vue.mixin( mixin )

- **Argumen:**
  - `{Object} mixin`

- **Penggunaan:**

  Menerapkan _mixin_ secara global, akan berpengaruh ke setiap _instance_ Vue yang tercipta setelah itu. Ini dapat digunakan oleh pembuat plugin untuk menginjeksi perilaku khusus ke dalam komponen. **Tidak direkomendasikan dalam kode aplikasi**.

- **Lihat juga:** [Global Mixin](../guide/mixins.html#Global-Mixin)

### Vue.compile( template )

- **Argumen:**
  - `{string} template`

- **Penggunaan:**

  Menghimpun untai templat kedalam fungsi render. **Hanya tersedia dalam build penuh.**

  ``` js
  var res = Vue.compile('<div><span>{{ msg }}</span></div>')

  new Vue({
    data: {
      msg: 'hello'
    },
    render: res.render,
    staticRenderFns: res.staticRenderFns
  })
  ```

- **Lihat juga:** [Render Functions](../guide/render-function.html)

### Vue.observable( object )

> Baru di 2.6.0+

- **Argumen:**
  - `{Object} object`

- **Penggunaan:**

  Membuat sebuah reaktif objek. Secara internal, Vue menggunakan ini dalam objek yang dikembalikan oleh fungsi `data`.

  Objek yang dikembalikan dapat digunakan secara langsung dalam [fungsi render](../guide/render-function.html) dan [properti computed](../guide/computed.html), dan akan memicu pembaruan yang sesuai ketika bermutasi. Ini juga dapat digunakan secara minimal, menyimpan _state_ lintas komponen untuk skenario sederhana:

  ``` js
  const state = Vue.observable({ count: 0 })

  const Demo = {
    render(h) {
      return h('button', {
        on: { click: () => { state.count++ }}
      }, `count is: ${state.count}`)
    }
  }
  ```

  <p class="tip">Dalam Vue 2.x, `Vue.observable` secara langsung memutasikan objek yang dioper ke dalamnya, jadi ini sama dengan pengembalian objek, [didemonstrasikan disini](../guide/instance.html#Data-and-Methods). Dalam Vue 3.x, proksi reaktif akan kembali sebagai gantinya, meninggalkan objek nonreaktif aslinya jika bermutasi secara langsung. Karena itu, untuk kompatibilitas di masa yang akan datang, kami selalu merekomendasikan bekerja dengan objek yang dikembalikan oleh `Vue.observable`, dari pada objek asli yang dioper ke dalamnya.</p>

- **Lihat juga:** [Reactivity in Depth](../guide/reactivity.html)

### Vue.version

- **Detil**: Menyediakan versi yang terpasang dari Vue sebagai untai. Terutama beguna untuk plugin dan komponen dari komunitas, dimana Anda mungkin menggunakan stretegi berbeda untuk versi yang berbeda.

- **Penggunaan**:

  ```js
  var version = Number(Vue.version.split('.')[0])

  if (version === 2) {
    // Vue v2.x.x
  } else if (version === 1) {
    // Vue v1.x.x
  } else {
    // Unsupported versions of Vue
  }
  ```

## Options / Data

### data

- **Tipe:** `Object | Function`

- **Batasan:** Only accepts `Function` when used in a component definition.

- **Detail:**

  The data object for the Vue instance. Vue will recursively convert its properties into getter/setters to make it "reactive". **The object must be plain**: native objects such as browser API objects and prototype properties are ignored. A rule of thumb is that data should just be data - it is not recommended to observe objects with their own stateful behavior.

  Once observed, you can no longer add reactive properties to the root data object. It is therefore recommended to declare all root-level reactive properties upfront, before creating the instance.

  After the instance is created, the original data object can be accessed as `vm.$data`. The Vue instance also proxies all the properties found on the data object, so `vm.a` will be equivalent to `vm.$data.a`.

  Properties that start with `_` or `$` will **not** be proxied on the Vue instance because they may conflict with Vue's internal properties and API methods. You will have to access them as `vm.$data._property`.

  When defining a **component**, `data` must be declared as a function that returns the initial data object, because there will be many instances created using the same definition. If we use a plain object for `data`, that same object will be **shared by reference** across all instances created! By providing a `data` function, every time a new instance is created we can call it to return a fresh copy of the initial data.

  If required, a deep clone of the original object can be obtained by passing `vm.$data` through `JSON.parse(JSON.stringify(...))`.

- **Example:**

  ``` js
  var data = { a: 1 }

  // direct instance creation
  var vm = new Vue({
    data: data
  })
  vm.a // => 1
  vm.$data === data // => true

  // must use function when in Vue.extend()
  var Component = Vue.extend({
    data: function () {
      return { a: 1 }
    }
  })
  ```

  Note that if you use an arrow function with the `data` property, `this` won't be the component's instance, but you can still access the instance as the function's first argument:

  ```js
  data: vm => ({ a: vm.myProp })
  ```

- **Lihat juga:** [Reactivity in Depth](../guide/reactivity.html)

### props

- **Tipe:** `Array<string> | Object`

- **Detail:**

  A list/hash of attributes that are exposed to accept data from the parent component. It has an Array-based simple syntax and an alternative Object-based syntax that allows advanced configurations such as type checking, custom validation and default values.

  With Object-based syntax, you can use following options:
    - **type:** can be one of the following native constructors: `String`, `Number`, `Boolean`, `Array`, `Object`, `Date`, `Function`, `Symbol`, any custom constructor function or an array of those. Will check if a prop has a given type, and will throw a warning if it doesn't. [More information](../guide/components-props.html#Prop-Types) on prop types.
    - **default:** `any`
    Specifies a default value for the prop. If the prop is not passed, this value will be used instead. Object or array defaults must be returned from a factory function.
    - **required:** `Boolean`
    Defines if the prop is required. In a non-production environment, a console warning will be thrown if this value is truthy and the prop is not passed.
    - **validator:** `Function`
    Custom validator function that takes the prop value as the sole argument. In a non-production environment, a console warning will be thrown if this function returns a falsy value (i.e. the validation fails). You can read more about prop validation [here](../guide/components-props.html#Prop-Validation).

- **Example:**

  ``` js
  // simple syntax
  Vue.component('props-demo-simple', {
    props: ['size', 'myMessage']
  })

  // object syntax with validation
  Vue.component('props-demo-advanced', {
    props: {
      // type check
      height: Number,
      // type check plus other validations
      age: {
        type: Number,
        default: 0,
        required: true,
        validator: function (value) {
          return value >= 0
        }
      }
    }
  })
  ```

- **Lihat juga:** [Props](../guide/components-props.html)

### propsData

- **Tipe:** `{ [key: string]: any }`

- **Batasan:** only respected in instance creation via `new`.

- **Detail:**

  Pass props to an instance during its creation. This is primarily intended to make unit testing easier.

- **Example:**

  ``` js
  var Comp = Vue.extend({
    props: ['msg'],
    template: '<div>{{ msg }}</div>'
  })

  var vm = new Comp({
    propsData: {
      msg: 'hello'
    }
  })
  ```

### computed

- **Tipe:** `{ [key: string]: Function | { get: Function, set: Function } }`

- **Detail:**

  Computed properties to be mixed into the Vue instance. All getters and setters have their `this` context automatically bound to the Vue instance.

  Note that if you use an arrow function with a computed property, `this` won't be the component's instance, but you can still access the instance as the function's first argument:

  ```js
  computed: {
    aDouble: vm => vm.a * 2
  }
  ```

  Computed properties are cached, and only re-computed on reactive dependency changes. Note that if a certain dependency is out of the instance's scope (i.e. not reactive), the computed property will __not__ be updated.

- **Example:**

  ```js
  var vm = new Vue({
    data: { a: 1 },
    computed: {
      // get only
      aDouble: function () {
        return this.a * 2
      },
      // both get and set
      aPlus: {
        get: function () {
          return this.a + 1
        },
        set: function (v) {
          this.a = v - 1
        }
      }
    }
  })
  vm.aPlus   // => 2
  vm.aPlus = 3
  vm.a       // => 2
  vm.aDouble // => 4
  ```

- **Lihat juga:** [Computed Properties](../guide/computed.html)

### methods

- **Tipe:** `{ [key: string]: Function }`

- **Detail:**

  Methods to be mixed into the Vue instance. You can access these methods directly on the VM instance, or use them in directive expressions. All methods will have their `this` context automatically bound to the Vue instance.

  <p class="tip">Note that __you should not use an arrow function to define a method__ (e.g. `plus: () => this.a++`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.a` will be undefined.</p>

- **Example:**

  ```js
  var vm = new Vue({
    data: { a: 1 },
    methods: {
      plus: function () {
        this.a++
      }
    }
  })
  vm.plus()
  vm.a // 2
  ```

- **Lihat juga:** [Event Handling](../guide/events.html)

### watch

- **Tipe:** `{ [key: string]: string | Function | Object | Array}`

- **Detail:**

  An object where keys are expressions to watch and values are the corresponding callbacks. The value can also be a string of a method name, or an Object that contains additional options. The Vue instance will call `$watch()` for each entry in the object at instantiation.

- **Example:**

  ``` js
  var vm = new Vue({
    data: {
      a: 1,
      b: 2,
      c: 3,
      d: 4,
      e: {
        f: {
          g: 5
        }
      }
    },
    watch: {
      a: function (val, oldVal) {
        console.log('new: %s, old: %s', val, oldVal)
      },
      // string method name
      b: 'someMethod',
      // deep watcher
      c: {
        handler: function (val, oldVal) { /* ... */ },
        deep: true
      },
      // the callback will be called immediately after the start of the observation
      d: {
        handler: function (val, oldVal) { /* ... */ },
        immediate: true
      },
      e: [
        'handle1',
        function handle2 (val, oldVal) { /* ... */ },
        {
          handler: function handle3 (val, oldVal) { /* ... */ },
          /* ... */
        }
      ],
      // watch vm.e.f's value: {g: 5}
      'e.f': function (val, oldVal) { /* ... */ }
    }
  })
  vm.a = 2 // => new: 2, old: 1
  ```

  <p class="tip">Note that __you should not use an arrow function to define a watcher__ (e.g. `searchQuery: newValue => this.updateAutocomplete(newValue)`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.updateAutocomplete` will be undefined.</p>

- **Lihat juga:** [Instance Methods / Data - vm.$watch](#vm-watch)

## Opsi / DOM

### el

- **Tipe:** `string | Element`

- **Batasan:** hanya berlaku pada pembuatan _instance_ melalui `new`.

- **Detail:**

  Berikan _instance_ Vue sebuah elemen DOM yang sudah ada untuk dipasangkan. Ini bisa berupa kata pemilih CSS atau sebuah HTMLElement sesungguhnya.

  Setelah _instance_ sudah dipasangkan, elemen akan dapat diakses sebagai `vm.$el`.

  Jika opsi ini ada saat pembuatan _instance_, _instance_ akan langsung menjalankan kompilasi; jika tidak, pengguna harus memanggil `vm.$mount()` secara eksplisit untuk menjalankan kompilasi secara manual.

  <p class="tip">Elemen yang diberikan hanya menjadi titik pemasangan. Tidak seperti Vue 1.x, elemen yang dipasangkan akan digantikan dengan DOM yang dihasilkan oleh Vue dalam semua kasus. Karena itu tidak disarankan untuk memasangkan instance akar ke `<html>` atau `<body>`.</p>

  <p class="tip">Jika  fungsi `render` maupun opsi `template` tidak diberikan, DOM HTML yang di dalam dari elemen DOM yang dipasangkan akan diekstrak sebagai template. Dalam kasus ini, Runtime + Compiler build Vue harus digunakan.</p>

- **Lihat juga:**
  - [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
  - [Runtime + Compiler vs. Runtime-only](../guide/installation.html#Runtime-Compiler-vs-Runtime-only)

### template

- **Tipe:** `string`

- **Detail:**

  Templat string yang akan digunakan sebagai markup untuk _instance_ Vue. Template akan **menggantikan** elemen yang dipasang. Markup apa pun yang ada di dalam elemen yang dipasang akan diabaikan, kecuali jika slot distribusi konten ada dalam template.

  Jika kata dimulai dengan `#` itu akan digunakan sebagai querySelector dan menggunakan innerHTML elemen yang dipilih sebagai string template. Ini memungkinkan penggunaan trik umum `<script type =" x-template ">` untuk menyertakan templat.

  <p class="tip">Dari perspektif keamanan, Anda hanya boleh menggunakan templat Vue yang dapat Anda percayai. Jangan pernah menggunakan konten yang dibuat pengguna sebagai templat Anda.</p>

  <p class="tip">Jika fungsi _render_ ada dalam opsi Vue, _template_ akan diabaikan.</p>

- **Lihat juga:**
  - [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
  - [Content Distribution with Slots](../guide/components.html#Content-Distribution-with-Slots)

### render

  - **Tipe:** `(createElement: () => VNode) => VNode`

  - **Detail:**

    Alternatif untuk templat string yang memungkinkan Anda memanfaatkan kekuatan penuh program JavaScript. Fungsi render menerima sebuah metode `createElement` sebagai argumen pertama yang digunakan untuk membuat `VNode`.

    Jika komponen merupakan sebuah komponen fungsional, fungsi render juga menerima ekstra argumen `context`, yang memberikan aksesk pada data kontekstual karena komponen fungsional tidak memiliki _instance_.

    <p class="tip">Fungsi `render` memiliki prioritas di atas fungsi render yang dikompilasi dari opsi `template` atau templat HTML dalam DOM dari elemen pemasangan yang ditentukan oleh opsi` el`..</p>

  - **Lihat juga:** [Render Functions](../guide/render-function.html)

### renderError

> Baru di 2.2.0+

  - **Tipe:** `(createElement: () => VNode, error: Error) => VNode`

  - **Detail:**

    **Hanya bekerja pada mode pengembangan.**

    Memberikan sebuah keluaran _render_ alternatif ketika fungsi `render` standar menemukan kesalahan. Kesalahan akan diteruskan ke `renderError` sebagai argumen kedua. Ini sangat berguna saat digunakan bersama dengan _hot-reload_.

  - **Contoh:**

    ``` js
    new Vue({
      render (h) {
        throw new Error('oops')
      },
      renderError (h, err) {
        return h('pre', { style: { color: 'red' }}, err.stack)
      }
    }).$mount('#app')
    ```

  - **Lihat juga:** [Render Functions](../guide/render-function.html)

## Opsi / Kait Siklus hidup

<p class="tip">Semua kait siklus hidup secara otomatis terikat konteks `this` dengan *instance*, sehingga Anda bisa mengakses data, computed properties, dan methods. Ini berarti __Anda seharusnya tidak menggunakan *arrow function* saat mendefinisikan metode siklus hidup__ (e.g. `created: () => this.fetchTodos()`). Alasannya adalah *arrow function* mengikat konteks induk, jadi `this` tidak akan menjadi *instance* Vue seperti yang Anda harapkan dan `this.fetchTodos` akan undefined.</p>

### beforeCreate

- **Tipe:** `Function`

- **Detail:**

  Dipanggil secara sinkron segera setelah *instance* diinisialisasi, sebelum pengaturan data observasi dan *event/watcher*.

- **Lihat juga:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### created

- **Tipe:** `Function`

- **Detail:**

  Dipanggil secara sinkron setelah *instance* dibuat. Pada tahap ini, *instance* telah selesai memproses opsi yang berarti yang berikut telah diatur: data observasi, *computed properties*, *methods*, *watch/event callbacks*. Namun, tahap pemasangan belum dimulai, dan properti `$el` belum tersedia.

- **Lihat juga:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### beforeMount

- **Tipe:** `Function`

- **Detail:**

  Dipanggil tepat sebelum pemasangan dimulai: fungsi `render` akan dipanggil untuk pertama kalinya

  **Kait ini tidak dipanggil selama *rendering* di sisi server.**

- **Lihat juga:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### mounted

- **Tipe:** `Function`

- **Detail:**

  Dipanggil setelah *instance* telah terpasang, dimana `el` digantikan oleh `vm.$el` yang baru dibuat. Jika root instance sudah terpasang ke sebuah elemen pada dokumen, `vm.$el` juga akan di dokumen ketika `mounted` dipanggil.

  Perhatikan bahwa `mounted` tidak menjamin bahwa semua komponen turunannya telah terpasang. Jika Anda ingin menunggu hingga seluruh tampilan telah di-*rander*,  anda dapat menggunakan [vm.$nextTick](#vm-nextTick) di dalam `mounted`


  ``` js
  mounted: function () {
    this.$nextTick(function () {
      // Kode yang hanya akan berjalan setelah
      // seluruh tampilan telah di-render
    })
  }
  ```

  **Kait ini tidak dipanggil selama *rendering* di sisi server.**

- **Lihat juga:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### beforeUpdate

- **Tipe:** `Function`

- **Detail:**

  Dipanggil ketika data berubah sebelum DOM di-*patched*. Ini adalah tempat yang baik untuk mengakses DOM yang ada sebelum pembaruan, misalnya untuk menghapus *event listeners* yang ditambahkan secara manual.

  **Kait ini tidak dipanggil selama *rendering* di sisi server, karena hanya render awal yang dilakukan di sisi server.**

- **Lihat juga:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### updated

- **Tipe:** `Function`

- **Detail:**

  Dipanggil setelah perubahan data menyebabkan *virtual DOM*  diubah dan *patched*.

  *DOM* komponen akan diperbarui ketika kait ini dipanggil, sehingga Anda dapat melakukan operasi yang bergantung pada *DOM* di sini. Namun, dalam sebagian besar kasus, Anda harus menghindari perubahan *state* di dalam kait. Untuk bereaksi terhadap perubahan *state*, biasanya lebih baik menggunakan [computed property](#computed) atau [watcher](#watch).

  Perhatikan bahwa `updated` tidak menjamin bahawa semua komponen turunannya telah terpasang. Jika Anda ingin menunggu hingga seluruh tampilan telah di-*render*,  anda dapat menggunakan [vm.$nextTick](#vm-nextTick) di dalam `mounted`

  ``` js
  updated: function () {
    this.$nextTick(function () {
      // Kode yang hanya akan berjalan setelah
      // seluruh tampilan telah di-render
    })
  }
  ```

  **Kait ini tidak dipanggil selama *rendering* di sisi server.**

- **Lihat juga:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### activated

- **Tipe:** `Function`

- **Detail:**

  Dipanggil ketika komponen *kept-alive* diaktifkan.

  **Kait ini tidak dipanggil selama *rendering* di sisi server.**

- **Lihat juga:**
  - [Built-in Components - keep-alive](#keep-alive)
  - [Dynamic Components - keep-alive](../guide/components.html#keep-alive)

### deactivated

- **Tipe:** `Function`

- **Detail:**

  Dipanggil ketika komponen *kept-alive* dinonaktifkan.

  **Kait ini tidak dipanggil selama *rendering* di sisi server.**

- **Lihat juga:**
  - [Built-in Components - keep-alive](#keep-alive)
  - [Dynamic Components - keep-alive](../guide/components.html#keep-alive)

### beforeDestroy

- **Tipe:** `Function`

- **Detail:**

  Dipanggil tepat sebelum *instance* Vue dihancurkan. Pada tahap ini *instance* masih berfungsi penuh.

  **Kait ini tidak dipanggil selama *rendering* di sisi server.**

- **Lihat juga:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### destroyed

- **Tipe:** `Function`

- **Detail:**

  Dipanggil setelah *instance* Vue dihancurkan. Ketika kait ini dipanggil, semua *directives* dari *instance* Vue *unbound*, semua *event listeners* telah dihapus, dan semua turunan *instance* Vue juga telah dihancurkan.

  **Kait ini tidak dipanggil selama *rendering* di sisi server.**

- **Lihat juga:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### errorCaptured

> Baru di 2.5.0+

- **Tipe:** `(err: Error, vm: Component, info: string) => ?boolean`

- **Detail:**

  Dipanggil ketika galat dari komponen turunan ditangkap. Kait menerima tiga argumen: Galatnya, *instance* komponen yang memicu galat, dan *string* yang memuat informasi dimana galat itu tertangkap. Kait dapat mengembalikan `false` untuk menghentikan galat agar tidak menyebar lebih lanjut.

  <p class="tip">Anda dapat mengubah state komponen dalam kait ini. Namun, penting untuk memiliki persyaratan dalam templat Anda atau fungsi *render* yang memiliki sirkuit pendek konten lain ketika galat telah ditangkap; jika tidak, komponen akan dilemparkan ke *loop render* yang tak terbatas.</p>

  **Aturan Propagasi Galat**

  - Secara *default*, semua kesalahan masih dikirim ke `config.errorHandler` global jika sudah didefinisikan, sehingga galat ini masih dapat dilaporkan ke layanan analitik di satu tempat.

  - Jika ada beberapa kait `errorCaptured` pada rantai pewarisan atau rantai induk komponen, semuanya akan dipanggil pada kesalahan yang sama.

  - Jika kait `errorCaptured` itu sendiri melempar kesalahan, kesalahan ini dan kesalahan yang ditangkap asli dikirim ke `config.errorHandler` global.

  - Kait `errorCaptured` dapat mengembalikan `false` untuk mencegah kesalahan agar tidak menyebar lebih lanjut. Ini pada dasarnya mengatakan "kesalahan ini telah ditangani dan harus diabaikan." Ini akan mencegah kait `errorCaptured` tambahan atau `global config.errorHandler` global agar tidak dipanggil untuk kesalahan ini.

## Opsi / Assets

### directives

- **Tipe:** `Object`

- **Detail:**

  Sejumlah directive yang harus disediakan untuk _instance_ Vue.

- **Lihat juga:** [Custom Directives](../guide/custom-directive.html)

### filters

- **Tipe:** `Object`

- **Detail:**

  Sejumlah filter yang harus disediakan untuk _instance_ Vue.

- **Lihat juga:** [`Vue.filter`](#Vue-filter)

### components

- **Tipe:** `Object`

- **Detail:**

  Sejumlah komponen yang harus disediakan untuk _instance_ Vue.

- **Lihat juga:** [Components](../guide/components.html)

## Options / Composition

### parent

- **Tipe:** `Vue instance`

- **Detail:**

  Specify the parent instance for the instance to be created. Establishes a parent-child relationship between the two. The parent will be accessible as `this.$parent` for the child, and the child will be pushed into the parent's `$children` array.

  <p class="tip">Use `$parent` and `$children` sparingly - they mostly serve as an escape-hatch. Prefer using props and events for parent-child communication.</p>

### mixins

- **Tipe:** `Array<Object>`

- **Detail:**

  The `mixins` option accepts an array of mixin objects. These mixin objects can contain instance options like normal instance objects, and they will be merged against the eventual options using the same option merging logic in `Vue.extend()`. e.g. If your mixin contains a created hook and the component itself also has one, both functions will be called.

  Mixin hooks are called in the order they are provided, and called before the component's own hooks.

- **Example:**

  ``` js
  var mixin = {
    created: function () { console.log(1) }
  }
  var vm = new Vue({
    created: function () { console.log(2) },
    mixins: [mixin]
  })
  // => 1
  // => 2
  ```

- **Lihat juga:** [Mixins](../guide/mixins.html)

### extends

- **Tipe:** `Object | Function`

- **Detail:**

  Allows declaratively extending another component (could be either a plain options object or a constructor) without having to use `Vue.extend`. This is primarily intended to make it easier to extend between single file components.

  This is similar to `mixins`.

- **Example:**

  ``` js
  var CompA = { ... }

  // extend CompA without having to call `Vue.extend` on either
  var CompB = {
    extends: CompA,
    ...
  }
  ```

### provide / inject

> New in 2.2.0+

- **Tipe:**
  - **provide:** `Object | () => Object`
  - **inject:** `Array<string> | { [key: string]: string | Symbol | Object }`

- **Detail:**

  <p class="tip">`provide` and `inject` are primarily provided for advanced plugin / component library use cases. It is NOT recommended to use them in generic application code.</p>

  This pair of options are used together to allow an ancestor component to serve as a dependency injector for all its descendants, regardless of how deep the component hierarchy is, as long as they are in the same parent chain. If you are familiar with React, this is very similar to React's context feature.

  The `provide` option should be an object or a function that returns an object. This object contains the properties that are available for injection into its descendants. You can use ES2015 Symbols as keys in this object, but only in environments that natively support `Symbol` and `Reflect.ownKeys`.

  The `inject` option should be either:
  - an array of strings, or
  - an object where the keys are the local binding name and the value is either:
    - the key (string or Symbol) to search for in available injections, or
    - an object where:
      - the `from` property is the key (string or Symbol) to search for in available injections, and
      - the `default` property is used as fallback value

  > Note: the `provide` and `inject` bindings are NOT reactive. This is intentional. However, if you pass down an observed object, properties on that object do remain reactive.

- **Example:**

  ``` js
  // parent component providing 'foo'
  var Provider = {
    provide: {
      foo: 'bar'
    },
    // ...
  }

  // child component injecting 'foo'
  var Child = {
    inject: ['foo'],
    created () {
      console.log(this.foo) // => "bar"
    }
    // ...
  }
  ```

  With ES2015 Symbols, function `provide` and object `inject`:
  ``` js
  const s = Symbol()

  const Provider = {
    provide () {
      return {
        [s]: 'foo'
      }
    }
  }

  const Child = {
    inject: { s },
    // ...
  }
  ```

  > The next 2 examples work with Vue 2.2.1+. Below that version, injected values were resolved after the `props` and the `data` initialization.

  Using an injected value as the default for a prop:
  ```js
  const Child = {
    inject: ['foo'],
    props: {
      bar: {
        default () {
          return this.foo
        }
      }
    }
  }
  ```

  Using an injected value as data entry:
  ```js
  const Child = {
    inject: ['foo'],
    data () {
      return {
        bar: this.foo
      }
    }
  }
  ```

  > In 2.5.0+ injections can be optional with default value:

  ``` js
  const Child = {
    inject: {
      foo: { default: 'foo' }
    }
  }
  ```

  If it needs to be injected from a property with a different name, use `from` to denote the source property:

  ``` js
  const Child = {
    inject: {
      foo: {
        from: 'bar',
        default: 'foo'
      }
    }
  }
  ```

  Similar to prop defaults, you need to use a factory function for non primitive values:

  ``` js
  const Child = {
    inject: {
      foo: {
        from: 'bar',
        default: () => [1, 2, 3]
      }
    }
  }
  ```

## Options / Misc

### name

- **Tipe:** `string`

- **Batasan:** only respected when used as a component option.

- **Detail:**

  Allow the component to recursively invoke itself in its template. Note that when a component is registered globally with `Vue.component()`, the global ID is automatically set as its name.

  Another benefit of specifying a `name` option is debugging. Named components result in more helpful warning messages. Also, when inspecting an app in the [vue-devtools](https://github.com/vuejs/vue-devtools), unnamed components will show up as `<AnonymousComponent>`, which isn't very informative. By providing the `name` option, you will get a much more informative component tree.

### delimiters

- **Tipe:** `Array<string>`

- **Nilai Anggapan:** `{% raw %}["{{", "}}"]{% endraw %}`

- **Batasan:** This option is only available in the full build, with in-browser compilation.

- **Detail:**

  Change the plain text interpolation delimiters.

- **Example:**

  ``` js
  new Vue({
    delimiters: ['${', '}']
  })

  // Delimiters changed to ES6 template string style
  ```

### functional

- **Tipe:** `boolean`

- **Detail:**

  Causes a component to be stateless (no `data`) and instanceless (no `this` context). They are only a `render` function that returns virtual nodes making them much cheaper to render.

- **Lihat juga:** [Functional Components](../guide/render-function.html#Functional-Components)

### model

> New in 2.2.0

- **Tipe:** `{ prop?: string, event?: string }`

- **Detail:**

  Allows a custom component to customize the prop and event used when it's used with `v-model`. By default, `v-model` on a component uses `value` as the prop and `input` as the event, but some input types such as checkboxes and radio buttons may want to use the `value` prop for a different purpose. Using the `model` option can avoid the conflict in such cases.

- **Example:**

  ``` js
  Vue.component('my-checkbox', {
    model: {
      prop: 'checked',
      event: 'change'
    },
    props: {
      // this allows using the `value` prop for a different purpose
      value: String,
      // use `checked` as the prop which take the place of `value`
      checked: {
        type: Number,
        default: 0
      }
    },
    // ...
  })
  ```

  ``` html
  <my-checkbox v-model="foo" value="some value"></my-checkbox>
  ```

  The above will be equivalent to:

  ``` html
  <my-checkbox
    :checked="foo"
    @change="val => { foo = val }"
    value="some value">
  </my-checkbox>
  ```

### inheritAttrs

> New in 2.4.0+

- **Tipe:** `boolean`

- **Nilai Anggapan:** `true`

- **Detail:**

  By default, parent scope attribute bindings that are not recognized as props will "fallthrough" and be applied to the root element of the child component as normal HTML attributes. When authoring a component that wraps a target element or another component, this may not always be the desired behavior. By setting `inheritAttrs` to `false`, this default behavior can be disabled. The attributes are available via the `$attrs` instance property (also new in 2.4) and can be explicitly bound to a non-root element using `v-bind`.

  Note: this option does **not** affect `class` and `style` bindings.

### comments

> New in 2.4.0+

- **Tipe:** `boolean`

- **Nilai Anggapan:** `false`

- **Batasan:** This option is only available in the full build, with in-browser compilation.

- **Detail:**

  When set to `true`, will preserve and render HTML comments found in templates. The default behavior is discarding them.

## Instance Properties

### vm.$data

- **Tipe:** `Object`

- **Detail:**

  Objek data yang dipantau oleh *Vue instance*. *Vue instance* menjembatani akses ke properti-properti yang ada di objek data *instance* tersebut.

- **Lihat juga:** [Options / Data - data](#data)

### vm.$props

> Baru di versi 2.2.0+

- **Tipe:** `Object`

- **Detail:**

  Sebuah objek yang merepresentasikan *props* terkini yang diterima sebuah komponen. *Vue instance* menjembatani akses ke properti-properti yang ada di objek *props* *instance* tersebut.

### vm.$el

- **Tipe:** `Element`

- **Hanya dapat dibaca**

- **Detail:**

  Elemen pangkal (root) DOM yang dikelola oleh *Vue instance*.

### vm.$options

- **Tipe:** `Object`

- **Hanya dapat dibaca**

- **Detail:**

  Opsi-opsi pembuatan (*instantiation*) yang digunakan untuk *Vue instance* tersebut. Properti ini berguna saat kamu ingin memasukkan properti khusus di dalam opsi:

  ``` js
  new Vue({
    customOption: 'foo',
    created: function () {
      console.log(this.$options.customOption) // => 'foo'
    }
  })
  ```

### vm.$parent

- **Tipe:** `Vue instance`

- **Hanya dapat dibaca**

- **Detail:**

  *Instance* induk (*parent*), jika *instance* tersebut mempunyainya.

### vm.$root

- **Tipe:** `Vue instance`

- **Hanya dapat dibaca**

- **Detail:**

  *Vue instance* pangkal (root) dari pohon komponen yang ada. Jika *instance* tersebut tidak mempunyai induk (*parent*), maka nilai dari properti ini adalah *instance* itu sendiri.

### vm.$children

- **Tipe:** `Array<Vue instance>`

- **Hanya dapat dibaca**

- **Detail:**

  Komponen-komponen anakan (child) yang langsung berada dibawah *instance* tersebut. **Sebagai catatan, tidak ada jaminan akan urutan `$children`, dan mereka tidak bersifat reaktif.** Jika kamu ingin mencoba menggunakan `$children` untuk mengikat data (binding), pertimbangkan untuk menggunakan *Array* dan `v-for` untuk membuat (*generate*) komponen anakan (*child*), dan gunakan *Array* tersebut sebagai sumber validitas.

### vm.$slots

- **Tipe:** `{ [name: string]: ?Array<VNode> }`

- **Hanya dapat dibaca**

- **Detail:**

  Digunakan untuk secara terprogram mengakses konten [yang didistribusi dengan slots](../guide/components.html#Content-Distribution-with-Slots). Tiap [slot yang memiliki nama (*named slot*)](../guide/components-slots.html#Slot-yang-Memiliki-Nama-Named-Slot) mempunyai properti terkait sendiri (misal: konten `slot="foo"` akan ditemukan pada `vm.$slots.foo`). Properti `default` berisi titik-titik (*nodes*) yang tidak masuk di dalam slot yang memiliki nama (*named slot*).

  Pengaksesan `vm.$slots` paling berguna saat menulis sebuah komponen dengan [fungsi *render*](.../guide/render-function.html).

- **Contoh:**

  ```html
  <blog-post>
    <h1 slot="header">
      Tentang Saya
    </h1>

    <p>Disini beberapa konten halaman, yang akan masuk di dalam vm.$slots.default, karena tidak masuk di slot yang memiliki nama (named slot).</p>

    <p slot="footer">
      Copyright 2016 Evan You
    </p>

    <p>Jika saya memiliki beberapa konten disini, itu juga akan masuk di dalam vm.$slots.default.</p>.
  </blog-post>
  ```

  ```js
  Vue.component('blog-post', {
    render: function (createElement) {
      var header = this.$slots.header
      var body   = this.$slots.default
      var footer = this.$slots.footer
      return createElement('div', [
        createElement('header', header),
        createElement('main', body),
        createElement('footer', footer)
      ])
    }
  })
  ```

- **Lihat juga:**
  - [Komponen `<slot>`](#slot-1)
  - [Distribusi Konten dengan Slots](../guide/components.html#Content-Distribution-with-Slots)
  - [Fungsi Render - Slots](../guide/render-function.html#Slots)

### vm.$scopedSlots

> Baru di versi 2.1.0+

- **Tipe:** `{ [name: string]: props => Array<VNode> | undefined }`

- **Hanya dapat dibaca**

- **Detail:**

  Digunakan untuk secara terprogram mengakses [scoped slots](../guide/components.html#Scoped-Slots). Tiap slot, termasuk `default`, objeknya memiliki fungsi terkait yang mengembalikan *VNodes*.

  Pengaksesan `vm.$scopedSlots` sangat berguna saat menulis komponen dengan [fungsi render](../guide/render-function.html).

  **Catatan:** sejak versi 2.6.0+, ada dua perubahan di properti ini yg perlu diperhatikan:

  1. Fungsi *scoped slot* saat ini menjamin akan mengembalikan *array* dari *VNodes*, kecuali jika nilai yang dikembalikan tidak valid, yang berarti fungsi tersebut akan mengembalikan `undefined`.

  2. Semua `$slots` saat ini juga akan terekspos di `$scopedSlots` sebagai fungsi. Jika kamu berurusan dengan fungsi render, saat ini direkomendasikan untuk selalu mengakses *slots* via `$scopedSlots`, entah saat ini mereka menggunakan *scope* atau tidak. Hal ini tidak hanya akan membuat *refactors* mendatang semakin mudah dalam menambahkan sebuah *scope*, tapi juga akan memudahkan migrasi nantinya ke Vue 3, yang dimana semua *slots* adalah fungsi-fungsi.

- **Lihat juga:**
  - [Komponen `<slot>`](#slot-1)
  - [Scoped Slots](../guide/components.html#Scoped-Slots)
  - [Fungsi Render - Slots](../guide/render-function.html#Slots)

### vm.$refs

- **Tipe:** `Object`

- **Hanya dapat dibaca**

- **Detail:**

  Sebuah objek dari elemen-elemen DOM dan *instances* komponen, didaftarkan dengan [attribut `ref`](#ref).

- **Lihat juga:**
  - [*Refs* pada Komponen Anakan (*Child*)](../guide/components.html#Child-Component-Refs)
  - [Special Attributes - ref](#ref)

### vm.$isServer

- **Tipe:** `boolean`

- **Hanya dapat dibaca**

- **Detail:**

  Menyatakan apakah *Vue instance* tersebut berjalan di server.

- **Lihat juga:** [Server-Side Rendering](../guide/ssr.html)

### vm.$attrs

> Baru di versi 2.4.0+

- **Tipe:** `{ [key: string]: string }`

- **Hanya dapat dibaca**

- **Detail:**

  Berisi ikatan (bindings) attribut yang berada di cakupan induk (*parent*) (kecuali untuk `class` dan `style`) yang tidak dianggap (dan diekstrak) sebagai *props*. Saat sebuah komponen tidak memiliki deklarasi *props*, properti ini pada dasarnya berisi semua ikatan (bindings) yang berada di cakupan induk (*parent*) (kecuali untuk `class` dan `style`), dan dapat diteruskan kebawah ke komponen *inner* via `v-bind="$attrs"` - berguna saat membuat komponen *higher-order* (HOC).

### vm.$listeners

> Baru di versi 2.4.0+

- **Tipe:** `{ [key: string]: Function | Array<Function> }`

- **Hanya dapat dibaca**

- **Detail:**

  Berisi pemantau (*listeners*) *events* `v-on` yang berada di cakupan induk (parent) (tanpa pengubah (*modifiers*) `.native`). Properti ini dapat diteruskan kebawah ke komponen *inner* via `v-on="$listeners"` - berguna saat membuat komponen-komponen penyelubung (wrapper) yang bersifat transparan.

## Metode Instance / Data 

### vm.$watch( expOrFn, callback, [options] )

- **Argumen:**
  - `{string | Function} expOrFn`
  - `{Function | Object} callback`
  - `{Object} [options]`
    - `{boolean} deep`
    - `{boolean} immediate`

- **Mengembalikan:** `{Function} unwatch`

- **Penggunaan:**

Mengawasi suatu ekspresi atau fungsi penghitung (*computed function*) pada *instance* Vue untuk perubahan. *Callback* dipanggil dengan nilai baru dan nilai lama. Ekspresi hanya menerima jalur *dot-delimited*. Untuk ekspresi yang lebih kompleks, gunakan fungsi.

<p class="tip">Catatan: ketika memutasi (alih-alih mengganti) suatu Objek atau Array, nilai lama akan sama dengan nilai baru karena mereka mereferensikan Objek/Array yang sama. Vue tidak menyimpan salinan dari nilai pra-mutasi.</p>

- **Contoh:**

  ``` js
  // keypath
  vm.$watch('a.b.c', function (newVal, oldVal) {
    // lakukan sesuatu
  })

  // function
  vm.$watch(
    function () {
      // setiap kali ekspresi `this.a + this.b` menghasilkan hasil yang berbeda,     
      // handler akan dipanggil. Seolah-olah kita sedang mengawasi properti 
      // penghitung (computed property) tanpa mendefinisikan properti penghitung itu sendiri
      return this.a + this.b
    },
    function (newVal, oldVal) {
      // lakukan sesuatu
    }
  )
  ```

  `vm.$watch` mengembalikan fungsi yang tidak diamati yang berhenti menembakkan *callback*:

  ``` js
  var unwatch = vm.$watch('a', cb)
  // nanti, runtuhkan pengamat
  unwatch()
  ```

- **Opsi: deep**

  Untuk mendeteksi perubahan nilai bersarang (*nested value*) di dalam Objek, anda harus mengoper `deep: true` dalam argumen opsi. Perhatikan bahwa anda tidak perlu mendengarkan (*listen*) untuk mutasi Array.

  ``` js
  vm.$watch('someObject', callback, {
    deep: true
  })
  vm.someObject.nestedValue = 123
  // callback dieksekusi
  ```

- **Opsi: immediate**

  Mengoper `immediate: true` dalam opsi akan segera memicu *callback* dengan nilai ekspresi saat ini:

  ``` js
  vm.$watch('a', callback, {
    immediate: true
  })
  // `callback` segera dieksekusi dengan nilai `a` saat ini
  ```

### vm.$set( target, key, value )

- **Argumen:**
  - `{Object | Array} target`
  - `{string | number} key`
  - `{any} value`

- **Mengembalikan:** set nilai.

- **Penggunaan:**

  Ini adalah **alias** dari global `Vue.set`.

- **Lihat juga:** [Vue.set](#Vue-set)

### vm.$delete( target, key )

- **Argumen:**
  - `{Object | Array} target`
  - `{string | number} key`

- **Penggunaan:**

  Ini adalah **alias** dari global `Vue.delete`.

- **Lihat juga:** [Vue.delete](#Vue-delete)

## Instance Methods / Events

### vm.$on( event, callback )

- **Arguments:**
  - `{string | Array<string>} event` (array only supported in 2.2.0+)
  - `{Function} callback`

- **Penggunaan:**

  Listen for a custom event on the current vm. Events can be triggered by `vm.$emit`. The callback will receive all the additional arguments passed into these event-triggering methods.

- **Example:**

  ``` js
  vm.$on('test', function (msg) {
    console.log(msg)
  })
  vm.$emit('test', 'hi')
  // => "hi"
  ```

### vm.$once( event, callback )

- **Arguments:**
  - `{string} event`
  - `{Function} callback`

- **Penggunaan:**

  Listen for a custom event, but only once. The listener will be removed once it triggers for the first time.

### vm.$off( [event, callback] )

- **Arguments:**
  - `{string | Array<string>} event` (array only supported in 2.2.2+)
  - `{Function} [callback]`

- **Penggunaan:**

  Remove custom event listener(s).

  - If no arguments are provided, remove all event listeners;

  - If only the event is provided, remove all listeners for that event;

  - If both event and callback are given, remove the listener for that specific callback only.

### vm.$emit( eventName, [...args] )

- **Arguments:**
  - `{string} eventName`
  - `[...args]`

  Trigger an event on the current instance. Any additional arguments will be passed into the listener's callback function.

- **Examples:**

  Using `$emit` with only an event name:

  ```js
  Vue.component('welcome-button', {
    template: `
      <button v-on:click="$emit('welcome')">
        Click me to be welcomed
      </button>
    `
  })
  ```
  ```html
  <div id="emit-example-simple">
    <welcome-button v-on:welcome="sayHi"></welcome-button>
  </div>
  ```
  ```js
  new Vue({
    el: '#emit-example-simple',
    methods: {
      sayHi: function () {
        alert('Hi!')
      }
    }
  })
  ```
  {% raw %}
  <div id="emit-example-simple" class="demo">
    <welcome-button v-on:welcome="sayHi"></welcome-button>
  </div>
  <script>
    Vue.component('welcome-button', {
      template: `
        <button v-on:click="$emit('welcome')">
          Click me to be welcomed
        </button>
      `
    })
    new Vue({
      el: '#emit-example-simple',
      methods: {
        sayHi: function () {
          alert('Hi!')
        }
      }
    })
  </script>
  {% endraw %}

  Using `$emit` with additional arguments:

  ```js
  Vue.component('magic-eight-ball', {
    data: function () {
      return {
        possibleAdvice: ['Yes', 'No', 'Maybe']
      }
    },
    methods: {
      giveAdvice: function () {
        var randomAdviceIndex = Math.floor(Math.random() * this.possibleAdvice.length)
        this.$emit('give-advice', this.possibleAdvice[randomAdviceIndex])
      }
    },
    template: `
      <button v-on:click="giveAdvice">
        Click me for advice
      </button>
    `
  })
  ```

  ```html
  <div id="emit-example-argument">
    <magic-eight-ball v-on:give-advice="showAdvice"></magic-eight-ball>
  </div>
  ```

  ```js
  new Vue({
    el: '#emit-example-argument',
    methods: {
      showAdvice: function (advice) {
        alert(advice)
      }
    }
  })
  ```

  {% raw %}
  <div id="emit-example-argument" class="demo">
    <magic-eight-ball v-on:give-advice="showAdvice"></magic-eight-ball>
  </div>
  <script>
    Vue.component('magic-eight-ball', {
      data: function () {
        return {
          possibleAdvice: ['Yes', 'No', 'Maybe']
        }
      },
      methods: {
        giveAdvice: function () {
          var randomAdviceIndex = Math.floor(Math.random() * this.possibleAdvice.length)
          this.$emit('give-advice', this.possibleAdvice[randomAdviceIndex])
        }
      },
      template: `
        <button v-on:click="giveAdvice">
          Click me for advice
        </button>
      `
    })
    new Vue({
      el: '#emit-example-argument',
      methods: {
        showAdvice: function (advice) {
          alert(advice)
        }
      }
    })
  </script>
  {% endraw %}

## Instance Methods / Siklus Hidup

### vm.$mount( [elementOrSelector] )

- **Argumen:**
  - `{Element | string} [elementOrSelector]`
  - `{boolean} [hydrating]`

- **Mengembalikan:** `vm` - instansiasi dari dirinya sendiri

- **Penggunaan:**

  Jika *instance* vue tidak mencantumkan opsi `el` saat instansiasi, maka *instance* tersebut akan masuk kedalam keadaan "tidak terpasang/*unmounted*", tanpa terasosiasi dengan elemen *DOM* manapun. `vm.$mount()` dapat digunakan untuk menjalankan proses pemasangan/*mount* secara manual dari sebuah *instance* vue yang tak terpasang sebelumnya.

  Jika argumen `elementOrSelector` tidak disediakan, maka templat akan di*render* sebagai dokumen mati atau *off-document element*, dan anda dapat menggunakan *native DOM API* untuk memasukannya elemen kedalam dokumen anda.

  *method* ini akan mengembalikan *instance*nya sendiri, oleh sebab itu anda dapat mengaitkannya ke method-method yang lain setelah pemanggilannya.

- **Contoh:**

  ``` js
  var MyComponent = Vue.extend({
    template: '<div>Hello!</div>'
  })

  // instansiasi dan pemasangan kedalam elemen #app (akan menggantikan #app)
  new MyComponent().$mount('#app')

  // kode diatas serupa dengan ini:
  new MyComponent({ el: '#app' })

  // atau, me*render* dokumen mati dan menambahkan setelahnya:
  var component = new MyComponent().$mount()
  document.getElementById('app').appendChild(component.$el)
  ```

- **Lihat juga:**
  - [Siklus Hidup Diagram](../guide/instance.html#Lifecycle-Diagram)
  - [Proses Render Pada Sisi Server](../guide/ssr.html)

### vm.$forceUpdate()

- **Penggunaan:**

  Memaksa *instance* Vue untuk melakukan proses *render* ulang. Catatan hal ini tidak akan berpengaruh kepada semua anak dari komponen, hanya berpengaruh pada *instance*nya sendiri dan anak komponen yang disertai sisipan konten *slot*.

### vm.$nextTick( [callback] )

- **Argumen:**
  - `{Function} [callback]`

- **Penggunaan:**

  Menunda eksekusi dari *callback* setelah siklus pembaharuan DOM berikutnya. Gunakan method ini sesegera mungkin setelah anda melakukan perubahan data untuk menunggu pembaharuan *DOM*. Ini sama halnya dengan global `Vue.nextTick`, kecuali dalam konteks `this` pada *callback* yang akan otomatis tertuju kepada *instance* vue pemanggil.

  > Baru di 2.1.0+: mengembalikan *Promise* jika tidak disediakan *callback* dan *Promise* ini telah mendukung eksekusi lingkungan/*environtment*. Mohon dicatat *promise* yang disediakan oleh *Vue* tidak menyertakan *polyfill*, oleh sebab itu jika target browser anda tidak mendukung *promise* (seperti IE), anda sendiri yang memiliki kewajiban untuk menyediakannya.

- **Contoh:**

  ``` js
  new Vue({
    // ...
    methods: {
      // ...
      example: function () {
        // mengubah data
        this.message = 'changed'
        // DOM belum diperbaharui
        this.$nextTick(function () {
          // DOM telah diperbaharui
          // `this` tertuju pada yang menginstansiasi
          this.doSomethingElse()
        })
      }
    }
  })
  ```

- **Lihat juga:**
  - [Vue.nextTick](#Vue-nextTick)
  - [Pembaharuan Antrian Async](../guide/reactivity.html#Async-Update-Queue)

### vm.$destroy()

- **Kegunaan:**

  Menuntaskan pemusnahan sebuah vm. Membersihkan koneksi-koneksi terhadap vm-vm aktif yang lain, melepaskan keterikatan terhadap semua direktifnya, dan menonaktifkan semua *event listener*nya.

  Memicu `beforeDestroy` dan `destroyed` *hooks*.

  <p class="tip">Dalam situasi normal anda diharapkan tidak menggunakan method ini. Dalam pengontrolan siklus hidup komponen anak pada *data-driven*, Lebih disarankan untuk menggunakan `v-if` and `v-for`.</p>

- **Lihat juga:** [Siklus Hidup Diagram](../guide/instance.html#Lifecycle-Diagram)

## Direktif

### v-text

- **Nilai Anggapan:** `string`

- **Rincian:**

  Mengubah `textContent` dari elemen. Jika Anda perlu mengubah bagian `textContent`, Anda harus menggunakan interpolasi `{% raw %}{{ Mustache }}{% endraw %}`.

- **Contoh:**

  ```html
  <span v-text="msg"></span>
  <!-- sama seperti -->
  <span>{{msg}}</span>
  ```

- **Lihat juga:** [Sintaksis Data Binding - Interpolasi](../guide/syntax.html#Text)

### v-html

- **Nilai Anggapan:** `string`

- **Rincian:**

  Mengubah `innerHTML` dari elemen. **Catat bahwa konten disisipkan sebagai HTML biasa - mereka tidak akan dikompilasi sebagai templat Vue**. Jika anda mencoba membuat templat menggunakan `v-html`, sebaiknya anda memikirkian ulang solusinya dengan menggunakan komponen.

  <p class="tip">Me-render sembarang HTML secara dinamis pada website anda bisa sangat berbahaya karena bisa dengan mudah mendapatkan [serangan XSS](https://en.wikipedia.org/wiki/Cross-site_scripting). Gunakan `v-html` hanya pada konten yang dapat dipercaya dan **jangan pernah** pada konten  yang disediakan pengguna.</p>

  <p class="tip">Pada [Komponen Berkas Tungal](../guide/single-file-components.html), `scoped` styles tidak akan berlaku pada konten di dalam `v-html`, karena HTML tersebut tidak diproses oleh kompiler templat Vue. Jika anda ingin menggunakan *scoped CSS* pada konten `v-html`, sebaiknya anda menggunakan [module CSS](https://vue-loader.vuejs.org/en/features/css-modules.html) atau tambahan, elemen `<style>` global dengan startegi *scoping* manual seperti BEM.</p>

- **Contoh:**

  ```html
  <div v-html="html"></div>
  ```

- **Lihat juga:** [Sintaksis Data Binding - Interpolasi](../guide/syntax.html#Raw-HTML)

### v-show

- **Nilai Anggapan:** `any`

- **Penggunaan:**

  Mengalihkan properti CSS `display` dari elemen berdasarkan pada syarat yang dianggap benar dari nilai sebuah ekspresi.

  Direktif ini memicu transisi ketika syaratnya berubah.

- **Lihat juga:** [Rendering Bersyarat - v-show](../guide/conditional.html#v-show)

### v-if

- **Nilai Anggapan:** `any`

- **Penggunaan:**

  Me-render elemen sesuai kondisi berdasarkan pada syarat yang dianggap benar dari nilai sebuah ekspresi. Elemen beserta direktif / komponen yang ada di dalamnya dihancurkan dan dibuat ulang saat peralihan. Jika elemen merupakan sebuah elemen `<template>`, Kontennya akan diekstrak sebagai blok bersyarat.

  Direktif ini memicu transisi ketika syaratnya berubah.

  <p class="tip">Saat digunakan bersamaan dengan v-if, v-for memiliki prioritas yang lebih tinggi dari v-if. Lihat <a href="../guide/list.html#v-for-with-v-if">petunjuk me-render daftar</a> untuk rinciannya.</p>

- **Lihat juga:** [Rendering Bersyarat - v-if](../guide/conditional.html)

### v-else

- **Tidak mengharapkan ekspresi**

- **Pembatasan:** elemen sejajar sebelumnya harus memiliki `v-if` atau `v-else-if`.

- **Penggunaan:**

  Menandai "blok else" untuk `v-if` atau rantai `v-if`/`v-else-if`.

  ```html
  <div v-if="Math.random() > 0.5">
    Now you see me
  </div>
  <div v-else>
    Now you don't
  </div>
  ```

- **Lihat juga:** [Rendering Bersyarat - v-else](../guide/conditional.html#v-else)

### v-else-if

> Baru pada 2.1.0+

- **Nilai Anggapan:** `any`

- **Pembatasan:** elemen sejajar sebelumnya harus memiliki `v-if` atau `v-else-if`.

- **Penggunaan:**

  Menandai "blok else if" untuk `v-if`. Dapat digunakan berantai.

  ```html
  <div v-if="type === 'A'">
    A
  </div>
  <div v-else-if="type === 'B'">
    B
  </div>
  <div v-else-if="type === 'C'">
    C
  </div>
  <div v-else>
    Not A/B/C
  </div>
  ```

- **Lihat juga:** [Rendering Bersyarat - v-else-if](../guide/conditional.html#v-else-if)

### v-for

- **Nilai Anggapan:** `Array | Object | number | string | Iterable (since 2.6)`

- **Penggunaan:**

  Me-render elemen atau blok templat berulang kali berdasarkan sumber data. Nilai direktif harus menggunakan sintaksis khusus `alias in expression` untuk menyediakan alias pada elemen yang sedang diiterasi:

  ``` html
  <div v-for="item in items">
    {{ item.text }}
  </div>
  ```

  Cara lainnya, anda juga dapat menentukan alias untuk *index* (atau *key* jika digunakan pada objek):

  ``` html
  <div v-for="(item, index) in items"></div>
  <div v-for="(val, key) in object"></div>
  <div v-for="(val, key, index) in object"></div>
  ```

  Prilaku standar `v-for` akan mencoba mengubah/menambal elemen pada tempatnya tanpa menggerakkan mereka. Agar bisa memaksanya untuk mengurutkan elemennya kembali, Anda perlu menyediakan petunjuk pengurutan dengan atribut khusus `key`:

  ``` html
  <div v-for="item in items" :key="item.id">
    {{ item.text }}
  </div>
  ```

  Pada 2.6+, `v-for` bisa juga bekerja pada nilai yang mengimplementasi [Iterable Protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterable_protocol), termasuk `Map` dan `Set` *native*. Akan tetapi, perlu dicatat bahwa Vue 2.x saat ini tidak mendukung reactivity pada nilai `Map` dan `Set`, jadi tidak bisa secara otomatis mendeteksi perubahan.

  <p class="tip">Saat digunakan bersama v-if, v-for memiliki prioritas yang lebih tinggi dari v-if. Lihat <a href="../guide/list.html#v-for-with-v-if">petunjuk me-render daftar</a> untuk rinciannya.</p>

  Rincian penggunaan `v-for` dijelaskan dalam bagian petunjuk pada tautan dibawah.

- **Lihat juga:**
  - [Me-render Daftar](../guide/list.html)
  - [key](../guide/list.html#key)

### v-on

- **Penulisan Singkat:** `@`

- **Nilai Anggapan:** `Function | Inline Statement | Object`

- **Argumen:** `event`

- **Modifier:**
  - `.stop` - memanggil `event.stopPropagation()`.
  - `.prevent` - memanggil `event.preventDefault()`.
  - `.capture` - menambahkan pendengar event (*event listener*) pada mode tangkap (*capture*).
  - `.self` - hanya memicu *handler* jika event dikirimkan dari elemen ini.
  - `.{keyCode | keyAlias}` - hanya memicu *handler* pada kunci tertentu.
  - `.native` - mendengarkan event native elemen *root* dari komponen.
  - `.once` - memicu *handler* paling banyak satu kali.
  - `.left` - (2.2.0+) hanya memicu *handler* untuk event tombol kiri mouse.
  - `.right` - (2.2.0+) hanya memicu *handler* untuk event tombol kanan mouse.
  - `.middle` - (2.2.0+) hanya memicu *handler* untuk event tombol tengah mouse.
  - `.passive` - (2.3.0+) meletakkan event DOM dengan `{ passive: true }`.

- **Penggunaan:**

  meletakkan sebuah pendengar event pada elemen. Tipe event ditandai dengan argumen. Ekspresi dapat berupa nama method, suatu pernyataan sebaris, atau  dihilangkan jika terdapat modifier.

  Saat digunakan pada elemen normal, Ia hanya akan mendengarkan [**even DOM native**](https://developer.mozilla.org/en-US/docs/Web/Events). Jika digunakan pada elemen komponen kustom, Ia akan mendengarkan **event kustom** yang dipancarkan pada komponen anak.

  Saat mendengarkan event DOM native, method menerima event native sebagai satu-satunya argumen. Jika menggunakan pernyataan sebaris, pernyataan tersebut memiliki akses pada properti khusus `$event`: `v-on:click="handle('ok', $event)"`.

  Mulai pada 2.4.0+, `v-on` juga mendukung *binding* pada pasangan objek event/listener tanpa argumen. Catatan ketika menggunakan sintaksis objek, tidak mendukung modifier apapun.

- **Contoh:**

  ```html
  <!-- handler method -->
  <button v-on:click="doThis"></button>

  <!-- event dinamis (2.6.0+) -->
  <button v-on:[event]="doThis"></button>

  <!-- pernyataan sebaris -->
  <button v-on:click="doThat('hello', $event)"></button>

  <!-- penulisan singkat -->
  <button @click="doThis"></button>

  <!-- penulisan singkat event dinamis (2.6.0+) -->
  <button @[event]="doThis"></button>

  <!-- stop perambatan -->
  <button @click.stop="doThis"></button>

  <!-- mencegah default -->
  <button @click.prevent="doThis"></button>

  <!-- mencegah default tanpa ekspresi -->
  <form @submit.prevent></form>

  <!-- modifier berantai -->
  <button @click.stop.prevent="doThis"></button>

  <!-- modifier key menggunakan keyAlias -->
  <input @keyup.enter="onEnter">

  <!-- modifier key menggunakan keyCode -->
  <input @keyup.13="onEnter">

  <!-- event klik akan dipicu paling banyak satu kali -->
  <button v-on:click.once="doThis"></button>

  <!-- sintaksis objek (2.4.0+) -->
  <button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
  ```

  Mendengarkan event kustom pada komponen anak (handler dipanggil ketika "my-event" dipancarkan pada komponen anak):

  ```html
  <my-component @my-event="handleThis"></my-component>

  <!-- pernyataan sebaris -->
  <my-component @my-event="handleThis(123, $event)"></my-component>

  <!-- event native pada komponen -->
  <my-component @click.native="onClick"></my-component>
  ```

- **Lihat juga:**
  - [Penanganan Event](../guide/events.html)
  - [Komponen - Event Kustom](../guide/components.html#Custom-Events)

### v-bind

- **Penulisan Singkat:** `:`

- **Nilai Anggapan:** `any (with argument) | Object (without argument)`

- **Argumen:** `attrOrProp (optional)`

- **Modifier:**
  - `.prop` - Mengaitkan sebagai sebuah properti DOM debanding sebuah atribut ([apa bedanya??](https://stackoverflow.com/questions/6003819/properties-and-attributes-in-html#answer-6004028)). Jika tag adalah sebuah komponen maka `.prop` akan mengatur properti pada `$el` komponen.
  - `.camel` - (2.1.0+) menguba nama atribut dengan penulisan kebab-case menjadi camelCase.
  - `.sync` - (2.3.0+) sintaksis pemanis yang memperbolehkan *handler* `v-on` untuk mengubah nilai yang terkait.

- **Penggunaan:**

  Secara dinamis mengaitkan satu atau lebih atribut, atau sebuah prop komponen pada sebuah ekspresi.

  Ketika digunakan untuk mengaitkan atribut `class` atau `style`, mendukung tipe atribut tambahan seperti Array atau Object. Lihat tautan bagian petunjuk di bawah untuk lebih banyak rincian.

  Ketika digunakan untuk mengaitkan prop, prop harus dideklarasikan pada komponen anak dengan benar.

  Ketika digunakan tanpa argumen, dapat digunakan untuk mengaitkan object yang berisi pasangan atribut nama-nilai. Catatan pada mode ini `class` dan `style` tidak mendukung Array atau Object.

- **Contoh:**

  ```html
  <!-- mengaitkan sebuah atribut -->
  <img v-bind:src="imageSrc">

  <!-- nama atribut dinamis (2.6.0+) -->
  <button v-bind:[key]="value"></button>

  <!-- cara penulisan singkat -->
  <img :src="imageSrc">

  <!-- cara penulisan singkat dengan nama atribut dinamis (2.6.0+) -->
  <button :[key]="value"></button>

  <!-- dengan penggabungan string sebaris -->
  <img :src="'/path/to/images/' + fileName">

  <!-- mengaitkan dengan class -->
  <div :class="{ red: isRed }"></div>
  <div :class="[classA, classB]"></div>
  <div :class="[classA, { classB: isB, classC: isC }]">

  <!-- mengaitkan dengan style -->
  <div :style="{ fontSize: size + 'px' }"></div>
  <div :style="[styleObjectA, styleObjectB]"></div>

  <!-- mengaitkan dengan atribut objek -->
  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

  <!-- mengaitkan atribut DOM dengan modifier prop -->
  <div v-bind:text-content.prop="text"></div>

  <!-- mengaitkan prop. "prop" harus dideklarasikan pada my-component. -->
  <my-component :prop="someThing"></my-component>

  <!-- mengirimkan props dari induk kepada anak yang memiliki props serupa -->
  <child-component v-bind="$props"></child-component>

  <!-- XLink -->
  <svg><a :xlink:special="foo"></a></svg>
  ```

  Modifier `.camel` akan melakukan konversi penulisan nama atribut `v-bind` menjadi gaya camelCase ketika digunakan pada DOM templates, mis. atribut `viewBox` pada SVG:

  ``` html
  <svg :view-box.camel="viewBox"></svg>
  ```

  `.camel` tidak diperlukan jika anda menggunakan templat string, atau melakukan kompilasi dengan `vue-loader`/`vueify`.

- **Lihat juga:**
  - [Kelas dan Binding Gaya](../guide/class-and-style.html)
  - [Komponen - Props](../guide/components.html#Props)
  - [Komponen - Modifier `.sync`](../guide/components.html#sync-Modifier)

### v-model

- **Nilai Anggapan:** beragam berdasarkan pada nilai elemen input form atau output dari komponen

- **Terbatas pada:**
  - `<input>`
  - `<select>`
  - `<textarea>`
  - components

- **Modifier:**
  - [`.lazy`](../guide/forms.html#lazy) - mendengarkan event `change` dibanding `input`
  - [`.number`](../guide/forms.html#number) - mengganti input string menjadi angka
  - [`.trim`](../guide/forms.html#trim) - trim input

- **Penggunaan:**

  Membuat kait dua arah pada elemen input form atau komponen. Untuk rincian penggunaan dan catatan lainnya, lihat bagian Petunjuk yang ditautkan di bawah.

- **Lihat juga:**
  - [Form Input Bindings](../guide/forms.html)
  - [Komponen - Form Input Komponen menggunakan Event Kustom](../guide/components.html#Form-Input-Components-using-Custom-Events)

### v-slot

- **Penulisan Singkat:** `#`

- **Nilai Anggapan:** Ekspersi JavaScript yang valid pada posisi argumen fungsi (mendukung destrukturisasi pada [lingkungan yang didukung](../guide/components-slots.html#Slot-Props-Destructuring)). Opsional - hanya dibutuhkan jika mengharapkan props dikirimkan ke dalam slot.

- **Argumen:** nama slot (opsional, default untuk `default`)

- **Terbatas pada:**
  - `<template>`
  - [komponen](../guide/components-slots.html#Abbreviated-Syntax-for-Lone-Default-Slots) (untuk default slot dengan props)

- **Penggunaan:**

  Menandai slot bernama atau slot yang mengharapkan untuk menerima prop.

- **Contoh:**

  ```html
  <!-- Slot bernama -->
  <base-layout>
    <template v-slot:header>
      Header content
    </template>

    Default slot content

    <template v-slot:footer>
      Footer content
    </template>
  </base-layout>

  <!-- Slot bernama yang menerima prop -->
  <infinite-scroll>
    <template v-slot:item="slotProps">
      <div class="item">
        {{ slotProps.item.text }}
      </div>
    </template>
  </infinite-scroll>

  <!-- Default slot yang menerima prop, dengan destrukturisasi -->
  <mouse-position v-slot="{ x, y }">
    Mouse position: {{ x }}, {{ y }}
  </mouse-position>
  ```

  Untuk lebih banyak rincian, lihat tautan di bawah.

- **Lihat juga:**
  - [Komponen - Slots](../guide/components-slots.html)
  - [RFC-0001](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0001-new-slot-syntax.md)

### v-pre

- **Tidak mengharapkan ekspresi**

- **Penggunaan:**

  Melewatkan kompilasi untuk elemen dan seluruh anaknya. Anda dapat menggunakan ini untuk menampilkan tag moustache mentah. Melewatkan banyak node yang tidak memiliki direktif juga bisa mempercepat kompilasi.

- **Contoh:**

  ```html
  <span v-pre>{{ ini tidak akan dikompilasi }}</span>
   ```

### v-cloak

- **Tidak mengharapkan ekspresi**

- **Penggunaan:**

  Direktif ini akan tetap pada elemen sampai Vue Instance selesai dikompilasi. Dikombinasikan dengan aturan CSS seperti `[v-cloak] { display: none }`, Direktif ini bisa digunakan untuk menyembunyikan kaitan mustache yang belum dikompilasi sampai Vue instance siap.

- **Contoh:**

  ```css
  [v-cloak] {
    display: none;
  }
  ```

  ```html
  <div v-cloak>
    {{ message }}
  </div>
  ```

  `<div>` tidak akan bisa dilihat sampai kompilasi selesai

### v-once

- **Tidak mengharapkan ekspresi**

- **Rincian:**

  Me-render elemen dan komponen hanya **sekali**. Pada render ulang selanjutnya, elemen/komponen dan semua anaknya akan diperlakukan seperti konten statis dan dilewatkan. Ini dapat digunakan untuk mengoptimasi performa update.

  ```html
  <!-- elemen tunggal -->
  <span v-once>This will never change: {{msg}}</span>
  <!-- elemen yang memiliki anak -->
  <div v-once>
    <h1>comment</h1>
    <p>{{msg}}</p>
  </div>
  <!-- komponen -->
  <my-component v-once :comment="msg"></my-component>
  <!-- direktif `v-for` -->
  <ul>
    <li v-for="i in list" v-once>{{i}}</li>
  </ul>
  ```

- **Lihat juga:**
  - [Sintaksis Data Binding - interpolasi](../guide/syntax.html#Text)
  - [Komponen - Komponen Statis Murah dengan `v-once`](../guide/components.html#Cheap-Static-Components-with-v-once)

## Atribut Spesial

### key

- **Nilai Anggapan:** `number | string`

  kegunaan Atribut spesial `key` adalah sebagai petunjuk bagi algoritma DOM virtual dari Vue untuk mengidentifikasi *VNodes* ketika membandingkan list yang baru terhadap list yang lama. Tanpa *keys*, Vue menggunakan algoritma yang meminimalisir pergerakan elemen dan mencoba untuk *patch/reuse* elemen di tempat dengan tipe yang sama sebanyak mungkin. Dengan *keys*, elemen-elemen akan disusun kembali berdasarkan susunan perubahan dari *keys*, dan elemen-elemen dengan *keys* yang sudah tidak ada lagi akan selalu di hapus.

  Anak dari induk yang sama harus mempunyai *key* yang unik. *Key* yang sama akan mengakibatkan galat pada saat render.

  Contoh penggunaan yang paling umum adalah menggabungkan *key* dengan `v-for`:

  ``` html
  <ul>
    <li v-for="item in items" :key="item.id">...</li>
  </ul>
  ```

  *Key* juga dapat digunakan untuk penggantian paksa terhadap sebuah elemen/komponen alih-alih menggunakannya kembali. Ini akan berguna ketika anda ingin:

  - Memicu (*trigger*) *lifecycle hooks* dari sebuah komponen dengan tepat
  - Memicu (*trigger*) transisi

  Sebagai contoh:

  ``` html
  <transition>
    <span :key="text">{{ text }}</span>
  </transition>
  ```

  Ketika `text` berubah, `<span>` akan selalu diganti alih-alih di *patch*, jadi sebuah transisi akan  terpicu.

### ref

- **Nilai Anggapan:** `string`

  `ref` digunakan untuk mendaftarkan referensi terhadap elemen atau komponen anak. Referensi akan didaftarkan dibawah obyek `$refs` dari komponen parent. Jika digunakan pada elemen DOM polos, referensi akan menjadi elemen itu; jika digunakan pada sebuah elemen anak, referensi akan menjadi *instance* komponen:

  ``` html
  <!-- vm.$refs.p akan menjadi node DOM -->
  <p ref="p">hello</p>

  <!-- vm.$refs.child akan menjadi *instance* komponen anak -->
  <child-component ref="child"></child-component>
  ```

  Ketika digunakan pada elemen/komponen dengan `v-for`, referensi yang terdaftar akan menjadi sebuah Array yang berisi *DOM nodes*  atau *instances* komponen.

  Catatan penting dalam pendaftaran *ref* adalah pemilihan waktu: karena *refs* sendiri terbuat atas hasil dari *render function*, anda tidak dapat mengakses pada saat *initial render* - *refs* belum terbuat/ada! `$refs` juga tidak reaktif, untuk itu anda tidak seharusnya mencoba untuk menggunakannya pada template atau *data-binding*.

- **Lihat juga:** [*Child Component Refs*](../guide/components.html#Child-Component-Refs)

### is

- **Nilai Anggapan:** `string | Object (Objek opsi komponen)`

  Digunakan untuk [Komponen-komponen dinamis](../guide/components.html#Dynamic-Components) dan juga sebagai solusi pada [keterbatasan dari *in-DOM templates*](../guide/components.html#DOM-Template-Parsing-Caveats).

  Sebagai contoh:

  ``` html
  <!-- komponen berubah ketika currentView berubah -->
  <component v-bind:is="currentView"></component>

  <!-- perlu karena `<my-row>` akan menjadi tidak valid di dalam -->
  <!-- sebuah elemen `<table>` dan sebagainya akan diangkat  -->
  <table>
    <tr is="my-row"></tr>
  </table>
  ```

  Untuk kegunaan lebih lanjut, ikuti tautan dalam deskripsi diatas.

- **Lihat juga:**
  - [Komponen Dinamis](../guide/components.html#Dynamic-Components)
  - [Peringatan *Parsing* pada *DOM template*](../guide/components.html#DOM-Template-Parsing-Caveats)

### slot <sup style="color:#c92222">*deprecated*</sup>

**Rujuk ke [v-slot](#v-slot) di 2.6.0+.**

- **Nilai Anggapan:** `string`

  Digunakan pada konten yang dimasukkan pada komponen anak untuk menunjukkan slot mana yang dinamai pada konten tersebut.

- **Lihat juga:** [Slot yang dinamai `slot`](../guide/components.html#Named-Slots-with-slot)

### slot-scope <sup style="color:#c92222">deprecated</sup>

**Rujuk ke [v-slot](#v-slot) di 2.6.0+.**

- **Nilai Anggapan:** `ekspresi fungsi argumen`

- **Kegunaan:**

  Digunakan untuk menunjukkan sebuah elemen atau komponen sebagai slot tertutup (*scoped slot*). Nilai atribut harus menjadi ekspresi Javascript yang valid yang bisa muncul pada *argument position* dari sebuah *function signature*. Ini berarti dalam *environments* yang di dukung anda juga bisa menggunakan ES2015 *destructuring* pada ekpresi Javascript. Sebagai pengganti [`scope`](#scope-replaced) pada 2.5.0+.

  Attribut ini tidak mendukung *dynamic binding*.

- **Lihat juga:** [Slot tertutup (*scoped slot*) dengan  `slot-scope`](../guide/components.html#Scoped-Slots-with-slot-scope)

### scope <sup style="color:#c92222">terhapus</sup>

**Digantikan dengan [slot-scope](#slot-scope) pada 2.5.0+. Rujuk ke [v-slot](#v-slot) in 2.6.0+.**

Digunakan untuk menunjukkan sebuah elemen `<template>` sebagai sebuah slot tertutup.

- **Kegunaan:**

  Sama seperti [`slot-scope`](#slot-scope) Selain itu `scope` hanya bisa digunakan pada elemen `<template>`.

## Built-In Components

### component

- **Props:**
  - `is` - string | ComponentDefinition | ComponentConstructor
  - `inline-template` - boolean

- **Penggunaan:**

  A "meta component" for rendering dynamic components. The actual component to render is determined by the `is` prop:

  ```html
  <!-- a dynamic component controlled by -->
  <!-- the `componentId` property on the vm -->
  <component :is="componentId"></component>

  <!-- can also render registered component or component passed as prop -->
  <component :is="$options.components.child"></component>
  ```

- **Lihat juga:** [Dynamic Components](../guide/components.html#Dynamic-Components)

### transition

- **Props:**
  - `name` - string, Used to automatically generate transition CSS class names. e.g. `name: 'fade'` will auto expand to `.fade-enter`, `.fade-enter-active`, etc. Defaults to `"v"`.
  - `appear` - boolean, Whether to apply transition on initial render. Defaults to `false`.
  - `css` - boolean, Whether to apply CSS transition classes. Defaults to `true`. If set to `false`, will only trigger JavaScript hooks registered via component events.
  - `type` - string, Specify the type of transition events to wait for to determine transition end timing. Available values are `"transition"` and `"animation"`. By default, it will automatically detect the type that has a longer duration.
  - `mode` - string, Controls the timing sequence of leaving/entering transitions. Available modes are `"out-in"` and `"in-out"`; defaults to simultaneous.
  - `enter-class` - string
  - `leave-class` - string
  - `appear-class` - string
  - `enter-to-class` - string
  - `leave-to-class` - string
  - `appear-to-class` - string
  - `enter-active-class` - string
  - `leave-active-class` - string
  - `appear-active-class` - string

- **Events:**
  - `before-enter`
  - `before-leave`
  - `before-appear`
  - `enter`
  - `leave`
  - `appear`
  - `after-enter`
  - `after-leave`
  - `after-appear`
  - `enter-cancelled`
  - `leave-cancelled` (`v-show` only)
  - `appear-cancelled`

- **Penggunaan:**

  `<transition>` serve as transition effects for **single** element/component. The `<transition>` only applies the transition behavior to the wrapped content inside; it doesn't render an extra DOM element, or show up in the inspected component hierarchy.

  ```html
  <!-- simple element -->
  <transition>
    <div v-if="ok">toggled content</div>
  </transition>

  <!-- dynamic component -->
  <transition name="fade" mode="out-in" appear>
    <component :is="view"></component>
  </transition>

  <!-- event hooking -->
  <div id="transition-demo">
    <transition @after-enter="transitionComplete">
      <div v-show="ok">toggled content</div>
    </transition>
  </div>
  ```

  ``` js
  new Vue({
    ...
    methods: {
      transitionComplete: function (el) {
        // for passed 'el' that DOM element as the argument, something ...
      }
    }
    ...
  }).$mount('#transition-demo')
  ```

- **Lihat juga:** [Transitions: Entering, Leaving, and Lists](../guide/transitions.html)

### transition-group

- **Props:**
  - `tag` - string, defaults to `span`.
  - `move-class` - overwrite CSS class applied during moving transition.
  - exposes the same props as `<transition>` except `mode`.

- **Events:**
  - exposes the same events as `<transition>`.

- **Penggunaan:**

  `<transition-group>` serve as transition effects for **multiple** elements/components. The `<transition-group>` renders a real DOM element. By default it renders a `<span>`, and you can configure what element it should render via the `tag` attribute.

  Note every child in a `<transition-group>` must be **uniquely keyed** for the animations to work properly.

  `<transition-group>` supports moving transitions via CSS transform. When a child's position on screen has changed after an update, it will get applied a moving CSS class (auto generated from the `name` attribute or configured with the `move-class` attribute). If the CSS `transform` property is "transition-able" when the moving class is applied, the element will be smoothly animated to its destination using the [FLIP technique](https://aerotwist.com/blog/flip-your-animations/).

  ```html
  <transition-group tag="ul" name="slide">
    <li v-for="item in items" :key="item.id">
      {{ item.text }}
    </li>
  </transition-group>
  ```

- **Lihat juga:** [Transitions: Entering, Leaving, and Lists](../guide/transitions.html)

### keep-alive

- **Props:**
  - `include` - string or RegExp or Array. Only components with matching names will be cached.
  - `exclude` - string or RegExp or Array. Any component with a matching name will not be cached.
  - `max` - number. The maximum number of component instances to cache.

- **Penggunaan:**

  When wrapped around a dynamic component, `<keep-alive>` caches the inactive component instances without destroying them. Similar to `<transition>`, `<keep-alive>` is an abstract component: it doesn't render a DOM element itself, and doesn't show up in the component parent chain.

  When a component is toggled inside `<keep-alive>`, its `activated` and `deactivated` lifecycle hooks will be invoked accordingly.

  > In 2.2.0+ and above, `activated` and `deactivated` will fire for all nested components inside a `<keep-alive>` tree.

  Primarily used to preserve component state or avoid re-rendering.

  ```html
  <!-- basic -->
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>

  <!-- multiple conditional children -->
  <keep-alive>
    <comp-a v-if="a > 1"></comp-a>
    <comp-b v-else></comp-b>
  </keep-alive>

  <!-- used together with `<transition>` -->
  <transition>
    <keep-alive>
      <component :is="view"></component>
    </keep-alive>
  </transition>
  ```

  Note, `<keep-alive>` is designed for the case where it has one direct child component that is being toggled. It does not work if you have `v-for` inside it. When there are multiple conditional children, as above, `<keep-alive>` requires that only one child is rendered at a time.

- **`include` and `exclude`**

  > New in 2.1.0+

  The `include` and `exclude` props allow components to be conditionally cached. Both props can be a comma-delimited string, a RegExp or an Array:

  ``` html
  <!-- comma-delimited string -->
  <keep-alive include="a,b">
    <component :is="view"></component>
  </keep-alive>

  <!-- regex (use `v-bind`) -->
  <keep-alive :include="/a|b/">
    <component :is="view"></component>
  </keep-alive>

  <!-- Array (use `v-bind`) -->
  <keep-alive :include="['a', 'b']">
    <component :is="view"></component>
  </keep-alive>
  ```

  The match is first checked on the component's own `name` option, then its local registration name (the key in the parent's `components` option) if the `name` option is not available. Anonymous components cannot be matched against.

- **`max`**

  > New in 2.5.0+

  The maximum number of component instances to cache. Once this number is reached, the cached component instance that was least recently accessed will be destroyed before creating a new instance.

  ``` html
  <keep-alive :max="10">
    <component :is="view"></component>
  </keep-alive>
  ```

  <p class="tip">`<keep-alive>` does not work with functional components because they do not have instances to be cached.</p>

- **Lihat juga:** [Dynamic Components - keep-alive](../guide/components.html#keep-alive)

### slot

- **Props:**
  - `name` - string, Used for named slot.

- **Penggunaan:**

  `<slot>` serve as content distribution outlets in component templates. `<slot>` itself will be replaced.

  For detailed usage, see the guide section linked below.

- **Lihat juga:** [Content Distribution with Slots](../guide/components.html#Content-Distribution-with-Slots)

## VNode Interface

- Silakan merujuk ke [deklarasi class VNode](https://github.com/vuejs/vue/blob/dev/src/core/vdom/vnode.js).

## Server-Side Rendering

- Silakan merujuk ke [dokumentasi paket vue-server-renderer](https://github.com/vuejs/vue/tree/dev/packages/vue-server-renderer).
