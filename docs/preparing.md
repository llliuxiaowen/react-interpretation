[TOC]

# 关于版本
fork 的各版本是：
```
react-dom: 17.0.0-alpha.0
```

# 如何 debug
根据 react 官方[贡献流程](https://zh-hans.reactjs.org/docs/how-to-contribute.html)可知，
> 想测试你做出的更改的话，最简单的方法就是运行 `yarn build react/index,react-dom/index --type=UMD`，之后再打开 `fixtures/packaging/babel-standalone/dev.html`，该文件已使用 `build` 文件夹内的 `react.development.js` 来搞定你的更改。

改过 debugger 位置后重新 build 即可。

# vscode 中的准备工作
1. 安装扩展 `Flow Language Support`，这样就有类型提示了
2. vscode 如果报错 `'import type' declarations can only be used in TypeScript files.`，需要设置： `"javascript.validate.enable": false"`，以禁用掉内置检查
> ref: [js 'types' can only be used in a .ts file - Visual Studio Code using @ts-check - stackoverflow](https://stackoverflow.com/questions/48859169/js-types-can-only-be-used-in-a-ts-file-visual-studio-code-using-ts-check)

# 参考资料
- [如何阅读大型前端开源项目的源码 - zxc0328](https://zxc0328.github.io/2018/05/01/react-source-reading-howto/)
- [React 源码解析 - jokcy](https://react.jokcy.me/)

# 第一遍读觉得很困难，如何应对？
我的做法是：1）硬看，先看大概流程，不必细读；2）边看边画流程图。

# React 17 updates
## expirationTime v.s. lane
它们之间的区别参考：[如何看待React源码中调度优先级使用lane取代expirationTime？- 知乎](https://www.zhihu.com/question/405268183/answer/1328519761)