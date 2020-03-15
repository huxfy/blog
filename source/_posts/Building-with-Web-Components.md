---
title: Building with Web Components
date: 2020-03-14 21:58:32
tags:
- front-end
categories:
- trans
---



{% blockquote Jamie White https://blog.heroku.com/building-with-web-components Building with Web Components -- MARCH 04, 2020%}
本文翻译自 Jamie White 
{% endblockquote %}

<!-- more -->

---

> In the early years of web development, there were three standard fundamentals upon which every website was built: HTML, CSS, and JavaScript. As time passed, web developers became more proficient in their construction of fancy UI/UX widgets for websites. With the need for newer ways of crafting a site coming in conflict with the relatively slow adoption of newer standards, more and more developers began to build their own libraries to abstract away some of the technical details. The web ceased being a standard: now your website could be a React site, or an Angular site, or a Vue site, or any number of other web framework that are not interoperable with each other.

对于开发web前端，在更早些年所有的 web 站点都是基于三个标准的基本原理来搭建：HTML、CSS 和 JavaScript，随着时间推移，web 开发者越来越精通于为网站开发一些炫酷的 UI/UX 组件，新的标准采纳相对较慢，其与用新方式制作网站的诉求出现了冲突，越来越多的开发者开始开发他们自己的库去抽象一些技术细节，web 开发不再是一套标准：现在你的网站可以是 React 网站、Angular 网站、Vue 网站，或其他任何 **不能互相兼容交互（ interoperable，相关问题可扩展阅读：[Web Components: Seamlessly interoperable](https://medium.com/@sergicontre/web-components-seamlessly-interoperable-82efd6989ca4) ）** 的 web 框架。

>Web components seek to tilt the balance of web development back towards a standard agreed upon by browser vendors and developers. Various polyfills and proprietary frameworks have achieved what web components are now trying to standardize: composable units of JavaScript and HTML that can be imported and reused across web applications. Let's explore the history of web components and the advantages they provide over third-party libraries.

针对 web 开发，web components 试图将重点放到浏览器供应商和web开发者共同制定的标准上来。其实现在的各种 polyfill 和 **专有框架（ proprietary，相关问题可扩展阅读：[专有软件](https://baike.baidu.com/item/%E4%B8%93%E6%9C%89%E8%BD%AF%E4%BB%B6/5395294?fr=aladdin) ）** 已经做到了目前 web component 努力去标准化的东西：通过 JavaScript 和 HTML 组合出来的基础单元，能够被跨应用的引入和复用。让我们研究下 web component 的历史和它相比于三方库，能提供的优势是什么。

---

## 总结




