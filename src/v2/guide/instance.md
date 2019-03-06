---
title: Vue Instance
type: guide
order: 3
---

## Membuat *Vue Instance*

Setiap aplikasi Vue dimulai dengan membuat **Vue Instance** dengan fungsi `Vue` seperti berikut :

```js
var vm = new Vue({
  // opsi
})
```

Meskipun tidak dengan terkait dengan ketat dengan [Pola MVVM](https://en.wikipedia.org/wiki/Model_View_ViewModel), desain yang dimiliki Vue sebagian terinspirasi oleh konsep tersebut. Sebagai ketentuan, kita kadang menggunakan variabel `vm` ( kependekan dari ViewModel ) untuk mengacu pada *Vue Instance* kami.


Ketika anda membuat *Vue Instance*, anda mengirimkan **obyek opsi**. Sebagian besar dari panduan ini menjelaskan bagaimana anda bisa menggunakan opsi ini untuk membuat tindakan yang anda inginkan. Sebagai contoh, Anda juga bisa melihat lihat daftar lengkap dari opsi ini di [Contoh API](../api/#Options-Data).


Sebuah aplikasi Vue terdiri dari **permintaan dasar Vue** yang dibuat dengan `new Vue`, secara opsional di atur ke dalam akar (root) *Vue Instance*, komponen yang bisa digunakan ulang. Sebagai contoh, komponen aplikasi pengingat mungkin terlihat seperti ini :


```
Root Instance
└─ TodoList
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```

Kita akan membicarakan tentang [sistem komponen](components.html) pada detail selanjutnya. Sekarang, dengan hanya mengetahui bahwa semua komponen Vue juga adalah *Vue Instance*, dan juga menerima opsi objek yang sama (kecuali untuk beberapa opsi *tree of nested*).


## Data dan Metode

Ketika *Vue Instance* dibuat, ini menambah semua properti yang ditemukan di dalam objek `data` ke Vue's **sistem reaktifitas**. Ketika nilai dari properti tersebut berubah, tampilanya akan "bereaksi", memperbarui untuk mencocokan dengan nilai yang baru.


```js
// Objek data kita
var data = { a: 1 }

// Objek ditambahkan kedalam Vue Instance
var vm = new Vue({
  data: data
})

// Mendapat properti dari permintaan
// lalu mengembalikan dari data asli
vm.a == data.a // => true

// Mengatur properti pada permintaan
// yang juga berdampak pada data asli
vm.a = 2
data.a // => 2

// ... dan kebalikanya
data.a = 3
vm.a // => 3
```

Ketika data ini berubah, tampilanya akan berubah. Itu harus dicatat bahwa properti di `data` adalah yang satu satunya **reaktif** jika mereka ada pada saat permintaan dibuat. Itu berarti kamu harus menambah properti baru, seperti:


```js
vm.b = 'halo'
```

Lalu mengubah ke `b` tidak akan memicu perubahan lain pada tampilan. Jika anda mengetahui bahwa anda memerlukan properti nanti, tapi ini dimulai dengan kosong atau tidak ada, anda harus memberi beberapa nilai awal. Sebagai contoh:


```js
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

Satu satunya pengecualian untuk ini adalah menggunakan `Object.freeze()`, yang mencegah properti untuk dirubah, yang juga berarti reaktifitas sistem yang tidak bisa _menarik_ perubahan.


```js
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
```

```html
<div id="app">
  <p>{{ foo }}</p>
  <!-- ini tidak akan mengubah `foo`! -->
  <button v-on:click="foo = 'baz'">Change it</button>
</div>
```

Untuk tambahan ke data properti, *Vue Instance* menunjukan sejumlah properti permintaan dan metode yang berguna. Ini juga diawalai dengan `$` untuk membedakan mereka dari properti yang ditetapkan-pengguna. Sebagai contoh:


```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch adalah metode permintaan
vm.$watch('a', function (newValue, oldValue) {
  // Panggilan balik ini akan di panggil ketika `vm.a` berubah
})
```

Selanjutnya, anda bisa berkonsultasi ke [Referensi API](../api/#Instance-Properties) untuk daftar lengkap dari properti permintaan dan metode.


## Siklus Hidup Pengait Permintaan

Setiap *Vue Instance* melewati beberapa serangkaian langkan inisiasi pada saat dibuat - sebagai contoh, ini perlu diatur observasi data, memproses templat, memasang permintaan ke DOM, dan memperbarui DOM ketika data berubah. Sepanjang proses, ini juga berjalan dengan fungsi bernama **siklus hidup pengait**, memberi pengguna kesempatan untuk menambah kode mereka sendiri pada tahapan tertentu.

Sebagai contoh, pengait [`created`](../api/#created) bisa digunakan untuk menjalankan kode setelah permintaan ini dibuat:

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` menunjuk ke poin permintaan vm
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

Ada juga pengait lain yang akan dipanggil pada tahapan berbeda di siklus hidup permintaan, seperti [`mounted`](../api/#mounted), [`updated`](../api/#updated), dan [`destroyed`](../api/#destroyed). Semua siklus hidup pengait dipanggil dengan konteks `this` mereka yang menunjuk ke konteks *Vue Instance* yang menjalankanya.

<p class="tip">Jangan menggunakan [fungsi panah](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) pada opsi properti atau pemanggilan ulang, seperti `created: () => console.log(this.a)` or `vm.$watch('a', newValue => this.myMethod())`. Sejak fungsi panah terikat dengan konteks induk, `this` tidak akan menjadi *Vue Instance* seperti yang anda harapkan, kadang menyebabkan kesalahan seperti `Uncaught TypeError: Cannot read property of undefined` atau `Uncaught TypeError: this.myMethod is not a function`.</p>

## Diagram Siklus Hidup

Dibawah ini adalah diagram dari siklus hidup permintaan. Anda tidak perlu memahami dengan penuh apapun yang terjadi sekarang, tapi selama anda belajar dan terus membuat, ini akan menjadi referensi yang berguna


![The *Vue Instance* Lifecycle](/images/lifecycle.png)
