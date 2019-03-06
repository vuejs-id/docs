---
title: Template Syntax
type: guide
order: 4
---
Vue.js menggunakan sintaks templat berbasis HTML yang memungkinkan anda secara deklaratif untuk mengikat hasil render DOM untuk yang mendasari instant data Vue. Semua templat Vue.js adalah HTML yang valid yang dapat diuraikan oleh browser yang sesuai spesifikasi dan pengurai HTML.

Pada dasarnya, Vue mengkompilasi templat ke dalam fungsi render Virtual DOM. Dikombinasikan dengan sistem reaktivitas, Vue mampu secara cerdas mencari tahu jumlah minimum komponen untuk render ulang dan menerapkan jumlah minimal manipulasi DOM ketika state pada aplikasi berubah.

Jika anda terbiasa dengan konsep Virtual DOM dan lebih suka native JavaScript, anda juga dapat [menulis langsung fungsi render](render-function.html) pengganti templat, dengan opsi dukungan JSX.

## Interpolasi

### Teks

Bentuk paling mendasar dari data binding adalah interpolasi teks menggunakan sintaksis "Kumis" (kurung kurawal ganda):

```html
<span>Pesan: {{ msg }}</span>
```

Tag kumis akan di ganti dengan nilai property `msg` pada objek data yang sesuai. Ini juga akan diperbarui setiap kali properti objek data `msg` berubah.

