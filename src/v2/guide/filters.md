---
title: Filter
type: guide
order: 305
---

Di Vue.js Anda dapat mendeklarasikan atau membuat kustom filter yang dapat digunakan untuk memformat teks. Filter digunakan dalam dua tempat: **mustache interpolations** dan ekspresi `v-bind` (didukung pada versi 2.1.0+). Filter harus ditambahkan pada bagian akhir atau setelahnya dengan menggunakan ekspresi JavaScript, dipisahkan dengan simbol "pipe" (&nbsp;|&nbsp;):

``` html
<!-- di dalam sintaks `mustaches` -->
{{ message | capitalize }}

<!-- di dalam `v-bind` -->
<div v-bind:id="rawId | formatId"></div>
```

Anda dapat mendeklarasikan filter ke dalam opsi komponen.

``` js
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```

atau mendeklarasikan global filter sebelum membuat *instance* Vue:

``` js
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})

// Vue Instance
new Vue({
  // ...
})
```

Di bawah ini contoh dari filter huruf kapital (`capitalize`) yang digunakan:

{% raw %}
<div id="example_1" class="demo">
  <input type="text" v-model="message">
  <p>{{ message | capitalize }}</p>
</div>
<script>
  new Vue({
    el: '#example_1',
    data: function () {
      return {
        message: 'john'
      }
    },
    filters: {
      capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
      }
    }
  })
</script>
{% endraw %}

*Function* atau *method* filter selalu menerima nilai dari ekspresi nilai (hasil rantai pertama) sebagai argumen/parameter awal. Pada contoh di atas, filter dari fungsi `capitalize` akan menerima nilai dari `message` sebagai argumen-nya.

Filter dapat dijadikan berantai (metode *chain*):

``` html
{{ message | filterA | filterB }}
```

Dalam hal ini, `filterA` yang dideklarasikan dengan satu argumen, akan menerima nilai dari `message`. Selanjutnya fungsi `filterB` akan dipanggil dengan hasil dari fungsi `filterA` yang dimasukkan ke dalam 
`filterB` sebagai argumen tunggal.

Karena filter merupakan *fungsi* atau *method Javascript*, maka dapat digunakan lebih dari satu argumen atau parameter.

``` html
{{ message | filterA('arg1', arg2) }}
```

Di sini `filterA` dideklarasikan sebagai sebuah fungsi yang memiliki tiga buah argumen. Nilai atau *value* dari `message` akan dimasukkan ke dalam argumen pertama. Variabel bertipe string dari `'arg1'` akan dimasukkan ke dalam `filterA` sebagai argumen kedua, dan nilai dari ekspresi `arg2` akan dievaluasi dan dimasukkan sebagai parameter ketiga.
