---
title: Penanganan Event
type: guide
order: 9
---

## Memantau Event

Kita dapat menggunakan `v-on` direktif untuk memantau event DOM dan menjalankan beberapa JavaScript ketika dipicu.

Contoh:

``` html
<div id="contoh-1">
  <button v-on:click="counter += 1">Tambah 1</button>
  <p>Tombol diatas telah ditekan {{ counter }} kali.</p>
</div>
```
``` js
var contoh1 = new Vue({
  el: '#contoh-1',
  data: {
    counter: 0
  }
})
```

Hasil:

{% raw %}
<div id="contoh-1" class="demo">
  <button v-on:click="counter += 1">Add 1</button>
  <p>Tombol diatas telah ditekan {{ counter }} kali.</p>
</div>
<script>
var contoh1 = new Vue({
  el: '#contoh-1',
  data: {
    counter: 0
  }
})
</script>
{% endraw %}

## Metode Event Handler

Bagaimanapun baris kode untuk banyak event handler akan terlihat rumit, jadi menempatkan kode JavaScript Anda dalam `v-on` atribut tidaklah tepat. Itulah mengapa `v-on` dapat juga menerima nama metode yang ingin dijalankan.

Contoh:

``` html
<div id="contoh-2">
  <!-- `greet` adalah nama metode yang telah didefinisikan dibawah -->
  <button v-on:click="greet">Greet</button>
</div>
```

``` js
var contoh2 = new Vue({
  el: '#contoh-2',
  data: {
    name: 'Vue.js'
  },
  // mendefinisikan metode dibawah `methods` object
  methods: {
    greet: function (event) {
      // `this` di dalam metode merujuk ke Vue instance
      alert('Hello ' + this.name + '!')
      // `event` adalah bawaan DOM event
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})

// Anda juga dapat memanggil metode dalam JavaScript
contoh2.greet() // => 'Hello Vue.js!'
```

Hasil:

{% raw %}
<div id="contoh-2" class="demo">
  <button v-on:click="greet">Greet</button>
