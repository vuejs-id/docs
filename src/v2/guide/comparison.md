---
title: Perbandingan dengan Kerangka Kerja yang Lain
type: guide
order: 801
---

Ini pastinya halaman yang paling sulit untuk ditulis di dokumentasi ini, tetapi kita rasa hal ini sangatlah penting untuk dibahas. Kemungkinan yang ada adalah, Anda mencoba untuk menyelesaikan suatu masalah menggunakan pustaka yang lain. Anda berada di halaman ini karena Anda ingin tahu lebih jauh apakah dengan Vue bisa menyelesaikan masalah yang lebih spesifik dengan baik. Itulah yang kita harapkan untuk menjawab pertanyaan Anda.

Kita juga berusaha dengan keras untuk menghindari bias. Sebagai tim inti, sudah jelas kita sangat mencintai Vue. Ada beberapa permasalahan yang bisa diselesaikan oleh Vue, daripada menggunakan pustaka lain di luar sana. Jika kita tidak percaya dengan hal tersebut, maka kita tidak akan bekerja untuk membangun Vue. Kita ingin berlaku adil dan seakurat mungkin. Dimana pada kerangka kerja yang lain menawarkan kelebihan/keuntungan yang lebih, contohnya seperti React yang memiliki ekosistem yang sangat luas sebagai alternatif pe-*render* antarmuka atau mengembalikan dukungan peramban untuk IE6, kita akan mencoba untuk menaruh hal-hal seperti ini kedalam daftar dengan sebaik mungkin.

