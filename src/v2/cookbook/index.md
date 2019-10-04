---
title: Introduction
type: cookbook
order: 0
---

## Buku Petunjuk vs Petunjuk

Bagaimana buku petunjuk berbeda dengan petunjuk? Mengapa hal ini penting?

* **Fokus Yang Lebih Baik**: Di dalam petunjuk, kami pada dasarnya bercerita. Setiap bagian dibangun dan diasumsikan dari tiap bagian sebelumnya. Di dalam buku petunjuk, setiap instruksi bisa dan harus berdiri sendiri. Ini berarti instruksi dapat fokus pada satu aspek spesifik Vue, daripada harus memberikan gambaran umum. 

* **Lebih Mendalam**: Untuk menghindari membuat panduan lebih panjang, kami mencoba untuk hanya menyertakan contoh paling sederhana untuk membantu anda memahami setiap fitur. Lalu kami melanjutkan. Di dalam buku petunjuk, kami bisa menyertakan lebih banyak contoh kompleks, menggabungkan fitur dengan cara yang menarik. Setiap instuksi juga bisa menjadi panjang dan rinci sesuai yang dibutuhkan, demi mendalami topik secara menyeluruh.

* **Mengajarkan JavaScript**: Di dalam petunjuk, kami berasumsi bahwa setidaknya sudah terbiasa dengan ES5 JavaScript. Sebagai contoh, kami tidak akan menjelaskan bagaimana `Array.prototype.filter` bekerja pada properti _computed_ yang menyaring sebuah daftar.

* **Menjelajahi Ekosistem**: Untuk fitur tingkat lanjut, kami mengasumsikan beberapa pengetahuan ekosistem. Sebagai contoh, jika anda ingin menggunakan komponen berkas tunggal (single-file components) di Webpack, kami tidak menjelaskan bagaimana cara untuk mengkonfigurasi selain bagian dari Vue pada konfigurasi Webpack. Di dalam buku petunjuk, kami memiliki ruang untuk menjelajahi pustaka ekosistem lebih dalam - setidaknya selama berguna secara universal bagi pengembang Vue.

## Kontribusi Buku Petunjuk

### Apa yang kami cari

Buku petunjuk memberikan contoh kepada pengembang untuk mengatasi kasus penggunaan umum atau menarik, dan juga menjelaskan detail yang lebih kompleks. Tujuan kami adalah untuk melampaui contoh pengantar sederhana, dan menunjukkan konsep yang lebih luas yang bisa diterapkan, dan beberapa peringatan untuk pendekatan.

Jika anda tertarik untuk berkontribusi, silakan mulai kolaborasi dengan cara mengisi sebuah isu dibawah tanda **ide buku petunjuk** dengan konsep anda sehingga kami bisa memandu anda untuk melakukan _pull request_ dengan sukses. Setelah ide anda disetujui, silakan ikuti templat dibawah ini sebanyak mungkin. Beberapa bagian diperlukan, dan lainnya adalah opsional. Mengikuti urutan penomoran sangat disarankan, tetapi tidak diperlukan. 

Recipes should generally:
Instruksi umumnya harus:

> * Menyelesaikan masalah umum, secara spesifik
> * Mulai dengan contoh yang paling sederhana
> * Mengenalkan kompleksitas satu per satu
> * Menautkan ke dokumentasi lain, daripada menjelaskan kembali konsep 
> * Mendeskripsikan masalah, daripada mengasumsikan keakraban
> * Menjelaskan prosesnya, daripada hanya memberikan hasil akhir
> * Menjelaskan kelebihan dan kekurangan dari strategi anda, termasuk ketika tepat atau tidak tepat
> * Menyebutkan solusi alternativ, jika relevan, tetapi berikan penjelajahan mendalam untuk instruksi terpisah

Kami memohon anda untuk mengikuti templat dibawah ini. Kami mengerti, namun, bahwa ada saat ketika anda perlu menyimpang demi kejelasan maupun aliran. Bagaimanapun juga, semua instruksi pada suatu titik tertentu harus mendiskusikan nuansa pilihan yang dibuat menggunakan pola ini, lebih disukai dalam bentuk bagian pola alternatif.

### Base Example

_required_

1.  Articulate the problem in a sentence or two.
2.  Explain the simplest possible solution in a sentence or two.
3.  Show a small code sample.
4.  Explain what this accomplishes in a sentence.

### Details about the Value

_required_

1.  Address common questions that one might have while looking at the example. (Blockquotes are great for this)
2.  Show examples of common missteps and how they can be avoided.
3.  Show very simple code samples of good and bad patterns.
4.  Discuss why this may be a compelling pattern. Links for reference are not required but encouraged.

### Real-World Example

_required_

Demonstrate the code that would power a common or interesting use case, either by:

1.  Walking through a few terse examples of setup, or
2.  Embedding a codepen/jsfiddle example

If you choose to do the latter, you should still talk through what it is and does.

### Additional Context

_optional_

It's extremely helpful to write a bit about this pattern, where else it would apply, why it works well, and run through a bit of code as you do so or give people further reading materials here.

### When To Avoid This Pattern

_optional_

This section is not required, but heavily recommended. It won't make sense to write it for something very simple such as toggling classes based on state change, but for more advanced patterns like mixins it's vital. The answer to most questions about development is ["It depends!"](https://codepen.io/rachsmith/pen/YweZbG), this section embraces that. Here, we'll take an honest look at when the pattern is useful and when it should be avoided, or when something else makes more sense.

### Alternative Patterns

_required_

This section is required when you've provided the section above about avoidance. It's important to explore other methods so that people told that something is an antipattern in certain situations are not left wondering. In doing so, consider that the web is a big tent and that many people have different codebase structures and are solving different goals. Is the app large or small? Are they integrating Vue into an existing project, or are they building from scratch? Are their users only trying to achieve one goal or many? Is there a lot of asynchronous data? All of these concerns will impact alternative implementations. A good cookbook recipe gives developers this context.

## Thank you

It takes time to contribute to documentation, and if you spend the time to submit a PR to this section of our docs, you do so with our gratitude.