</div>
<script>
var contoh2 = new Vue({
  el: '#contoh-2',
  data: {
    name: 'Vue.js'
  },
  methods: {
    greet: function (event) {
      alert('Hello ' + this.name + '!')
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})
</script>
{% endraw %}

## Metode dalam Inline Handler

Dari pada menempatkan nama metode secara langsung, kita juga dapat menggunakan metode dalam sebuah inline JavaScript statement:

``` html
<div id="contoh-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
```
``` js
new Vue({
  el: '#contoh-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
```

Hasil:
{% raw %}
<div id="example-3" class="demo">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
<script>
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
</script>
{% endraw %}

Terkadang kita juga butuh akses ke event DOM bawaan dalam inline statement handler. Anda dapat menempatkan variabel khusus `$event` ke dalam sebuah metode:

``` html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```

``` js
// ...
methods: {
  warn: function (message, event) {
    // sekarang kita memiliki akses ke event bawaan
    if (event) event.preventDefault()
    alert(message)
  }
}
```

## Event Modifier

Merupakan kebutuhan umum memanggil `event.preventDefault()` atau `event.stopPropagation()` di dalam event handler. Meskipun kita dapat melakukannya dengan mudah di dalam metode, akan lebih baik jika metode murni hanya berisi logika data dari pada menangani rincian event DOM.

Untuk menyelesaikan masalah ini, Vue menyediakan  **event modifier** untuk `v-on`. Penggunaan modifier tersebut ialah dengan tambahan direktif diawali dengan simbol titik.

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

``` html
<!-- click event propagation akan dihentikan -->
<a v-on:click.stop="doThis"></a>

<!-- the submit event tidak lagi memuat ulang halaman -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- modifiers dapat ditulis berantai -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- sebuah modifier -->
<form v-on:submit.prevent></form>

<!-- gunakan mode capture saat menambahkan event listener -->
<!-- sebagai contoh, sebuah event menargetkan sebuah inner element ditangani disini sebelum ditangani oleh element tesebut -->
<div v-on:click.capture="doThis">...</div>

<!-- handler hanya dipicu jiks event.target adalah element itu sendiri -->
<!-- sebagai contoh, tidak dari child element -->
<div v-on:click.self="doThat">...</div>
```

<p class="tip">Urutan penting saat menggunakan modifier karena kode yang bersangkutan dibentuk dengan urutan yang sama. Maka dari itu, menggunakan `v-on:click.prevent.self` akan mencegah **semua klik** sementara `v-on:click.self.prevent` hanya akan mencegah klik pada element itu sendiri.</p>

> Baru pada versi 2.1.4+

``` html
<!-- click event akan dipicu paling banyak sekali -->
<a v-on:click.once="doThis"></a>
```

Tidak seperti modifier lainnya, yang khusus untuk event DOM bawaan, `.once` modifier juga dapat digunakan pada [komponen event](components-custom-events.html). Jika anda belum membaca tentang komponen, jangan khawatir tentang ini untuk sekarang.

> Baru pada versi 2.3.0+

Vue juga menawarkan `.passive` modifier, sesuai dengan [opsi `addEventListener`'s `passive`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#Parameters).

``` html
<!-- perilaku bawaan event scroll (scrolling) akan terjadi -->
<!-- seger, dari pada menunggu `onScroll` selesai -->
<!-- jika memuat `event.preventDefault()` -->
<div v-on:scroll.passive="onScroll">...</div>
```

`.passive` modifier secara khusus berguna meningkatkan performa pada perangkat mobile.

<p class="tip">Jangan menggunakan `.passive` dan `.prevent` bersamaan, karena `.prevent` akan diabaikan dan browser anda kemungkinan akan menampilkan peringatan. Ingat, `.passive` berkomunikasi dengan broswer yang mana anda tidak ingin mencegah perilaku bawaan event.</p>

## Key Modifier

Saat memantau keyboard event, kita sering membutuhkan untuk memeriksa key tertentu. Vue mengizinkan penambahan key modifier untuk `v-on` saat memantau key events:

``` html
<!-- hanya memanggil `vm.submit()` saat `key` adalah `Enter` -->
<input v-on:keyup.enter="submit">
```

Anda dapat secara langsung menggunakan nama key yang valid via [`KeyboardEvent.key`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) sebagai modifier dengan mengubahnya ke kebab-case.

``` html
<input v-on:keyup.page-down="onPageDown">
```

Pada contoh di atas, handler hanya akan dipanggail jika `$event.key` sama dengan `'PageDown'`.

### Key Code

<p class="tip">Penggunaan `keyCode` event [telah usang](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode) dan mungkin tidak didukung pada browser-browser baru.</p>

Menggunakan atribut `keyCode` juga diperbolehkan:

``` html
<input v-on:keyup.13="submit">
```

Vue menyediakan alias untuk key code umum jika dibutuhkan untuk mendukung browser lama:

- `.enter`
- `.tab`
- `.delete` (menangkap baik "Delete" dan "Backspace" key)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

<p class="tip">beberapa kunci (.esc dan semua tombol arah) memiliki nilai key yang tidak konsisten pada peramban IE9, jadi alias yang telah tersedia di atas lebih disarankan jika anda butuh dukungan untuk peramban IE9.</p>

Anda juga dapat [mendefinisikan kustom alias key modifier](../api/#keyCodes) melalui global config.keyCodes object:

``` js
// Mengaktifkan `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

## Sistem Modifier Key

> Baru pada versi 2.1.0+

Anda dapat menggunakan modifier dibawah untuk memicu mouse atau keyboard event listener hanya pada saat modifier tertentu ditekan:

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

> Note: pada keyboard Macintosh, meta adalah command key (⌘). Pada keyboard Windows, meta adalah windows key (⊞). Pada keyboard Sun Microsystems, meta ditandai dengan solid diamond (◆). Pada keyboard tertentu, khususnya keyboard mesin MIT and Lisp dan penerusnya, seperti the Knight keyboard, space-cadet keyboard, meta diberi label “META”. Pada keyboard Symbolics, meta diberi label “META” atau “Meta”.

Contoh:

```html
<!-- Alt + C -->
<input @keyup.alt.67="clear">

<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```

<p class="tip">Perhatikan bahwa modifier key berbeda dengan key pada umumnya dan saat digunakan dengan `keyup` event, key tersebut harus ditekan saat event dijalankan. Dengan kata lain, `keyup.ctrl` hanya akan dipicu ketika Anda melepas key saat masih menekan `ctrl`. Hal tersebut tidak akan dipicu jika Anda melepas `ctrl` key sendiri. Jika Anda tidak ingin perilaku seperti itu, gunakan `keyCode` untuk `ctrl` alih-alih: `keyup.17`.</p>

### `.exact` Modifier

> Baru pada versi 2.5.0+

`.exact` modifier mengizinkan kontrol kombinasi eksak sistem modifier yang dibutuhkan untuk memicu event.

``` html
<!-- baris ini akan mengeksekusi metode bahkan jika tombol Alt atau Shift juga ditekan -->
<button @click.ctrl="onClick">A</button>

<!-- baris ini hanya akan mengeksekusi metode saat tombol Ctrl ditekan tanpa diikuti tombol lain -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- baris ini hanya akan mengeksekusi metode saat tidak menekan sistem modifier keys -->
<button @click.exact="onClick">A</button>
```

### Tombol Mouse Modifier

> Baru pada versi 2.2.0+

- `.left`
- `.right`
- `.middle`

Modifier ini membatasi handler pada event saat dipicu oleh tombol mouse tertentu.

## Mengapa Listener di dalam HTML?

Anda mungkin menyoroti bahwa keseluruhan pendekatan event listening menyalahi aturan lama tentang "separation of concerns". Kebanyakan meyakini itu - sejak semua fungsi-fungsi dan ekspresi Vue handler terikat secara terbatas pada ViewModel yang mana menangani view yang berhubungan, hal itu tidak akan mempersulit pemeliharaan. Faktanya, terdapat beberapa keuntungan penggunaan `v-on`:

1. Mempermudah penempatan implementasi fungsi handler diantara baris kode JS dengan menelusuri sekilas HTML templat.

2. Sejak anda tidak membutuhkan peletakkan event listeners dalam JS secara manual, baris kode ViewModel menjadi berisi murni logika dan bebas DOM. Ini memudahkan dalam pengujian.

3. Saat ViewModel dihapus, semua event listener secara otomatis dihapus. Anda tidak perlu khawatir tentang membersihkan event yang ada.