Anda juga dapat melakukan interpolasi satu kali yang tidak memperbarui pada perubahan data dengan menggunakan direktif [v-once](../api/#v-once), tetapi perlu diingat ini juga akan mempengaruhi binding lain pada node yang sama:

```html
<span v-once>Ini tidak akan berubah: {{ msg }}</span>
```

### Raw HTML

Tanda kumis ganda menerjemahkan data sebagai teks biasa, bukan HTML. Untuk menghasilkan HTML asli, gunakan direktif `v-html`:

```html
<p>Menggunakan tanda kumis: {{ rawHtml }}</p>
<p>Menggunakan direktif v-html: <span v-html="rawHtml"></span></p>
```

{% raw %}

<div id="example1" class="demo">
  <p>Menggunakan tanda kumis: {{ rawHtml }}</p>
  <p>Menggunakan direktif v-html: <span v-html="rawHtml"></span></p>
</div>
<script>
new Vue({
  el: '#example1',
  data: function () {
    return {
      rawHtml: '<span style="color: red">Ini berwarna merah.</span>'
    }
  }
})
</script>
{% endraw %}

Konten `span` akan diganti dengan nilai properti `rawHtml`, diterjemahkan sebagai HTML biasa - binding data diabaikan. Perhatikan bahwa anda tidak dapat menggunakan `v-html` untuk membuat parsial templat, karena Vue bukan mesin templating berbasis String. Sebaliknya, komponen lebih disukai sebagai unit dasar UI yang bisa digunakan kembali.

<p class="tip">Merender HTML yang berubah-ubah secara dinamis di situs web anda bisa sangat berbahaya karena dapat mudah terserang [kerentanan XSS](https://en.wikipedia.org/wiki/Cross-site_scripting). Gunakan interpolasi HTML pada konten yang terpercaya dan **jangan pernah** pada konten yang disediakan pengguna.</p> 

### Atribut

Tanda kumis tidak dapat digunakan di dalam atribut HTML. Sebagai gantinya, gunakan direktif [v-bind](../api/#v-bind):

```html
<div v-bind:id="dynamicId"></div>
```

Pada kasus atribut tipe boolean, di mana keberadaan nilainya `true`, penggunaan `v-bind` sedikit berbeda. Dalam contoh ini:

```html
<button v-bind:disabled="isButtonDisabled">Tombol</button>
```

Jika `isButtonDisabled` bernilai `null`, `undefined`, atau `false`, atribut `disabled` tidak akan dimasukkan ke dalam elemen `<button>`. 

### Menggunakan Ekspresi JavaScript

Sejauh ini kami hanya binding properti sederhana di template kami. Tetapi Vue.js sebenarnya mendukung penuh ekspresi JavaScript di dalam semua binding data:

```html
{{ number + 1 }} {{ ok ? 'YES' : 'NO' }} {{ message.split('').reverse().join('')
}}

<div v-bind:id="'list-' + id"></div>
```

Ekspresi ini akan dievaluasi sebagai JavaScript dalam scope data dari Vue instance. Satu batasan adalah bahwa setiap binding hanya dapat berisi **one single expression**, jadi yang berikut ini **TIDAK** akan berfungsi:

```html
<!-- ini adalah sebuah statemen, bukan sebuah ekspresi: -->
{{ var a = 1 }}

<!-- kontrol flow tidak akan bekerja, gunakan ekspresi ternary -->
{{ if (ok) { return message } }}
```

<p class="tip">Ekspresi templat adalah sandboxed dan hanya memiliki akses ke daftar putih global seperti `Matematika` dan `Tanggal`. Anda tidak boleh mencoba mengakses pendefinisian global dalam ekspresi templat.</p>

## Direktif

Direktif (arahan) adalah atribut khusus dengan awalan `v-`. Nilai atribut direktif diharapkan menjadi **sebuah ekspresi JavaScript tunggal** (dengan pengecualian `v-for`, yang akan dibahas nanti). Tugas direktif adalah menerapkan efek pada DOM secara reaktif ketika nilai ekspresinya berubah. Mari kita lihat pada contoh terdahulu:

```html
<p v-if="seen">Sekarang anda bisa melihatku</p>
```

Di sini, direktif `v-if` akan menghapus / memasukkan elemen `<p>` berdasarkan kebenaran dari nilai ekspresi `seen`.

### Arguments

Some directives can take an "argument", denoted by a colon after the directive name. For example, the `v-bind` directive is used to reactively update an HTML attribute:

```html
<a v-bind:href="url"> ... </a>
```

Here `href` is the argument, which tells the `v-bind` directive to bind the element's `href` attribute to the value of the expression `url`.

Another example is the `v-on` directive, which listens to DOM events:

```html
<a v-on:click="doSomething"> ... </a>
```

Here the argument is the event name to listen to. We will talk about event handling in more detail too.

### Dynamic Arguments

> New in 2.6.0+

Starting in version 2.6.0, it is also possible to use a JavaScript expression in a directive argument by wrapping it with square brackets:

```html
<a v-bind:[attributeName]="url"> ... </a>
```

Here `attributeName` will be dynamically evaluated as a JavaScript expression, and its evaluated value will be used as the final value for the argument. For example, if your Vue instance has a data property, `attributeName`, whose value is `"href"`, then this binding will be equivalent to `v-bind:href`.

Similarly, you can use dynamic arguments to bind a handler to a dynamic event name:

```html
<a v-on:[eventName]="doSomething"> ... </a>
```

Similarly, when `eventName`'s value is `"focus"`, for example, `v-on:[eventName]` will be equivalent to `v-on:focus`.

#### Dynamic Argument Value Constraints

Dynamic arguments are expected to evaluate to a string, with the exception of `null`. The special value `null` can be used to explicitly remove the binding. Any other non-string value will trigger a warning.

#### Dynamic Argument Expression Constraints

<p class="tip">Dynamic argument expressions have some syntax constraints because certain characters are invalid inside HTML attribute names, such as spaces and quotes. You also need to avoid uppercase keys when using in-DOM templates.</p>

For example, the following is invalid:

```html
<!-- This will trigger a compiler warning. -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

The workaround is to either use expressions without spaces or quotes, or replace the complex expression with a computed property.

In addition, if you are using in-DOM templates (templates directly written in an HTML file), you have to be aware that browsers will coerce attribute names into lowercase:

```html
<!-- This will be converted to v-bind:[someattr] in in-DOM templates. -->
<a v-bind:[someAttr]="value"> ... </a>
```

### Modifiers

Modifiers are special postfixes denoted by a dot, which indicate that a directive should be bound in some special way. For example, the `.prevent` modifier tells the `v-on` directive to call `event.preventDefault()` on the triggered event:

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

You'll see other examples of modifiers later, [for `v-on`](events.html#Event-Modifiers) and [for `v-model`](forms.html#Modifiers), when we explore those features.

## Shorthands

The `v-` prefix serves as a visual cue for identifying Vue-specific attributes in your templates. This is useful when you are using Vue.js to apply dynamic behavior to some existing markup, but can feel verbose for some frequently used directives. At the same time, the need for the `v-` prefix becomes less important when you are building a [SPA](https://en.wikipedia.org/wiki/Single-page_application), where Vue manages every template. Therefore, Vue provides special shorthands for two of the most often used directives, `v-bind` and `v-on`:

### `v-bind` Shorthand

```html
<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a :[key]="url"> ... </a>
```

### `v-on` Shorthand

```html
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

They may look a bit different from normal HTML, but `:` and `@` are valid characters for attribute names and all Vue-supported browsers can parse it correctly. In addition, they do not appear in the final rendered markup. The shorthand syntax is totally optional, but you will likely appreciate it when you learn more about its usage later.
