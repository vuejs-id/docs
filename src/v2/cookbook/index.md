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

## Cookbook Contributions

### What we're looking for

The Cookbook gives developers examples to work off of that both cover common or interesting use cases, and also progressively explain more complex detail. Our goal is to move beyond a simple introductory example, and demonstrate concepts that are more widely applicable, as well as some caveats to the approach.

If you're interested in contributing, please initiate collaboration by filing an issue under the tag **cookbook idea** with your concept so that we can help guide you to a successful pull request. After your idea has been approved, please follow the template below as much as possible. Some sections are required, and some are optional. Following the numerical order is strongly suggested, but not required.

Recipes should generally:

> * Solve a specific, common problem
> * Start with the simplest possible example
> * Introduce complexities one at a time
> * Link to other docs, rather than re-explaining concepts
> * Describe the problem, rather than assuming familiarity
> * Explain the process, rather than just the end result
> * Explain the pros and cons of your strategy, including when it is and isn't appropriate
> * Mention alternative solutions, if relevant, but leave in-depth explorations to a separate recipe

We request that you follow the template below. We understand, however, that there are times when you may necessarily need to deviate for clarity or flow. Either way, all recipes should at some point discuss the nuance of the choice made using this pattern, preferably in the form of the alternative patterns section.

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
