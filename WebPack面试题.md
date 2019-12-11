# 工程化

[toc]



## webpack面试题

webpack是事实上的前端打包标准,相关的面试题也是面试的热点.

### 1. webpack与grunt、gulp的不同？

Grunt、Gulp是基于任务运行的工具：

它们会自动执行指定的任务，就像流水线，把资源放上去然后通过不同插件进行加工，它们包含活跃的社区，丰富的插件，能方便的打造各种工作流。

Webpack是基于模块化打包的工具:

自动化处理模块,webpack把一切当成模块，当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

因此这是完全不同的两类工具,而现在主流的方式是用npm script代替Grunt、Gulp,npm script同样可以打造任务流.

### 2. webpack、rollup、parcel优劣？

- webpack适用于大型复杂的前端站点构建: webpack有强大的loader和插件生态,打包后的文件实际上就是一个立即执行函数，这个立即执行函数接收一个参数，这个参数是模块对象，键为各个模块的路径，值为模块内容。立即执行函数内部则处理模块之间的引用，执行模块等,这种情况更适合文件依赖复杂的应用开发.
- rollup适用于基础库的打包，如vue、d3等: Rollup 就是将各个模块打包进一个文件中，并且通过 Tree-shaking 来删除无用的代码,可以最大程度上降低代码体积,但是rollup没有webpack如此多的的如代码分割、按需加载等高级功能，其更聚焦于库的打包，因此更适合库的开发.
- parcel适用于简单的实验性项目: 他可以满足低门槛的快速看到效果,但是生态差、报错信息不够全面都是他的硬伤，除了一些玩具项目或者实验项目不建议使用

