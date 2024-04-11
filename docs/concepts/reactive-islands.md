
---
title: Reactive islands / Nue docs
---

# Reactive islands
Reactive Island is an interactive UI component on an otherwise static HTML page.

[bunny-video]
  videoId: 6e00cbdf-e36e-43a2-af0a-f95e0430ddad
  caption: Feedback component in action
  width: 650

Nue makes a perfect environment to author, create, update, and manage reactive islands. It's trivial to add a new UI library and Nue automatically takes care of dependency management, mounting, and hot-reloading.


## Defining islands
Islands are defined in a file with a `.nue` extension. Nue-files can contain one or multiple components. You author them with the same [HTML-based syntax](../reference/template-syntax.html), as you create your [layout components](../concepts/layout-components.html). For example, here is the source code for the feedback component in the intro-video of this page:


```
<!-- file: feedback.nue -->

<dialog @name="feedback-dialog">
  <a class="close" @click="root.close()">&times;</a>

  <div :if="thanks" class="thanks">
    <h2>Thank you!</h2>
  </div>

  <form @submit.prevent="submit" :else>
    <h2>Give us feedback</h2>

    <div>
      <h3>Your name</h3>
      <input type="text" name="name" placeholder="Example: John Doe" required>
    </div>

    <div>
      <h3>Your email</h3>
      <input type="email" name="email" placeholder="your@email.com" required>
    </div>

    <div>
      <h3>Your thoughts</h3>
      <textarea name="feedback" placeholder="Type here..."/>
    </div>

    <button>Submit</button>

  </form>

  <script>
    submit({ target }) {
      this.thanks = true
    }
  </script>

</dialog>
```

Your components can either be global or specific to an application. Global components must be defined inside a [global directory](files-and-directories.html#deps) and application-specific components are defined inside the app directory.

You can also have page-specific components by defining them inside a page directory such as `/blog/my-blog-entry/my-entry-component.nue`.


## Adding islands to a page
You can mount islands on your `layout.html` files: in the header, footer, or anywhere on the main layout. For example, the feedback component is mounted on the footer:


```
<footer>
  <a href="/">© { fullname }</a>
  <strong>{ slogan }</strong>

  <!-- feedback component -->
  <feedback-dialog id="feedback"/>

  <!-- a launcher element to open the dialog -->
  <img src="/img/feedback.svg" onclick="feedback.showModal()">
</footer>
```

## Auto-mounting
After the component is defined and placed somewhere on your layout, Nue takes care of the rest: mounting, unmounting, and hot-reloading. Nue even remembers the original state after the component was reloaded: the form values, whether the dialog was opened, and in the case of a single-page application the state is completely restored from the URL.

You can add as many islands as you want and it has a minor impact on loading performance because all the islands are lazily mounted after the HTML and CSS are fully loaded and rendered. This is called "progressive enhancement" (or "hydration" in post-2019 terminology).

NOTE: the key to [performance optimization](performance-optimization.html) is more about the first stages of page loading (HTML and CSS) and less about JavaScript and reactive islands.


## Single-page applications
A single-page application is essentially one, big reactive island. You develop them in [almost the same way](../tutorials/build-a-simple-spa.html) as you'd develop a simpler island. And you can enjoy the development boost of hot-reloading even if you have multiple/nested views and the UI is connected with the backend data over the network:

[bunny-video]
  videoId: 7c291fcd-b344-4ff9-bd3f-5c7301707c5d
  caption: "Single-page application — one big reactive island"
  poster: thumbnail_0e853239.jpg
