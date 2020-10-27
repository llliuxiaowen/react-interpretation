目录：
[TOC]

# 在开始之前
需要了解：
- [如何阅读大型前端开源项目的源码 - zxc0328](https://zxc0328.github.io/2018/05/01/react-source-reading-howto/)
- [源码概览 - react](https://reactjs.bootcss.com/docs/codebase-overview.html)：包含源码架构和约定

vscode 中需要做的准备：
1. 安装扩展 `Flow Language Support`，这样就有类型提示了
2. vscode 如果报错 `'import type' declarations can only be used in TypeScript files.`，需要设置： `"javascript.validate.enable": false"`，以禁用内置检查
> ref: [js 'types' can only be used in a .ts file - Visual Studio Code using @ts-check - stackoverflow](https://stackoverflow.com/questions/48859169/js-types-can-only-be-used-in-a-ts-file-visual-studio-code-using-ts-check)

# 如何 debug
根据 react 官方[贡献流程](https://zh-hans.reactjs.org/docs/how-to-contribute.html)可知，
> 想测试你做出的更改的话，最简单的方法就是运行 `yarn build react/index,react-dom/index --type=UMD`，之后再打开 `fixtures/packaging/babel-standalone/dev.html`，该文件已使用 `build` 文件夹内的 `react.development.js` 来搞定你的更改。

TIPS:
- 在源码中改过 debugger 位置后需要重新 build；
- 在 `fixtures/packaging/babel-standalone/dev.html` 中可以直接使用 React、ReactDOM 中的各方法。 :tada:

# 重要概念
## Fiber
它的主要目标是：
- 能够把可中断的任务切片处理。
- 能够调整优先级，重置并复用任务。
- 能够在父元素与子元素之间交错处理，以支持 React 中的布局。
- 能够在 `render()` 中返回多个元素。
- 更好地支持错误边界。

参考：[React Fiber Architecture - react](https://github.com/acdlite/react-fiber-architecture) in English

## expirationTime v.s. lane
参考：[如何看待React源码中调度优先级使用lane取代expirationTime？- 知乎](https://www.zhihu.com/question/405268183/answer/1328519761)

# 参考资料
- [React 源码解析 - jokcy](https://react.jokcy.me/)
