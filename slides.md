---
# You can also start simply with 'default'
theme: apple-basic
layout: intro-image
image: '/images/cover-intro.png'
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Microfrontends in Angular with Native Federation
info: |
  ## Microfrontends in Angular with Native Federation
# apply unocss classes to the current slide
# class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# open graph
seoMeta:
  # By default, Slidev will use ./og-image.png if it exists,
  # or generate one from the first slide if not found.
  ogImage: auto
  # ogImage: https://cover.sli.dev
---

# Microfrontends in Angular with Native Federation

<div class="absolute bottom-10">
  <span class="font-700">
    Valerio Como
  </span>
</div>

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/valeriocomo/slides-micro-frontends-in-angular-with-native-federation" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
src: ./pages/00-about-me/about-me.md
transition: slide-left
---

---
src: ./pages/01-agenda/agenda.md
transition: slide-left
---

---
src: ./pages/02-what-mf-are/what-mf-are.md
transition: slide-left
---

---
src: ./pages/03-mf-in-angular/mf-in-angular.md
transition: slide-left
--- 

---
src: ./pages/04-native-federation/native-federation.md
transition: slide-left
---

---
src: ./pages/05-advanced-use-cases/advanced-use-cases.md
transition: slide-left
---

---
src: ./pages/06-wrapup/wrapup.md
transition: slide-left
---
