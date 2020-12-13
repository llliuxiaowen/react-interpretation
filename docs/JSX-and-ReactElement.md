# JSX 和 React.createElement 源码分析

> 分析过程参考 [React.createElement源码分析 - 掘金](https://juejin.cn/post/6844903998537859079)

JSX 是 ``React.createElement`` 的语法糖，在 babel 中，一段 JSX 被转为如下代码：
```js
// JSX
const container = <div className="container">hello, world</div>;
// Babel 编译
var container = /*#__PURE__*/ React.createElement(
  "div",
  {
    className: "container"
  },
  "hello, world"
);
```
> tips： **为什么使用 JSX 的时候看上去没用到 React，但还是需要引用 React？**
> 因为 JSX 是 ``React.createElement`` 的语法糖，实际用的是 ``React.createElement``

参考[源码](../packages/react/src/ReactElement.js)，``React.createElement()`` 做的事情有：
1. 创建 props 对象用来存放属性，包含 config 中通过校验的值，默认属性，children 节点；
2. 提取 config 中的 ref 和 key；
3. 返回一个 ``ReactElement``。

``ReactElement`` 唯一做的事情是给对象添加了一个 ``$$typeof`` 属性。
```js
const ReactElement = function(type, key, ref, self, source, owner, props) {
  const element = {
    // This tag allows us to uniquely identify this as a React Element
    $$typeof: REACT_ELEMENT_TYPE,

    // Built-in properties that belong on the element
    type: type,
    key: key,
    ref: ref,
    props: props,
    // Record the component responsible for creating this element.
    _owner: owner,
  };
  return element;
};
```

看一下 ``$$typeof`` 的值 ``REACT_ELEMENT_TYPE`` 是什么：
```js
export let REACT_ELEMENT_TYPE = 0xeac7; // 如果浏览器不支持 Symbol 的话
if (typeof Symbol === 'function' && Symbol.for) {
  REACT_ELEMENT_TYPE = symbolFor('react.element');
}
```

但是加 ``$$typeof`` 有什么用呢？根据 [Why Do React Elements Have a $$typeof Property?](https://overreacted.io/why-do-react-elements-have-typeof-property/) 来看，React 0.14 中用 Symbol 标记每个 React 元素，React 会检查 ``element.$$typeof``，如果该值缺失或无效的话 React 会拒绝处理。使用 Symbol 是因为 Symbol 在如 iframes 和 workers 的环境间是全局的，如果浏览器不支持 Symbol 则使用默认值 ``0xeac7``（因为它看起来像 React 的写法）。

```js
// 可能造成 XSS 攻击
// Server could have a hole that lets user store JSON
let expectedTextButGotJSON = {
  type: 'div',
  props: {
    dangerouslySetInnerHTML: {
      __html: '/* put your exploit here */'
    },
  },
  // ...
};
let message = { text: expectedTextButGotJSON };

// Dangerous in React 0.13
<p>
  {message.text}
</p>
```