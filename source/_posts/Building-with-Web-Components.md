---
title: Building with Web Components
date: 2020-03-15 21:41:00
tags:
- front-end
- lwc
- polymer
- w3c
- javascript
- web components
categories:
- trans
---



{% blockquote Jamie White https://blog.heroku.com/building-with-web-components Building with Web Components -- MARCH 04, 2020%}
本文翻译自 Jamie White
{% endblockquote %}

<!-- more -->

---

> In the early years of web development, there were three standard fundamentals upon which every website was built: HTML, CSS, and JavaScript. As time passed, web developers became more proficient in their construction of fancy UI/UX widgets for websites. With the need for newer ways of crafting a site coming in conflict with the relatively slow adoption of newer standards, more and more developers began to build their own libraries to abstract away some of the technical details. The web ceased being a standard: now your website could be a React site, or an Angular site, or a Vue site, or any number of other web framework that are not interoperable with each other.

在早几年前开发 web ，所有的 web 站点都是基于三个标准的基本原理来搭建：HTML、CSS 和 JavaScript，随着时间推移，web 开发者越来越精通于为网站开发一些炫酷的 UI/UX 组件，新的标准采纳相对较慢，这与用新方式制作网站的诉求出现了冲突，越来越多的开发者开始开发他们自己的库去抽象一些技术细节，web 开发不再是一套标准：现在你的网站的框架可以是 React 、Vue ，或其他任何 **不能互相兼容交互（ interoperable，相关问题可扩展阅读：[Web Components: Seamlessly interoperable](https://medium.com/@sergicontre/web-components-seamlessly-interoperable-82efd6989ca4) ）** 的 web 框架。

>Web components seek to tilt the balance of web development back towards a standard agreed upon by browser vendors and developers. Various polyfills and proprietary frameworks have achieved what web components are now trying to standardize: composable units of JavaScript and HTML that can be imported and reused across web applications. Let's explore the history of web components and the advantages they provide over third-party libraries.

针对 web 开发，web components 试图将重点放到浏览器供应商和web开发者共同制定的标准上来。其实现在的各种 polyfill 和 **专有框架（ proprietary，相关问题可扩展阅读：[专有软件](https://baike.baidu.com/item/%E4%B8%93%E6%9C%89%E8%BD%AF%E4%BB%B6/5395294?fr=aladdin) ）** 已经做到了目前 web component 努力去标准化的东西：通过 JavaScript 和 HTML 组合出来的基础单元，能够被跨应用的引入和复用。让我们研究下 web component 的历史和它相比于三方库，能提供的优势是什么。

## web components 的概念是怎么开始的

>After some attempts by browser vendors to create a standard—and subsequent slow progress—front-end developers realized it was up to them to create a browser-agnostic library delivering on the promise of the web components vision. When React was released, it completely changed the paradigm of web development in two key ways. First, with a bit of JavaScript and some XML-like syntax, React allowed you to compose custom HTML tags it called components:

浏览器供应商们尝试建立一套标准，在许多次的尝试之后 - 同时伴随着缓慢的进展 - 前端开发者们意识到，想要实现 web components 的愿景，其实还是要靠他们自己去创建一个与浏览器完全无关的库。当 React 被发布时，它在两个关键点上完全的改变了 web 开发的模式。首先，React 允许你构造名叫 components 的自定义 HTML 标签，只需要使用几行 JavaScript 和一些类似 XML 的语法：

```javascript
class HelloMessage extends React.Component {
  render() {
    return (
    <h1>
        Hello <span class="name">{this.props.name}</span>
    </h1>
    );
  }
}

ReactDOM.render(
  <HelloMessage name="Johnny" />,
  document.getElementById('hello-example-container')
);
```

>This trivial example shows how you can encapsulate logic to create React components which can be reused across your app and shared with other developers.

React components 能够跨应用复用，同时共享给其他的开发者们，这个小例子向你展示了如何去封装逻辑从而创建 React components。

> Second, React popularized the concept of a virtual DOM. The DOM is your entire HTML document, all the HTML tags that a browser slurps up to render a website. However, the relationship between HTML tags, JavaScript, and CSS which make up a website is rather fragile. Making changes to one component could inadvertently affect other aspects of the site. One of the benefits of the virtual DOM was to make sure that UI updates only redrew specific chunks of HTML through JavaScript events. Thus, developers could easily build websites rendering massive amounts of changing data without necessarily worrying about the performance implications.

其次，React 使[虚拟 DOM](https://reactkungfu.com/2015/10/the-difference-between-virtual-dom-and-dom/)的概念得到普及。DOM 是指整个 HTML document，是浏览器用来渲染一个网站的所有 HTML 标签。然而，用于制造网站的 HTML 标签，JavaScript 和 CSS ，它们之间的联系相当的脆弱，修改一个组件可能无意间影响到网站的其他地方。虚拟 DOM 的其中一个好处就是确保 UI 的各种更新只会通过 JavaScript 事件重绘 HTML 的特定部分。这样一来，开发者们在开发一些正在做大量数据变更渲染的网站时就能够非常的轻松，不需要去担心对于网站其他地方的影响（ performance 翻译成性能感觉不契合上下文，这里应该是指整个网站的表现 ）。

>Around 2015, Google began developing the Polymer Project as a means of demonstrating how they wanted web standards to evolve through polyfills. Over the years and various releases, the ideas presented by Polymer library began to be incorporated by the W3C for standardization and browser adoption. The work started back in 2012 by the W3C (and originally introduced by Alex Russell at Fronteers Conference 2011) began to get more attention, undergoing various design changes to address developers' concerns.

在2015年，谷歌开始开发 [the Polymer Project](https://www.polymer-project.org/) ，以这个项目展示他们是如何通过 polyfill 来使得 web 标准得以发展。在过去几年的各种发布中，Polymer 库带来的许多想法已经开始被 W3C 纳入到标准化中，同时也被各浏览器所采纳。 web component 起始于 2012 年，由 [W3C 发起](https://www.w3.org/TR/2012/WD-components-intro-20120522/)（ 最初是由 Alex Russell 在 [2011 前端开发者大会](https://www.w3.org/TR/2012/WD-components-intro-20120522/) 提出 ），已经获得了越来越多的关注，同时也经历了各种设计改版以解决开发者们的问题。

## web components 的工具集

>Let's take a look at the web standards which make up web components today.

让我看看构成目前 web components 的 web 标准。

### 自定义元素特性

>Custom elements allows you to create custom HTML tags which can exhibit any JavaScript behavior:

[自定义元素特性](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_custom_elements) 允许你创建自定义的 HTML 标签，这个标签可以表现出任何 JavaScript 的行为：

```javascript
class SayHello extends HTMLElement {
  constructor() {
    super();

    let p = document.createElement(“p”);
    let text = document.createTextNode(“Hello world!”);
    p.appendChild(text);

    this.appendChild(p);
  }
}

customElements.define('say-hello', SayHello);
```

>Custom elements can be used to encapsulate logic across your site and reused wherever necessary. Since they're a web standard, you won't need to load an additional JavaScript framework to support them.

自定义元素可以用来封装整个网站的逻辑，并且被复用到任何需要的地方。自从它们成为了 web 标准之后，你已经不需要加载额外的 JavaScript 框架来支持这个特性。

### HTML 模板特性

>If you need to reuse markup on a website, it can be helpful to make use of an HTML template. HTML templates are ignored by the browser until they are called upon to be rendered. Thus, you can create complicated blocks of HTML and render them instantaneously via JavaScript.

如果你需要在一个网站反复的使用一些标签片段，利用 [HTML 模板特性](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_templates_and_slots) 会帮助你做到这一点，HTML 模板会被浏览器忽视，直到它们被调用到，即将被渲染的时候。这样一来，你就可以开发一些复杂的 HTML 片段，并且通过 JavaScript 代码调用来瞬间渲染它们。

>To create an HTML template, all you need to do is wrap up your HTML with the new `<template>` tag:

创建一个 HTML 模板，你只需要通过 `<template>` 标签来包裹你的 HTML 代码片段即可：

```javascript
<template id="template">
  <script>
    const button = document.getElementById('click-button');
    button.addEventListener('click', event => alert(event));
  </script>
  <style>
    #click-button {
    border: 0;
    border-radius: 4px;
    color: white;
    font-size: 1.5rem;
    padding: .5rem 1rem;
    }
  </style>
  <button id="click-button">Click Me!</button>
</template>
```

### 影子 DOM 特性

>The shadow DOM is another concept which provides support for further web page encapsulation. Any elements within the shadow DOM are not affected by the CSS styles of any other markup on the page, and similarly, any CSS defined within the shadow DOM doesn't affect other elements. They can also be configured to not be affected by external JavaScript, either. Among other advantages, this results in lower memory usage for the browser and faster render times. If it's helpful, you can think of elements in the shadow DOM as more secure iframes.

[影子 DOM 特性](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM) 是另一个提供能力支持去进一步封装 web 页面的概念。影子 DOM 里面的任何元素都不会受到页面中其他标签的 CSS 样式影响，并且同样的，任何在影子 DOM 中定义的 CSS 也不会影响外面的其他元素。它们也能通过被配置，从而不被外部 JavaScript 所影响。至于其他的好处就是，它会降低浏览器的内存使用率，同时提升渲染速度。如果换一种理解有帮助的话，你可以把影子 DOM 里面的元素想成是更加安全的 `iframe`。

>To add an element to the shadow DOM, you call attachShadow() on it:

往影子 DOM 里面添加元素，你需要在它上面调用 `attachShadow()` ：

```javascript
class MyWebComponent extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: "open" });
    }
    connectedCallback() {
        this.shadowRoot.innerHTML = `
            <p>I'm in the Shadow Root!</p>
        `;
    }
}

window.customElements.define("my-web-component", MyWebComponent);
```

>This creates a custom element, <my-web-component>, whose p tag would not be affected by any other styles on the page.

这里创建了一个自定义元素， `<my-web-component>` ，它的 `p` 标签将不会被页面中的其他任何样式所影响。

## Web component 生态

>The greatest advantage web components have over using a library is their ability to provide standards-compliant, composable HTML elements. What this means is that if you have built a web component, you can package it up as a release for other developers to consume as a dependency in their project, just like any other Node or Ruby package, and those developers can be assured that that web component will work across all (well, most) web browsers without requiring the browser to load a front-end framework like React, Angular, or Vue.

与使用一个库相比，web component 的最大优点是它们有能力提供符合标准化的，可被自由组合的 HTML 元素。这意味着如果你已经开发了一个 web component，你可以把它打包成发行版，给其他的开发者在他们项目中作为一个依赖来使用，就像其他那些 Node 或 Ruby 包一样，同时那些开发者们能够得到保证，在不通过浏览器加载一些前端框架，类似 React ， Angular ，或者 Vue的情况下，那些 web component 将跨越所有（几乎大部分） web 浏览器正常工作。

>To give an example, Shader Doodle is a custom element which sets up the ability to easily create fragment shaders. Developers who need this functionality can just fetch the package and insert it as a <shader-doodle> tag in their HTML, rather than creating the functionality of Share Doodle from scratch.

这里给一个例子， [Shader Doodle](https://github.com/halvves/shader-doodle) 是一个自定义元素，能够轻松的创建片段着色器。需要这个功能的开发者只需要 fetch 这个包，并将这个包通过一个 `<shader-doodle>` 标签插入到他们的 HTML 中即可，不再需要从头开始开发 Share Doodle 的功能。

>Now, with the great interoperability that web components give you, many frameworks and libraries like Vue or React have started to provide the option to generate web components out of their proprietary code. That way you don't have to learn all the low-level APIs of the aforementioned standards, and can instead focus on coding. There many other libraries for creating web components, like Polymer , X-Tag , slim.js , Riot.js , and Stencil.

由于 web component 给你的强大的互相兼容性，现在许多的框架和库，像 Vue 或 React 已经开始提供 option ，通过它们自己的专有代码来生成 web components。通过这类生成的方式，你就没必要去学习前面提到的那些标准所提供的低级 API，从而专注于开发编码。这里有许多其他的库来生成 web component ，像 [Polymer](https://github.com/polymer) ，[X-Tag](https://github.com/x-tag/core) , [slim.js](https://github.com/slimjs/slim.js) , [Riot.js](https://github.com/riot/riot) , 和 [Stencil](https://github.com/ionic-team/stencil) 。

>Another great example of this are Salesforce’s Lightning Web Components, a lightweight framework that abstracts away the complexity of the different web standards. It provides a standards-compliant foundation for building web components which can be used in any project.

针对于 web component ，另外一个非常好的例子是 Salesforce 的 [Lightning Web Components](https://lwc.dev/) ， 一个轻量级框架，对不同 web 标准的复杂性进行了抽象。它提供了一套符合 web 标准的基础能力，能够通过它去开发 web component，从而使用到任何项目中。

## 获得更多相关的 web component

>We recorded an episode of Code[ish], our podcast on all things tech, that meticulously went through the history (and future!) of web components. Be sure to check out that interview from someone who literally wrote the book on web components.

我们录制了一段 Code[ish] 片段，我们的博客里面有所有的技术相关事情，它细致入微（ 这个词感觉比较符合语境 ）的经历了 web component 的历史（ 和未来！ ）。一定要听听那次对于 [Ben Farrell （ 我直接找了这个人的名字 ） 的访谈](https://www.heroku.com/podcasts/codeish/38-building-with-web-components)，他真的写了一本 [关于 web component 的书](https://www.manning.com/books/web-components-in-action)。

>You can also join the Polymer Slack workspace to chat with other web developers about working with these standards.

你也可以加入 [Polymer Stack 工作空间](https://polymer.slack.com/messages/general/) ，与其他开发者们聊聊通过这些标准开发的体验。

---

## 总结

web component 于 2011 年被 Alex Russell 提出，2012 年 W3C 开始正式发起 [草案](https://www.w3.org/TR/2012/WD-components-intro-20120522/)，2014年正式纳入 [标准](https://www.w3.org/TR/components-intro/)，后逐渐被浏览器所支持，其中谷歌 2015 年开始的 Polymer Project 项目，通过 polyfill 来临时支持浏览器兼容，起了很大的推进作用。 React 在 2013 年 3 月推出，也算应运而生。

web component 解决的是一个跨应用、跨浏览器、跨框架复用的问题，也是文中多次提到的 interoperable ，这是一个比较常见的问题，比如 React 组件想用到 Vue 框架的应用里面。web component 从根本上解决了问题，也就是推进标准化，让浏览器去支持，从而达到脱库的地步。

web component 通过三种方式的 API 来实现组件的封装：HTML Templates、Custom elements，和 Shadow DOM。 HTML 模板负责封装ui片段、Custom elements 耦合 JavaScript，而 Shadow DOM 用来保持 UI 的独立性，不至于污染全局样式，以及被影响。

目前的主流框架（ React、Vue 等 ）开始支持生成 Web Component，同时也有一些基于 web component 的库和框架诞生。

{% img [class names] /uploads/web-component/customElements-2020-03-15.png  '"customElements compatibility till 2020-03-15" "customElements compatibility till 2020-03-15"' %}

目前（ 2020-03-15 ） web component 的兼容性方面，除 IE 外，其他主流浏览器已全部兼容。

## 单词

- interoperable
    - ADJ 能共同操作的，能共同使用的
    - able to exchange and use information.
- paradigm
    - N-COUNT 典范；范例
    - A paradigm is a clear and typical example of something. 
- trivial
    - ADJ-GRADED 琐碎的;不重要的;微不足道的
    - (informal terms) small and of little importance.
- encapsulate
    - 封装（计算机术语）
- slurp
    - VERB 大声地啜;出声地喝
    - If you slurp a liquid, you drink it noisily
- fragile
    - ADJ-GRADED 脆弱的;不确定的
    - If you describe a situation as fragile, you mean that it is weak or uncertain, and unlikely to be able to resist strong pressure or attack.
- aspect
    - N-COUNT 方面;形态;特性
    - An aspect of something is one of the parts of its character or nature.
- chunk
    - N-COUNT 大量;大部分
    - A chunk of something is a large amount or large part of it.
- massive
    - ADJ-GRADED 巨大的;庞大的;厚重的;强大的
    - Something that is massive is very large in size, quantity, or extent.
- implications
    - N-COUNT 可能引发的后果
    - The implications of something are the things that are likely to happen as a result.
- performance
    - N-VAR 表现；业绩；性能；运行情况
    - Someone's or something's performance is how successful they are or how well they do something.
- demonstrating
    - VERB 证明;论证;表明;说明
    - To demonstrate a fact means to make it clear to people.
- evolve
    - V-ERG （使）逐步发展;（使）演化
    - If something evolves or you evolve it, it gradually develops over a period of time into something different and usually more advanced.
- incorporate
    - VERB 把…合并;使并入
    - If someone or something is incorporated into a large group, system, or area, they become a part of it.
- present
    - VERB 引起；导致；带来；提供
    - If something presents a difficulty, challenge, or opportunity, it causes it or provides it.
- address
    - VERB 对付；处理；设法了解并解决
    - If you address a problem or task or if you address yourself to it, you try to understand it or deal with it.
- exhibit
    - VERB 表现;显示
    - If someone or something shows a particular quality, feeling, or type of behaviour, you can say that they exhibit it.
- instantaneously
    - ADJ-GRADED 瞬间的;即刻的
    - Something that is instantaneous happens immediately and very quickly.
- resul
    - VERB 导致;引起;造成
    - If something results in a particular situation or event, it causes that situation or event to happen.
- scratch
    - PHRASE 从零开始;从头做起;白手起家
    - If you do something from scratch, you do it without making use of anything that has been done before.
- foundation
    - N-COUNT 基础;根基
    - The foundation of something such as a belief or way of life is the things on which it is based.
- meticulously
    - ADJ-GRADED 小心谨慎的;一丝不苟的;注意细节的
    - If you describe someone as meticulous, you mean that they do things very carefully and with great attention to detail.
- interview
    - N-COUNT 采访;访谈
    - An interview is a conversation in which a journalist puts questions to someone such as a famous person or politician.