---
title: Slots
type: guide
order: 104
---

> Sebelum lanjut membaca halaman ini, kami berasumsi bahwa Anda telah membaca [Dasar-Dasar Komponen](components.html). Baca halaman itu terlebih dahulu bila Anda belum mengerti tentang komponen.

> Di versi 2.6.0, kami memperkenalkan sintaks baru (direktif `v-slot`) untuk nama dan slot *scope*. Sintaks tersebut menggantikan `slot` dan atribut `slot-scope`, yang sekarang telah usang, tapi masih belum dihapus dan masih didokumentasikan [di sini](#Deprecated-Syntax). Alasan kami untuk memperkenalkan sintaks baru dapat dilihat di [RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0001-new-slot-syntax.md).

## Konten Slot

Vue mengimplementasikan *API* distribusi konten yang terinspirasi dari [Draf Spesifikasi Komponen Web,](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md), dan elemen `<slot>` dapat digunakan sebagai outlet distribusi untuk konten kita.

Ini memungkinkan Anda untuk membuat komponen seperti ini:

``` html
<navigation-link url="/profile">
  Profil Anda
</navigation-link>
```

Kemudian di templat `<navigation-link>`, Anda mungkin memiliki templat seperti ini:

``` html
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>
```

Ketika komponen di-*render*, `<slot></slot>` akan menggantinya dengan "Profil Anda". Elemen slot dapat berisi kode templat apa saja, termasuk HTML:

``` html
<navigation-link url="/profile">
  <!-- Menambahkan ikon Font Awesome -->
  <span class="fa fa-user"></span>
  Profil Anda
</navigation-link>
```

Atau bahkan dapat berisi komponen lain:

``` html
<navigation-link url="/profile">
  <!-- Menggunakan komponen untuk menambahkan ikon -->
  <font-awesome-icon name="user"></font-awesome-icon>
  Profil Anda
</navigation-link>
```

Jika templat `<navigation-link>` tidak berisi elemen `<slot>`, konten apapun yang kita sediakan di dalam komponen `<navigation-link>` akan di buang.

## Kompilasi Scope

Saat Anda ingin menggunakan data didalam slot, seperti ini:

``` html
<navigation-link url="/profile">
  Masuk sebagai {{ user.name }}
</navigation-link>
```

Slot dapat mengakses properti *instance* yang sama (*scope* yang sama). Slot **tidak** dapat mengakses ke *scope* `<navigation-link>`. Misalnya, Anda mencoba untuk mengakses data `url` dari *scope* `<navigation-link>`, itu tidak akan bisa:

``` html
<navigation-link url="/profile">
  Klik disini dan Anda akan diarahkan ke: {{ url }}
  <!--
  Data `url` akan *undefined*, karena tidak di definisikan _didalam_ komponen
  <navigation-link>, akan tetapi data `url` akan dilanjutkan ke
  _ke_ templat <navigation-link>.
  -->
</navigation-link>
```

Ingat, bahwa aturannya:

> Semua yang ada di templat induk, akan dikompilasi didalam *scope* induk; semua yang ada di templat anak, akan dikompilasi didalam *scope* anak.

## Konten Fallback

Ada beberapa kasus yang bermanfaat untuk menentukan konten *fallback* (*default*) slot, yang hanya akan di-*render* ketika konten slot telah kita sediakan. Misalnya, didalam komponen `<submit-button>`:

```html
<button type="submit">
  <slot></slot>
</button>
```

Kita mungkin ingin me*render* teks "Submit" didalam `<button>` setiap saat. Untuk membuatnya, kita bisa menempatkan teks "Submit" di antara tag `<slot>`:

```html
<button type="submit">
  <slot>Submit</slot>
</button>
```

Sekarang saat kita menggunakan `<submit-button>` sebagai komponen induk, yang tidak kita sediakan konten didalamnya:

```html
<submit-button></submit-button>
```

Komponen tersebut akan me*render* konten *fallback* (*default*) yang telah kita sediakan sebelumnya:

```html
<button type="submit">
  Submit
</button>
```

Akan tetapi jika kita menyediakan konten di komponen tersebut:

```html
<submit-button>
  Save
</submit-button>
```

Konten tersebut akan di-*render* sebagai gantinya:

```html
<button type="submit">
  Save
</button>
```

## Slot yang Diberi Nama

> Diperbarui di versi 2.6.0+. [Lihat disini](#Deprecated-Syntax) untuk sintaks yang telah usang, yang menggunakan atribut `slot`.

Ada kalanya memiliki banyak slot itu bermanfaat. Misalnya, didalam komponen `<base-layout>` dengan templat berikut ini:

``` html
<div class="container">
  <header>
    <!-- Kita ingin konten *header* disini -->
  </header>
  <main>
    <!-- Kita ingin konten *main* disini -->
  </main>
  <footer>
    <!-- Kita ingin konten *footer* disini -->
  </footer>
</div>
```

Pada kasus ini, elemen `<slot>` memiliki atribut khusus, `name`, yang dapat digunakan untuk mendefinisikan slot tambahan:

``` html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

`<slot>` tanpa menggunakan `name` secara implisit memiliki nama "default".

Untuk menyediakan konten di slot yang diberi nama, kita bisa menggunakan direktif `v-slot` di `<template>` yang menggunakan nama slot sebagai argumen `v-slot`:

```html
<base-layout>
  <template v-slot:header>
    <h1>Disini mungkin untuk judul halaman</h1>
  </template>

  <p>Paragraf untuk konten utama</p>
  <p>Dan satu lagi</p>

  <template v-slot:footer>
    <p>Disini untuk beberapa info kontak</p>
  </template>
</base-layout>
```

Sekarang semua yang ada di dalam templat `<template>` akan dilanjutkan ke slot yang sesuai. Konten apa pun yang tidak di bungkus dengan `<template>` yang menggunakan `v-slot`, itu akan diasumsikan sebagai slot default.

Namun, Anda masih bisa membungkus konten slot *default* di `<template>` jika Anda ingin lebih eksplisit:

```html
<base-layout>
  <template v-slot:header>
    <h1>Disini mungkin untuk judul halaman</h1>
  </template>

  <template v-slot:default>
    <p>Paragraf untuk konten utama</p>
    <p>Dan satu lagi</p>
  </template>

  <template v-slot:footer>
    <p>Disini untuk beberapa info kontak</p>
  </template>
</base-layout>
```

HTML yang kita tempatkan di antara templat `<template>` akan di-*render* seperti ini:

``` html
<div class="container">
  <header>
    <h1>Disini mungkin untuk judul halaman</h1>
  </header>
  <main>
    <p>Paragraf untuk konten utama</p>
    <p>Dan satu lagi</p>
  </main>
  <footer>
    <p>Disini untuk beberapa info kontak</p>
  </footer>
</div>
```

Perhatikan bahwa **`v-slot` hanya bisa ditambahkan di `<template>`** (dengan [satu pengecualian](#Abbreviated-Syntax-for-Lone-Default-Slots)), dan tidak usang seperti [atribut `slot`](#Deprecated-Syntax).

## Slot Scope

> Diperbarui di versi 2.6.0+. [Lihat disini](#Deprecated-Syntax) untuk sintaks yang telah usang, yang menggunakan atribut `slot-scope`.

Terkadang, konten slot yang hanya memiliki akses ke data yang tersedia di komponen anak, itu sangat berguna. Misalnya, bayangkan jika komponen `<current-user>` dengan templat berikut ini:

```html
<span>
  <slot>{{ user.lastName }}</slot>
</span>
```
Ingin kita ganti dengan komponen *fallback* (*default*) dengan nama depan pengguna, seperti ini:

``` html
<current-user>
  {{ user.firstName }}
</current-user>
```

Itu tidak akan bisa, karena komponen `<current-user>` hanya dapat mengakses data `user` dan konten yang kita sediakan di-*render* di induk.

Untuk membuat data `user` tersedia di induk konten slot, kita bisa *bind* data `user` sebagai attribut di elemen `<slot>`:

``` html
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```

Atribut yang di-*bind* di elemen `<slot>` diberi nama **slot props**. Sekarang, di *scope* induk, kita bisa menggunakan `v-slot` dengan nilai untuk mendefinisikan nama slot *props* yang telah kita sediakan:

``` html
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```

Di contoh ini, kita memilih untuk memberi nama objek yang berisi semua slot *props* `slotProps` kami, tetapi Anda bisa menggunakan nama apa pun yang Anda suka.

### Sintaks yang Diringkas untuk Slot Default yang Sendirian

In cases like above, when _only_ the default slot is provided content, the component's tags can be used as the slot's template. This allows us to use `v-slot` directly on the component:

``` html
<current-user v-slot:default="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

This can be shortened even further. Just as non-specified content is assumed to be for the default slot, `v-slot` without an argument is assumed to refer to the default slot:

``` html
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

Note that the abbreviated syntax for default slot **cannot** be mixed with named slots, as it would lead to scope ambiguity:

``` html
<!-- INVALID, will result in warning -->
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
  <template v-slot:other="otherSlotProps">
    slotProps is NOT available here
  </template>
</current-user>
```

Whenever there are multiple slots, use the full `<template>` based syntax for _all_ slots:

``` html
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>

  <template v-slot:other="otherSlotProps">
    ...
  </template>
</current-user>
```

### Destructuring Slot Props

Internally, scoped slots work by wrapping your slot content in a function passed a single argument:

```js
function (slotProps) {
  // ... slot content ...
}
```

That means the value of `v-slot` can actually accept any valid JavaScript expression that can appear in the argument position of a function definition. So in supported environments ([single-file components](single-file-components.html) or [modern browsers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Browser_compatibility)), you can also use [ES2015 destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Object_destructuring) to pull out specific slot props, like so:

``` html
<current-user v-slot="{ user }">
  {{ user.firstName }}
</current-user>
```

This can make the template much cleaner, especially when the slot provides many props. It also opens other possibilities, such as renaming props, e.g. `user` to `person`:

``` html
<current-user v-slot="{ user: person }">
  {{ person.firstName }}
</current-user>
```

You can even define fallbacks, to be used in case a slot prop is undefined:

``` html
<current-user v-slot="{ user = { firstName: 'Guest' } }">
  {{ user.firstName }}
</current-user>
```

## Dynamic Slot Names

> New in 2.6.0+

[Dynamic directive arguments](syntax.html#Dynamic-Arguments) also work on `v-slot`, allowing the definition of dynamic slot names:

``` html
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

## Named Slots Shorthand

> New in 2.6.0+

Similar to `v-on` and `v-bind`, `v-slot` also has a shorthand, replacing everything before the argument (`v-slot:`) with the special symbol `#`. For example, `v-slot:header` can be rewritten as `#header`:

```html
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

However, just as with other directives, the shorthand is only available when an argument is provided. That means the following syntax is invalid:

``` html
<!-- This will trigger a warning -->
<current-user #="{ user }">
  {{ user.firstName }}
</current-user>
```

Instead, you must always specify the name of the slot if you wish to use the shorthand:

``` html
<current-user #default="{ user }">
  {{ user.firstName }}
</current-user>
```

## Other Examples

**Slot props allow us to turn slots into reusable templates that can render different content based on input props.** This is most useful when you are designing a reusable component that encapsulates data logic while allowing the consuming parent component to customize part of its layout.

For example, we are implementing a `<todo-list>` component that contains the layout and filtering logic for a list:

```html
<ul>
  <li
    v-for="todo in filteredTodos"
    v-bind:key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```

Instead of hard-coding the content for each todo, we can let the parent component take control by making every todo a slot, then binding `todo` as a slot prop:

```html
<ul>
  <li
    v-for="todo in filteredTodos"
    v-bind:key="todo.id"
  >
    <!--
    We have a slot for each todo, passing it the
    `todo` object as a slot prop.
    -->
    <slot name="todo" v-bind:todo="todo">
      <!-- Fallback content -->
      {{ todo.text }}
    </slot>
  </li>
</ul>
```

Now when we use the `<todo-list>` component, we can optionally define an alternative `<template>` for todo items, but with access to data from the child:

```html
<todo-list v-bind:todos="todos">
  <template v-slot:todo="{ todo }">
    <span v-if="todo.isComplete">✓</span>
    {{ todo.text }}
  </template>
</todo-list>
```

However, even this barely scratches the surface of what scoped slots are capable of. For real-life, powerful examples of scoped slot usage, we recommend browsing libraries such as [Vue Virtual Scroller](https://github.com/Akryum/vue-virtual-scroller), [Vue Promised](https://github.com/posva/vue-promised), and [Portal Vue](https://github.com/LinusBorg/portal-vue).

## Deprecated Syntax

> The `v-slot` directive was introduced in Vue 2.6.0, offering an improved, alternative API to the still-supported `slot` and `slot-scope` attributes. The full rationale for introducing `v-slot` is described in this [RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0001-new-slot-syntax.md). The `slot` and `slot-scope` attributes will continue to be supported in all future 2.x releases, but are officially deprecated and will eventually be removed in Vue 3.

### Named Slots with the `slot` Attribute

> <abbr title="Still supported in all 2.x versions of Vue, but no longer recommended.">Deprecated</abbr> in 2.6.0+. See [here](#Named-Slots) for the new, recommended syntax.

To pass content to named slots from the parent, use the special `slot` attribute on `<template>` (using the `<base-layout>` component described [here](#Named-Slots) as example):

```html
<base-layout>
  <template slot="header">
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template slot="footer">
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

Or, the `slot` attribute can also be used directly on a normal element:

``` html
<base-layout>
  <h1 slot="header">Here might be a page title</h1>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <p slot="footer">Here's some contact info</p>
</base-layout>
```

There can still be one unnamed slot, which is the **default slot** that serves as a catch-all for any unmatched content. In both examples above, the rendered HTML would be:

``` html
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

### Scoped Slots with the `slot-scope` Attribute

> <abbr title="Still supported in all 2.x versions of Vue, but no longer recommended.">Deprecated</abbr> in 2.6.0+. See [here](#Scoped-Slots) for the new, recommended syntax.

To receive props passed to a slot, the parent component can use `<template>` with the `slot-scope` attribute (using the `<slot-example>` described [here](#Scoped-Slots) as example):

``` html
<slot-example>
  <template slot="default" slot-scope="slotProps">
    {{ slotProps.msg }}
  </template>
</slot-example>
```

Here, `slot-scope` declares the received props object as the `slotProps` variable, and makes it available inside the `<template>` scope. You can name `slotProps` anything you like similar to naming function arguments in JavaScript.

Here `slot="default"` can be omitted as it is implied:

``` html
<slot-example>
  <template slot-scope="slotProps">
    {{ slotProps.msg }}
  </template>
</slot-example>
```

The `slot-scope` attribute can also be used directly on a non-`<template>` element (including components):

``` html
<slot-example>
  <span slot-scope="slotProps">
    {{ slotProps.msg }}
  </span>
</slot-example>
```

The value of `slot-scope` can accept any valid JavaScript expression that can appear in the argument position of a function definition. This means in supported environments ([single-file components](single-file-components.html) or [modern browsers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Browser_compatibility)) you can also use [ES2015 destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Object_destructuring) in the expression, like so:

``` html
<slot-example>
  <span slot-scope="{ msg }">
    {{ msg }}
  </span>
</slot-example>
```

Using the `<todo-list>` described [here](#Other-Examples) as an example, here's the equivalent usage using `slot-scope`:

``` html
<todo-list v-bind:todos="todos">
  <template slot="todo" slot-scope="{ todo }">
    <span v-if="todo.isComplete">✓</span>
    {{ todo.text }}
  </template>
</todo-list>
```