Kita juga menerima dengan terbuka bantuan dari Anda untuk membuat dokumen ini selalu *up-to-date* dikarenakan dunia JavaScript bergerak sangat cepat! Jika Anda menyadari/menemukan beberapa hal yang kurang tepat atau ada sesuatu yang salah, mohon untuk menginformasikan kepada kami dengan cara [membuat *issue*](https://github.com/vuejs/vuejs.org/issues/new?title=Inaccuracy+in+comparisons+guide).

## React

React dan Vue mempunyai banyak kesamaan, mereka berdua sama-sama:

- Menggunakan dan memanfaatkan DOM virtual.
- Reaktif dan tampilan yang mudah disusun melalui komponen.
- Lebih fokus kepada inti dari pustaka, seperti lebih memperhatikan sistem *routing* dan pengelolaan *state* secara global yang ditangani oleh beberapa pustaka dari rekan kami sendiri.

Menjadi sesuatu yang sama didalam bidang yang sama pula, kita telah menggunakan banyak waktu untuk membuat perbandingan ini dengan sangat baik dibandingkan dengan yang lainnya. Kita ingin memastikan bahwa tidak hanya ketelitian di bidang teknikal, tetapi juga keseimbangan. Kita menemukan suatu sisi dimana React mampu mengalahkan Vue, sebagai contoh di kekayaan ekosistem dan *renderer* kustom yang melimpah milik mereka.

Dengan demikian, terjadi perbandingan berat sebelah yang tak bisa terhindarkan lagi bagi Vue untuk beberapa pengguna React, dikarenakan banyak subyek yang telah dieksplor tidak bersifat subyektif. Kami mengakui dan menyadari hal ini terjadi karena banyaknya variasi dari sisi teknikal, dan perbandingan ini dibuat dengan tujuan untuk membuat garis besar alasan mengapa Vue lebih berpotensi lebih baik dalam membangun aplikasi berdasarkan dengan preferensi kita.

Some of the sections below may also be slightly outdated due to recent updates in React 16+, and we are planning to work with the React community to revamp this section in the near future.
Beberapa bagian dibawah ini mungkin ada sedikit yang tidak sesuai karena pembaharuan terbaru di React 16+, dan kita berencana untuk berkolaborasi dengan komunitas React untuk merubah bagian ini kedepannya.

### Kinerja/Performa

Perfoma React dan Vue sama-sama luar biasa cepat, jadi kecepatan adalah salah satu faktor penting mengapa kita harus pilih satu diantara keduanya. Untuk metrik yang lebih spesifik, silakan cek berikut ini [*Benchmark* pihak ketika](https://stefankrause.net/js-frameworks-benchmark8/table.html), yang mana lebih berfokus pada kinerja/performa *render*/*update* dengan menggunakan *component trees* yang sederhana.

#### Upaya dalam Mengoptimalkan

Di React, ketika *state* pada komponen berubah, dia memicu untuk me-*render* semua *sub-tree* pada komponen, dimulai dari komponen itu sendiri sebagai awalannya. Untuk menghindari perenderan ulang yang tidak penting pada komponen anak, Anda wajib untuk menggunakan `PureComponent` atau menggunakan `shouldComponentUpdate` dikondisi apapun. Anda mungkin juga perlu untuk menggunakan struktur *immutable data* untuk membuat *state* Anda lebih mudah untuk dioptimalkan. Bagaimanapun, di dalam kasus-kasus tertentu Anda mungkin tidak bisa mengandalkan beberapa optimasi dikarenakan `PureComponent/shoudComponentUpdate` berasumsi bahwa di semua hasil pe-*render*-an pada *sub-tree* ditentukan oleh *props* yang ada di komponen saat ini. Jika permasalahan yang terjadi bukan karena itu, maka beberapa optimasi mungkin malah akan menuntun kita pada *state* DOM yang tidak konsisten.

Di Vue, ketergantungan sebuah komponen secara otomatis sudah terekam ketika proses *render* berlangsung, jadi sistem pada Vue secara tepat bisa mengetahui komponen mana yang benar-benar perlu untuk di *render* ulang ketika *state* berubah. Masing-masing komponen bisa dipertimbangkan untuk bisa mempunyai *method* `shouldComponenUpdate` yang secara otomatis sudah diimplementasikan, tanpa peringatan pada komponen yang bersarang.

Secara keseluruhan, hal ini menghilangkan kebutuhan pengoptimalan kinerja di seluruh kelas dari sudut pandang sang pengembang, dan karenanya para pengembang tinggal fokus untuk membangun aplikasi itu sendiri.

### HTML & CSS

Di React, segalanya berbentuk JavaScript. Tidak hanya struktur HTML saja yang berbentuk JSX, kecenderungan untuk melalukan manajemen CSS juga dilakukan didalam JavaScript. Hal ini punya keuntungan tersendiri, tetapi hal ini juga menimbulkan ketidaknyamanan yang bervariasi bagi beberapa pengembang.

Vue merangkul teknologi web yang klasik dan dibangun diatasnya. Untuk menunjukkan bagaimana maksudnya, kita akan berikan beberapa contohnya.

#### JSX vs Templates

Di React, semua komponen mengekspresikan fungsi pe-*render*-an UI nya menggunakan JSX. JSX merupakan sintaks deklaratif yang menyerupai XML di JavaScript.

Fungsi *render* di JSX mempunyai beberapa kelebihan:

- Anda bisa menggunakan kemampuan bahasa JavaScript sepenuhnya untuk membangun antarmuka web Anda. Termasuk juga variable sementara, dan menggunakan referensi yang ada pada JavaScript.

- Alat pendukung (contohnya *linting*, pemeriksaan tipe data, dan *autocompletion* pada editor) di beberapa pustaka yang tersedia untuk JSX lebih bagus daripada untuk templat Vue.

Di Vue, kita juga mempunyai [fungsi *render*](render-function.html) dan bahkan juga [mendukung JSX](render-function.html#JSX), karena terkadang Anda memerlukan fitur tersebut. Bagaimanapun, kita tetap menawarkan fungsi pe-*render*-an dalam bentuk templat sebagai alternatif yang lebih mudah.

- Bagi mayoritas pengembang yang telah bekerja menggunakan HTML, templat akan terasa lebih natural untuk dibaca dan ditulis. Preferensi itu sendiri bisa jadi bersifat subyektif, tetapi jika hal tersebut bisa membuat para pengembang menjadi lebih produktif, maka keuntungan tersebut bersifat obyektif.

- Templat yang berbasis HTML membuat kita mudah untuk migrasi secara progresif dari aplikasi yang sudah ada untuk mendapatkan kelebihan dari fitur reaktifitasnya Vue.

- Hal tersebut juga sangat mempermudah para desainer dan pengembang pemula untuk berkontribusi kedalam *codebase*.

- Anda bahkan bisa menggunakan *pre-processors* seperti Pug (dulunya ini disebut dengan nama Jade) untuk membangun templat Vue Anda.

Beberapa ada juga yang berkata bahwa Anda harus belajar DSL (*Domain-Specific Language*) secara ekstra agar bisa membuat templat - kita percaya bahwa ini argumen yang tidak beralasan. Pertama, menggunakan JSX bukan berarti pengembang tidak butuh untuk belajar hal lain - JSX hanyalah sintaks tambahan yang digunakan pada JavaScript, jadi hal tersebut bisa sangat mudah untuk dipelajari bagi seseorang yang sudah mengenal/familiar menggunakan JavaScript. Selain itu, templat hanyalah sintaks tambahan yang dibangun diatas HTML, oleh karena itu membuat templat jauh lebih mudah untuk dipelajari bagi orang yang sudah familiar dengan HTML. Dengan adanya DSL, kita juga bisa membantu para pengembang untuk menyelesaikan tugas dengan kode yang lebih sedikit (contohnya menggunakan `v-on` *modifiers*). Dengan tugas yang sama, dengan menggunakan JSX atau fungsi *render* malah bisa membutuhkan lebih banyak kode.

Di tingkat yang lebih tinggi lagi, kita bisa membagi komponen dalam dua kategori: bentuk *presentational* dan *logical*. Kami menyarankan untuk menggunakan templat untuk komponen yang berbentuk *presentational* dan fungsi pe-*render*-an / JSX untuk komponen yang berbentuk *logical*. Persentase pemilihan bentuk komponen ini tergantung dengan tipe aplikasi yang sedang Anda bangun, tetapi pada umumnya kita menjumpai komponen yang berbentuk *presentational* lebih banyak dipakai.

#### Component-Scoped CSS

Jika Anda memecah-mecah komponen menjadi beberapa berkas (sebagai contoh nya menggunakan [Modul CSS](https://github.com/gajus/react-css-modules)), menyematkan CSS di React bisa dilakukan melalui `CSS-in-JS` (contohnya [styled-components](https://github.com/styled-components/styled-components), [glamorous](https://github.com/paypal/glamorous), dan [emotion](https://github.com/emotion-js/emotion)). Beberapa dokumentasi tersebut memperkenalkan kita tentang paradigma *styling* yang berbasis komponen yang berbeda dengan gaya pemrosesan CSS pada umumnya. Selain itu, meskipun sudah mendukung untuk peng-ekstraksi-an CSS menjadi satu berkas CSS ketika proses *build*, ia tetap harus dimasukkan ke dalam *bundle* agar *styling* bisa bekerja dengan baik. Ketika Anda mempunyai hak untuk mendinamiskan JavaScript ketika membangun *style* Anda, sebagai gantinya adalah meningkatnya ukuran *bundle* dan biaya *runtime*.

Jika Anda adalah penggemar dari `CSS-in-JS`, sudah banyak pustaka `CSS-in-JS` populer yang mendukung Vue (contohnya [styled-components-vue](https://github.com/styled-components/vue-styled-components) dan [vue-emotion](https://github.com/egoist/vue-emotion)). Perbedaan utama antara React dan Vue disini adalah metode dasar pada proses *styling* di Vue lebih familiar dengan `style` tag di [single-file components](single-file-components.html).

[Single-file components](single-file-components.html) memberikan Anda akses penuh ke CSS didalam berkas yang sama di komponen Anda.

``` html
<style scoped>
  @media (min-width: 250px) {
    .list-container:hover {
      background: orange;
    }
  }
</style>
```

Atribut `scoped` yang bersifat opsional secara otomatis menggabungkan CSS kedalam komponen Anda (hanya di komponen ini saja) dengan cara menambahkan atribut yang unik (seperti `data-v-21e5b78`) dan di-*compile* dari `.list-container:hover` menjadi seperti `.list-container[data-v-21e5b78]:hover`.

Yang terakhir, *styling* pada *single-file component* milik Vue sangat fleksibel. Melalui [vue-loader](https://github.com/vuejs/vue-loader), Anda bisa menggunakan berbagai *pre-processor*, *post-processor*, dan bahkan bisa terintegrasi dengan [Modul CSS](https://vue-loader.vuejs.org/en/features/css-modules.html) -- semua nya hanya ada di dalam elemen `<style>`.

### Skala

#### Meningkatkan Besaran Skala

Untuk aplikasi yang besar, Vue dan React menawarkan solusi *routing* yang kuat. Komunitas React juga sangat inovatif dalam menentukan solusi untuk pengelolaan *state* (contohnya Flux/Redux). Pola dari Pengelolaan *state* ini dan [bahkan Redux itu sendiri](https://yarnpkg.com/en/packages?q=redux%20vue&p=1) bisa dengan mudah di integrasikan kedalam aplikasi Vue. Faktanya, Vue sudah menerapkan model ini selangkah lebih maju dengan menggunakan [Vuex](https://github.com/vuejs/vuex), solusi pengelolaan *state* yang terinspirasi dari Elm, yang kami harap bisa meningkatkan pengalaman disisi pengembang.

Informasi penting lainnya selain penawaran ini adalah pustaka Vue untuk pengelolaan *state* dan *routing* ([info lainnya](https://github.com/vuejs)) semuanya didukung secara resmi dan selalu terjaga pembaharuannya dengan inti pustaka. Sebaliknya, React lebih memilih untuk menyerahkan urusan ini kepada komunitas, dengan cara lebih banyak membangun kepingan ekosistem . Dampaknya, React menjadi lebih populer yang mengakibatkan ekosistem React menjadi sangat kaya daripada ekosistem milik Vue.

Yang terakhir, Vue menawarkan [*Generator* proyek CLI](https://github.com/vuejs/vue-cli) yang mampu membuat proyek awal dengan mudah menggunakan *build system* sesuai dengan pilihan Anda, beserta [webpack](https://github.com/vuejs-templates/webpack), [Browserify](https://github.com/vuejs-templates/browserify), atau bahkan [tanpa *build system*](https://github.com/vuejs-templates/simple). React juga berhasil membuat hal yang sama dengan menggunakan [create-react-app](https://github.com/facebookincubator/create-react-app), tetapi saat ini masih memiliki keterbatasan sebagai berikut:

- Dia tidak memperbolehkan konfigurasi apapun ketika proses *generate*, sedangkan proyek templat pada Vue memperbolehkannya, seperti kustomisasi milik [Yeoman](http://yeoman.io/)
- It only offers a single template that assumes you're building a single-page application, while Vue offers a wide variety of templates for various purposes and build systems.
- It cannot generate projects from user-built templates, which can be especially useful for enterprise environments with pre-established conventions.

It's important to note that many of these limitations are intentional design decisions made by the create-react-app team and they do have their advantages. For example, as long as your project's needs are very simple and you never need to "eject" to customize your build process, you'll be able to update it as a dependency. You can read more about the [differing philosophy here](https://github.com/facebookincubator/create-react-app#philosophy).

#### Scaling Down

React is renowned for its steep learning curve. Before you can really get started, you need to know about JSX and probably ES2015+, since many examples use React's class syntax. You also have to learn about build systems, because although you could technically use Babel Standalone to live-compile your code in the browser, it's absolutely not suitable for production.

While Vue scales up just as well as React, it also scales down just as well as jQuery. That's right - to get started, all you have to do is drop a single script tag into the page:

``` html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

Then you can start writing Vue code and even ship the minified version to production without feeling guilty or having to worry about performance problems.

Since you don't need to know about JSX, ES2015, or build systems to get started with Vue, it also typically takes developers less than a day reading [the guide](./) to learn enough to build non-trivial applications.

### Native Rendering

React Native enables you to write native-rendered apps for iOS and Android using the same React component model. This is great in that as a developer, you can apply your knowledge of a framework across multiple platforms. On this front, Vue has an official collaboration with [Weex](https://weex.apache.org/), a cross-platform UI framework created by Alibaba Group and being incubated by the Apache Software Foundation (ASF). Weex allows you to use the same Vue component syntax to author components that can not only be rendered in the browser, but also natively on iOS and Android!

At this moment, Weex is still in active development and is not as mature and battle-tested as React Native, but its development is driven by the production needs of the largest e-commerce business in the world, and the Vue team will also actively collaborate with the Weex team to ensure a smooth experience for Vue developers.

Another option is [NativeScript-Vue](https://nativescript-vue.org/), a [NativeScript](https://www.nativescript.org/) plugin for building truly native applications using Vue.js.

### With MobX

MobX has become quite popular in the React community and it actually uses a nearly identical reactivity system to Vue. To a limited extent, the React + MobX workflow can be thought of as a more verbose Vue, so if you're using that combination and are enjoying it, jumping into Vue is probably the next logical step.

### Preact and Other React-Like Libraries

React-like libraries usually try to share as much of their API and ecosystem with React as is feasible. For that reason, the vast majority of comparisons above will also apply to them. The main difference will typically be a reduced ecosystem, often significantly, compared to React. Since these libraries cannot be 100% compatible with everything in the React ecosystem, some tooling and companion libraries may not be usable. Or, even if they appear to work, they could break at any time unless your specific React-like library is officially supported on par with React.

## AngularJS (Angular 1)

Some of Vue's syntax will look very similar to AngularJS (e.g. `v-if` vs `ng-if`). This is because there were a lot of things that AngularJS got right and these were an inspiration for Vue very early in its development. There are also many pains that come with AngularJS however, where Vue has attempted to offer a significant improvement.

### Complexity

Vue is much simpler than AngularJS, both in terms of API and design. Learning enough to build non-trivial applications typically takes less than a day, which is not true for AngularJS.

### Flexibility and Modularity

AngularJS has strong opinions about how your applications should be structured, while Vue is a more flexible, modular solution. While this makes Vue more adaptable to a wide variety of projects, we also recognize that sometimes it's useful to have some decisions made for you, so that you can just start coding.

That's why we offer a [webpack template](https://github.com/vuejs-templates/webpack) that can set you up within minutes, while also granting you access to advanced features such as hot module reloading, linting, CSS extraction, and much more.

### Data binding

AngularJS uses two-way binding between scopes, while Vue enforces a one-way data flow between components. This makes the flow of data easier to reason about in non-trivial applications.

### Directives vs Components

Vue has a clearer separation between directives and components. Directives are meant to encapsulate DOM manipulations only, while components are self-contained units that have their own view and data logic. In AngularJS, directives do everything and components are just a specific kind of directive.

### Runtime Performance

Vue has better performance and is much, much easier to optimize because it doesn't use dirty checking. AngularJS becomes slow when there are a lot of watchers, because every time anything in the scope changes, all these watchers need to be re-evaluated again. Also, the digest cycle may have to run multiple times to "stabilize" if some watcher triggers another update. AngularJS users often have to resort to esoteric techniques to get around the digest cycle, and in some situations, there's no way to optimize a scope with many watchers.

Vue doesn't suffer from this at all because it uses a transparent dependency-tracking observation system with async queueing - all changes trigger independently unless they have explicit dependency relationships.

Interestingly, there are quite a few similarities in how Angular and Vue are addressing these AngularJS issues.

## Angular (Formerly known as Angular 2)

We have a separate section for the new Angular because it really is a completely different framework from AngularJS. For example, it features a first-class component system, many implementation details have been completely rewritten, and the API has also changed quite drastically.

### TypeScript

Angular essentially requires using TypeScript, given that almost all its documentation and learning resources are TypeScript-based. TypeScript has its benefits - static type checking can be very useful for large-scale applications, and can be a big productivity boost for developers with backgrounds in Java and C#.

However, not everyone wants to use TypeScript. In many smaller-scale use cases, introducing a type system may result in more overhead than productivity gain. In those cases you'd be better off going with Vue instead, since using Angular without TypeScript can be challenging.

Finally, although not as deeply integrated with TypeScript as Angular is, Vue also offers [official typings](https://github.com/vuejs/vue/tree/dev/types) and [official decorator](https://github.com/vuejs/vue-class-component) for those who wish to use TypeScript with Vue. We are also actively collaborating with the TypeScript and VSCode teams at Microsoft to improve the TS/IDE experience for Vue + TS users.

### Runtime Performance

Both frameworks are exceptionally fast, with very similar metrics on benchmarks. You can [browse specific metrics](https://stefankrause.net/js-frameworks-benchmark8/table.html) for a more granular comparison, but speed is unlikely to be a deciding factor.

### Size

Recent versions of Angular, with [AOT compilation](https://en.wikipedia.org/wiki/Ahead-of-time_compilation) and [tree-shaking](https://en.wikipedia.org/wiki/Tree_shaking), have been able to get its size down considerably. However, a full-featured Vue 2 project with Vuex + Vue Router included (~30KB gzipped) is still significantly lighter than an out-of-the-box, AOT-compiled application generated by `angular-cli` (~65KB gzipped).

### Flexibility

Vue is much less opinionated than Angular, offering official support for a variety of build systems, with no restrictions on how you structure your application. Many developers enjoy this freedom, while some prefer having only one Right Way to build any application.

### Learning Curve

To get started with Vue, all you need is familiarity with HTML and ES5 JavaScript (i.e. plain JavaScript). With these basic skills, you can start building non-trivial applications within less than a day of reading [the guide](./).

Angular's learning curve is much steeper. The API surface of the framework is huge and as a user you will need to familiarize yourself with a lot more concepts before getting productive. The complexity of Angular is largely due to its design goal of targeting only large, complex applications - but that does make the framework a lot more difficult for less-experienced developers to pick up.

## Ember

Ember is a full-featured framework that is designed to be highly opinionated. It provides a lot of established conventions and once you are familiar enough with them, it can make you very productive. However, it also means the learning curve is high and flexibility suffers. It's a trade-off when you try to pick between an opinionated framework and a library with a loosely coupled set of tools that work together. The latter gives you more freedom but also requires you to make more architectural decisions.

That said, it would probably make a better comparison between Vue core and Ember's [templating](https://guides.emberjs.com/v2.10.0/templates/handlebars-basics/) and [object model](https://guides.emberjs.com/v2.10.0/object-model/) layers:

- Vue provides unobtrusive reactivity on plain JavaScript objects and fully automatic computed properties. In Ember, you need to wrap everything in Ember Objects and manually declare dependencies for computed properties.

- Vue's template syntax harnesses the full power of JavaScript expressions, while Handlebars' expression and helper syntax is intentionally quite limited in comparison.

- Performance-wise, Vue outperforms Ember [by a fair margin](https://stefankrause.net/js-frameworks-benchmark8/table.html), even after the latest Glimmer engine update in Ember 3.x. Vue automatically batches updates, while in Ember you need to manually manage run loops in performance-critical situations.

## Knockout

Knockout was a pioneer in the MVVM and dependency tracking spaces and its reactivity system is very similar to Vue's. Its [browser support](http://knockoutjs.com/documentation/browser-support.html) is also very impressive considering everything it does, with support back to IE6! Vue on the other hand only supports IE9+.

Over time though, Knockout development has slowed and it's begun to show its age a little. For example, its component system lacks a full set of lifecycle hooks and although it's a very common use case, the interface for passing children to a component feels a little clunky compared to [Vue's](components.html#Content-Distribution-with-Slots).

There also seem to be philosophical differences in the API design which if you're curious, can be demonstrated by how each handles the creation of a [simple todo list](https://gist.github.com/chrisvfritz/9e5f2d6826af00fcbace7be8f6dccb89). It's definitely somewhat subjective, but many consider Vue's API to be less complex and better structured.

## Polymer

Polymer is another Google-sponsored project and in fact was a source of inspiration for Vue as well. Vue's components can be loosely compared to Polymer's custom elements and both provide a very similar development style. The biggest difference is that Polymer is built upon the latest Web Components features and requires non-trivial polyfills to work (with degraded performance) in browsers that don't support those features natively. In contrast, Vue works without any dependencies or polyfills down to IE9.

In Polymer, the team has also made its data-binding system very limited in order to compensate for the performance. For example, the only expressions supported in Polymer templates are boolean negation and single method calls. Its computed property implementation is also not very flexible.

## Riot

Riot 3.0 provides a similar component-based development model (which is called a "tag" in Riot), with a minimal and beautifully designed API. Riot and Vue probably share a lot in design philosophies. However, despite being a bit heavier than Riot, Vue does offer some significant advantages:

- Better performance. Riot [traverses a DOM tree](http://riotjs.com/compare/#virtual-dom-vs-expressions-binding) rather than using a virtual DOM, so suffers from the same performance issues as AngularJS.
- More mature tooling support. Vue provides official support for [webpack](https://github.com/vuejs/vue-loader) and [Browserify](https://github.com/vuejs/vueify), while Riot relies on community support for build system integration.
