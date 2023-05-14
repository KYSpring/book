# Typescript学习笔记

## TS内置类型: HTMLElement

typescript 对于Bom / Dom 的接口都进行了内置的类型化定义，以HTMLElement为例，其定义文件为lib.dom.d.ts。

这里摘录了下其ts内置定义【TODO-出一个详细说明】

```typescript
/** Any HTML element. Some elements directly implement this interface, while others implement it via an interface that inherits it. */
interface HTMLElement extends Element, DocumentAndElementEventHandlers, ElementCSSInlineStyle, ElementContentEditable, GlobalEventHandlers, HTMLOrSVGElement {
    accessKey: string;
    readonly accessKeyLabel: string;
    autocapitalize: string;
    dir: string;
    draggable: boolean;
    hidden: boolean;
    innerText: string;
    lang: string;
    readonly offsetHeight: number;
    readonly offsetLeft: number;
    readonly offsetParent: Element | null;
    readonly offsetTop: number;
    readonly offsetWidth: number;
    outerText: string;
    spellcheck: boolean;
    title: string;
    translate: boolean;
    attachInternals(): ElementInternals;
    click(): void;
    addEventListener<K extends keyof HTMLElementEventMap>(type: K, listener: (this: HTMLElement, ev: HTMLElementEventMap[K]) => any, options?: boolean | AddEventListenerOptions): void;
    addEventListener(type: string, listener: EventListenerOrEventListenerObject, options?: boolean | AddEventListenerOptions): void;
    removeEventListener<K extends keyof HTMLElementEventMap>(type: K, listener: (this: HTMLElement, ev: HTMLElementEventMap[K]) => any, options?: boolean | EventListenerOptions): void;
    removeEventListener(type: string, listener: EventListenerOrEventListenerObject, options?: boolean | EventListenerOptions): void;
}

```

由于HtmlElement类型的典型性，ts官方提供了一篇指南专门讨论Dom的类型化([文档地址](https://www.typescriptlang.org/docs/handbook/dom-manipulation.html))。

这篇文章主要罗列了Dom type的一些常用接口的定义，其中不乏一些有趣的内容，比如：

- children和childNodes的差异是什么？
  
  两者的区别在于childNodes能够包含的节点类型更多，而children仅能包含HTMl部分标签类型。通过如下代码可见一斑：

    ```html
    <div>
    <p>Hello, World</p>
    TypeScript!
    </div>;
    ```
    ```javascript
    const div = document.getElementsByTagName("div")[0];
    div.children;
    // HTMLCollection(1) [p]
    div.childNodes;
    // NodeList(2) [p, text]
    ```
- querySelectorAll的返回值为什么使用了一个新的类型```NodeListOf<E>```而不是直接使用```E[]```?
    ```typescript
    querySelectorAll<E extends Element = Element>(selectors: string): NodeListOf<E>;
    ```
    原因在于ts在处理dom类型系统时需要更精确的描述dom每个api的接口，而简单的返回一个```E[]```过于宽泛了，事实上```NodeListOf<E>```仅仅实现了部分属性：
    > NodeListOf only implements the following properties and methods: length , item(index), forEach((value, key, parent) => void) , and numeric indexing. 

<br/>
此外，文章中也明确指出，ts对于DOM的类型化处理是完全基于MDN中对于DOM的借口描述而实现的：

> The best part about the lib.dom.d.ts type definitions is that they are reflective of the types annotated in the Mozilla Developer Network (MDN) documentation site.