![2019-08-03-02-53-46](https://xiaomuzhu-image.oss-cn-beijing.aliyuncs.com/14f023a939e7a89330629f4cffb70cfd.png)

![2019-08-03-02-53-34](https://xiaomuzhu-image.oss-cn-beijing.aliyuncs.com/e790108aaba2c7c7a2e7393de102b1b5.png)

### 3. 有哪些常见的Loader？

- file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件
- url-loader：和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去
- source-map-loader：加载额外的 Source Map 文件，以方便断点调试
- image-loader：加载并且压缩图片文件
- babel-loader：把 ES6 转换成 ES5
- css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
- style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。
- eslint-loader：通过 ESLint 检查 JavaScript 代码

### 4. 有哪些常见的Plugin？

- define-plugin：定义环境变量
- html-webpack-plugin：简化html文件创建
- uglifyjs-webpack-plugin：通过`UglifyES`压缩`ES6`代码
- webpack-parallel-uglify-plugin: 多核压缩,提高压缩速度
- webpack-bundle-analyzer: 可视化webpack输出文件的体积
- mini-css-extract-plugin: CSS提取到单独的文件中,支持按需加载

### 5. 分别介绍bundle，chunk，module是什么？

- bundle：是由webpack打包出来的文件
- chunk：代码块，一个chunk由多个模块组合而成，用于代码的合并和分割
- module：是开发中的单个模块，在webpack的世界，一切皆模块，一个模块对应一个文件，webpack会从配置的entry中递归开始找出所有依赖的模块

### 6. Loader和Plugin的不同？

**不同的作用:**

- **Loader**直译为"加载器"。Webpack将一切文件视为模块，但是webpack原生是只能解析js文件，如果想将其他文件也打包的话，就会用到`loader`。 所以Loader的作用是让webpack拥有了加载和解析_非JavaScript文件_的能力。
- **Plugin**直译为"插件"。Plugin可以扩展webpack的功能，让webpack具有更多的灵活性。 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

**不同的用法:**

- **Loader**在`module.rules`中配置，也就是说他作为模块的解析规则而存在。 类型为数组，每一项都是一个`Object`，里面描述了对于什么类型的文件（`test`），使用什么加载(`loader`)和使用的参数（`options`）
- **Plugin**在`plugins`中单独配置。 类型为数组，每一项是一个`plugin`的实例，参数都通过构造函数传入

### 7. webpack的构建流程是什么?

Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：

1. 初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；
2. 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；
3. 确定入口：根据配置中的 entry 找出所有的入口文件；
4. 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；
5. 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；
6. 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；
7. 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。

在以上过程中，Webpack 会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，并且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行结果。

> 来源于[深入浅出webpack第五章](https://wangchong.tech/webpack/5原理/5-1工作原理概括.html)

> 拓展阅读[细说 webpack 之流程篇](https://fed.taobao.org/blog/2016/09/10/webpack-flow/)

### 8. 是否写过Loader和Plugin？描述一下编写loader或plugin的思路？

Loader像一个"翻译官"把读到的源文件内容转义成新的文件内容，并且每个Loader通过链式操作，将源文件一步步翻译成想要的样子。

编写Loader时要遵循单一原则，每个Loader只做一种"转义"工作。 每个Loader的拿到的是源文件内容（`source`），可以通过返回值的方式将处理后的内容输出，也可以调用`this.callback()`方法，将内容返回给webpack。 还可以通过 `this.async()`生成一个`callback`函数，再用这个callback将处理后的内容输出出去。 此外`webpack`还为开发者准备了开发loader的工具函数集——`loader-utils`。

相对于Loader而言，Plugin的编写就灵活了许多。 webpack在运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

### 9. webpack的热更新是如何做到的？说明其原理？

webpack的热更新又称热替换（Hot Module Replacement），缩写为HMR。 这个机制可以做到不用刷新浏览器而将新变更的模块替换掉旧的模块。

**原理：**

![2019-08-03-15-45-12](https://xiaomuzhu-image.oss-cn-beijing.aliyuncs.com/c0863ad3d922fceccfc8290e39bb2474.png)

首先要知道server端和client端都做了处理工作

1. 第一步，在 webpack 的 watch 模式下，文件系统中某一个文件发生修改，webpack 监听到文件变化，根据配置文件对模块重新编译打包，并将打包后的代码通过简单的 JavaScript 对象保存在内存中。
2. 第二步是 webpack-dev-server 和 webpack 之间的接口交互，而在这一步，主要是 dev-server 的中间件 webpack-dev-middleware 和 webpack 之间的交互，webpack-dev-middleware 调用 webpack 暴露的 API对代码变化进行监控，并且告诉 webpack，将代码打包到内存中。
3. 第三步是 webpack-dev-server 对文件变化的一个监控，这一步不同于第一步，并不是监控代码变化重新打包。当我们在配置文件中配置了devServer.watchContentBase 为 true 的时候，Server 会监听这些配置文件夹中静态文件的变化，变化后会通知浏览器端对应用进行 live reload。注意，这儿是浏览器刷新，和 HMR 是两个概念。
4. 第四步也是 webpack-dev-server 代码的工作，该步骤主要是通过 sockjs（webpack-dev-server 的依赖）在浏览器端和服务端之间建立一个 websocket 长连接，将 webpack 编译打包的各个阶段的状态信息告知浏览器端，同时也包括第三步中 Server 监听静态文件变化的信息。浏览器端根据这些 socket 消息进行不同的操作。当然服务端传递的最主要信息还是新模块的 hash 值，后面的步骤根据这一 hash 值来进行模块热替换。
5. webpack-dev-server/client 端并不能够请求更新的代码，也不会执行热更模块操作，而把这些工作又交回给了 webpack，webpack/hot/dev-server 的工作就是根据 webpack-dev-server/client 传给它的信息以及 dev-server 的配置决定是刷新浏览器呢还是进行模块热更新。当然如果仅仅是刷新浏览器，也就没有后面那些步骤了。
6. HotModuleReplacement.runtime 是客户端 HMR 的中枢，它接收到上一步传递给他的新模块的 hash 值，它通过 JsonpMainTemplate.runtime 向 server 端发送 Ajax 请求，服务端返回一个 json，该 json 包含了所有要更新的模块的 hash 值，获取到更新列表后，该模块再次通过 jsonp 请求，获取到最新的模块代码。这就是上图中 7、8、9 步骤。
7. 而第 10 步是决定 HMR 成功与否的关键步骤，在该步骤中，HotModulePlugin 将会对新旧模块进行对比，决定是否更新模块，在决定更新模块后，检查模块之间的依赖关系，更新模块的同时更新模块间的依赖引用。
8. 最后一步，当 HMR 失败后，回退到 live reload 操作，也就是进行浏览器刷新来获取最新打包代码。

> 详细原理解析来源于知乎饿了么前端[Webpack HMR 原理解析](https://zhuanlan.zhihu.com/p/30669007)

### 10. 如何用webpack来优化前端性能？

用webpack优化前端性能是指优化webpack的输出结果，让打包的最终结果在浏览器运行快速高效。

- 压缩代码:删除多余的代码、注释、简化代码的写法等等方式。可以利用webpack的`UglifyJsPlugin`和`ParallelUglifyPlugin`来压缩JS文件， 利用`cssnano`（css-loader?minimize）来压缩css
- 
- 利用CDN加速: 在构建过程中，将引用的静态资源路径修改为CDN上对应的路径。可以利用webpack对于`output`参数和各loader的`publicPath`参数来修改资源路径
- Tree Shaking: 将代码中永远不会走到的片段删除掉。可以通过在启动webpack时追加参数`--optimize-minimize`来实现
- Code Splitting: 将代码按路由维度或者组件分块(chunk),这样做到按需加载,同时可以充分利用浏览器缓存
- 提取公共第三方库:  SplitChunksPlugin插件来进行公共模块抽取,利用浏览器缓存可以长期缓存这些无需频繁变动的公共代码

> 详解可以参照[前端性能优化-加载](https://a.lmongo.com/offer/load.html)

### 11. 如何提高webpack的打包速度?

- happypack: 利用进程并行编译loader,利用缓存来使得 rebuild 更快,遗憾的是作者表示已经不会继续开发此项目,类似的替代者是[thread-loader](https://github.com/webpack-contrib/thread-loader)
- [外部扩展(externals)](https://webpack.docschina.org/configuration/externals/): 将不怎么需要更新的第三方库脱离webpack打包，不被打入bundle中，从而减少打包时间,比如jQuery用script标签引入
- dll: 采用webpack的 DllPlugin 和 DllReferencePlugin 引入dll，让一些基本不会改动的代码先打包成静态资源,避免反复编译浪费时间
- 利用缓存: `webpack.cache`、babel-loader.cacheDirectory、`HappyPack.cache`都可以利用缓存提高rebuild效率
- 缩小文件搜索范围: 比如babel-loader插件,如果你的文件仅存在于src中,那么可以`include: path.resolve(__dirname, 'src')`,当然绝大多数情况下这种操作的提升有限,除非不小心build了node_modules文件

> 实战文章推荐[使用webpack4提升180%编译速度 Tool](https://louiszhai.github.io/2019/01/04/webpack4/)

### 12. 如何提高webpack的构建速度？

1. 多入口情况下，使用`CommonsChunkPlugin`来提取公共代码
2. 通过`externals`配置来提取常用库
3. 利用`DllPlugin`和`DllReferencePlugin`预编译资源模块 通过`DllPlugin`来对那些我们引用但是绝对不会修改的npm包来进行预编译，再通过`DllReferencePlugin`将预编译的模块加载进来。
4. 使用`Happypack` 实现多线程加速编译
5. 使用`webpack-uglify-parallel`来提升`uglifyPlugin`的压缩速度。 原理上`webpack-uglify-parallel`采用了多核并行压缩来提升压缩速度
6. 使用`Tree-shaking`和`Scope Hoisting`来剔除多余代码

### 13. 怎么配置单页应用？怎么配置多页应用？

单页应用可以理解为webpack的标准模式，直接在`entry`中指定单页应用的入口即可，这里不再赘述

多页应用的话，可以使用webpack的 `AutoWebPlugin`来完成简单自动化的构建，但是前提是项目的目录结构必须遵守他预设的规范。 多页应用中要注意的是：

- 每个页面都有公共的代码，可以将这些代码抽离出来，避免重复的加载。比如，每个页面都引用了同一套css样式表
- 随着业务的不断扩展，页面可能会不断的追加，所以一定要让入口的配置足够灵活，避免每次添加新页面还需要修改构建配置

## 前端工程化面试题

### 1. Babel的原理是什么?

babel 的转译过程也分为三个阶段，这三步具体是：

- 解析 Parse: 将代码解析生成抽象语法树( 即AST )，即词法分析与语法分析的过程
- 转换 Transform: 对于 AST 进行变换一系列的操作，babel 接受得到 AST 并通过 babel-traverse 对其进行遍历，在此过程中进行添加、更新及移除等操作
- 生成 Generate: 将变换后的 AST 再转换为 JS 代码, 使用到的模块是 babel-generator

![2019-08-03-23-32-34](https://xiaomuzhu-image.oss-cn-beijing.aliyuncs.com/566a1ac865cfc0b1bf5511f377ff6828.png)

### 2. 你的git工作流是怎样的?

GitFlow 是由 Vincent Driessen 提出的一个 git操作流程标准。包含如下几个关键分支：

| 名称    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| master  | 主分支                                                       |
| develop | 主开发分支，包含确定即将发布的代码                           |
| feature | 新功能分支，一般一个新功能对应一个分支，对于功能的拆分需要比较合理，以避免一些后面不必要的代码冲突 |
| release | 发布分支，发布时候用的分支，一般测试时候发现的 bug 在这个分支进行修复 |
| hotfix  | hotfix 分支，紧急修 bug 的时候用                             |

GitFlow 的优势有如下几点：

- 并行开发：GitFlow 可以很方便的实现并行开发：每个新功能都会建立一个新的 `feature` 分支，从而和已经完成的功能隔离开来，而且只有在新功能完成开发的情况下，其对应的 `feature` 分支才会合并到主开发分支上（也就是我们经常说的 `develop` 分支）。另外，如果你正在开发某个功能，同时又有一个新的功能需要开发，你只需要提交当前 `feature` 的代码，然后创建另外一个 `feature` 分支并完成新功能开发。然后再切回之前的 `feature` 分支即可继续完成之前功能的开发。
- 协作开发：GitFlow 还支持多人协同开发，因为每个 `feature` 分支上改动的代码都只是为了让某个新的 `feature` 可以独立运行。同时我们也很容易知道每个人都在干啥。
- 发布阶段：当一个新 `feature` 开发完成的时候，它会被合并到 `develop` 分支，这个分支主要用来暂时保存那些还没有发布的内容，所以如果需要再开发新的 `feature`，我们只需要从 `develop` 分支创建新分支，即可包含所有已经完成的 `feature` 。
- 支持紧急修复：GitFlow 还包含了 `hotfix` 分支。这种类型的分支是从某个已经发布的 tag 上创建出来并做一个紧急的修复，而且这个紧急修复只影响这个已经发布的 tag，而不会影响到你正在开发的新 `feature`。

然后就是 GitFlow 最经典的几张流程图，一定要理解：
![img](https://cdn.nlark.com/yuque/0/2019/webp/128853/1564853230881-000559e1-bbcf-453e-b6a0-41b9a94e6937.webp#align=left&display=inline&height=715&originHeight=715&originWidth=483&size=0&status=done&width=483)
`feature` 分支都是从 `develop` 分支创建，完成后再合并到 `develop` 分支上，等待发布。
![img](https://cdn.nlark.com/yuque/0/2019/webp/128853/1564853230910-0cefe6ee-ee05-4332-b02b-44c65428d81a.webp#align=left&display=inline&height=715&originHeight=715&originWidth=483&size=0&status=done&width=483)
当需要发布时，我们从 `develop` 分支创建一个 `release` 分支
![img](https://cdn.nlark.com/yuque/0/2019/webp/128853/1564853230917-d7ab323c-06f5-4365-ae2d-f65a78c5faa0.webp#align=left&display=inline&height=715&originHeight=715&originWidth=494&size=0&status=done&width=494)
然后这个 `release` 分支会发布到测试环境进行测试，如果发现问题就在这个分支直接进行修复。在所有问题修复之前，我们会不停的重复**发布->测试->修复->重新发布->重新测试**这个流程。
发布结束后，这个 `release` 分支会合并到 `develop` 和 `master` 分支，从而保证不会有代码丢失。
![img](https://cdn.nlark.com/yuque/0/2019/webp/128853/1564853230859-855003ad-8d60-4c86-9803-a852c9d95b65.webp#align=left&display=inline&height=717&originHeight=717&originWidth=494&size=0&status=done&width=494)
`master` 分支只跟踪已经发布的代码，合并到 `master` 上的 commit 只能来自 `release` 分支和 `hotfix` 分支。
`hotfix` 分支的作用是紧急修复一些 Bug。
它们都是从 `master` 分支上的某个 tag 建立，修复结束后再合并到 `develop` 和 `master` 分支上。

> 更多工作流可以参考阮老师的[Git 工作流程](https://www.ruanyifeng.com/blog/2015/12/git-workflow.html)

### 3. rebase 与 merge的区别?

git rebase 和 git merge 一样都是用于从一个分支获取并且合并到当前分支.

假设一个场景,就是我们开发的[feature/todo]分支要合并到master主分支,那么用rebase或者merge有什么不同呢?

![2019-08-04-01-20-09](https://xiaomuzhu-image.oss-cn-beijing.aliyuncs.com/4f2db1d2289e18c7b874a9015a20b38d.png)

- marge 特点：自动创建一个新的commit 如果合并的时候遇到冲突，仅需要修改后重新commit
- 优点：记录了真实的commit情况，包括每个分支的详情
- 缺点：因为每次merge会自动产生一个merge commit，所以在使用一些git 的GUI tools，特别是commit比较频繁时，看到分支很杂乱。

![2019-08-04-01-22-04](https://xiaomuzhu-image.oss-cn-beijing.aliyuncs.com/ea8107df9a1650ed19b0b68ab3da4567.png)

- rebase 特点：会合并之前的commit历史
- 优点：得到更简洁的项目历史，去掉了merge commit
- 缺点：如果合并出现代码问题不容易定位，因为re-write了history

因此,当需要保留详细的合并信息的时候建议使用git merge，特别是需要将分支合并进入master分支时；当发现自己修改某个功能时，频繁进行了git commit提交时，发现其实过多的提交信息没有必要时，可以尝试git rebase.

### 4. git reset、git revert 和 git checkout 有什么区别？

这个问题同样也需要先了解 git 仓库的三个组成部分：工作区（Working Directory）、暂存区（Stage）和历史记录区（History）。

- 工作区：在 git 管理下的正常目录都算是工作区，我们平时的编辑工作都是在工作区完成
- 暂存区：临时区域。里面存放将要提交文件的快照
- 历史记录区：git commit 后的记录区

三个区的转换关系以及转换所使用的命令：

![2019-08-04-01-51-29](https://xiaomuzhu-image.oss-cn-beijing.aliyuncs.com/403ff092b287ae192b376ac93fb69816.png)

git reset、git revert 和 git checkout的共同点：用来撤销代码仓库中的某些更改。

然后是不同点：

首先，从 commit 层面来说：

- git reset 可以将一个分支的末端指向之前的一个 commit。然后再下次 git 执行垃圾回收的时候，会把这个 commit 之后的 commit 都扔掉。git reset 还支持三种标记，用来标记 reset 指令影响的范围：
  - --mixed：会影响到暂存区和历史记录区。也是默认选项
  - --soft：只影响历史记录区
  - --hard：影响工作区、暂存区和历史记录区

> 注意：因为 git reset 是直接删除 commit 记录，从而会影响到其他开发人员的分支，所以不要在公共分支（比如 develop）做这个操作。

- git checkout 可以将 HEAD 移到一个新的分支，并更新工作目录。因为可能会覆盖本地的修改，所以执行这个指令之前，你需要 stash 或者 commit 暂存区和工作区的更改。
- git revert 和 git reset 的目的是一样的，但是做法不同，它会以创建新的 commit 的方式来撤销 commit，这样能保留之前的 commit 历史，比较安全。另外，同样因为可能会覆盖本地的修改，所以执行这个指令之前，你需要 stash 或者 commit 暂存区和工作区的更改。

然后，从文件层面来说：

- git reset 只是把文件从历史记录区拿到暂存区，不影响工作区的内容，而且不支持 --mixed、--soft 和 --hard。
- git checkout 则是把文件从历史记录拿到工作区，不影响暂存区的内容。
- git revert 不支持文件层面的操作。