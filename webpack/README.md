### 1. `file-loader` 和 `url-loader`的区别是什么？

- `file-loader`

  主要解决开发时引入文件路径和打包后文件的路径不一致的问题。

- `url-loader`

  图片较多时，会有较多的 http 请求，会降低页面性能。`url-loader`会将引入的图片编码，生成 dataURL 并将其打包到文件中，最终引入这个 `dataURL` 就能访问图片资源。`url-loader`提供了一个`limit`参数，小于`limit`字节的文件会被转为`dataURL`，大于`limit`的会使用`file-loader`进行 copy。

- 联系

  `url-loader` 内部封装了 `file-loader`。`url-loader` 不依赖于 `file-loader`，即使用 `url-loader` 时，只需要安装 `url-loader` 即可，不需要安装 `file-loader`。

### 2. 常见的`loader`

- `file-loader` `url-loader`
- `image-loader`
- `babel-loader`
- `style-loader` `css-loader` `less-loader` `sass-loader`
- `ts-loader` `json-loader`
- `eslint-loader` `tslint-loader`
- `vue-loader`

### 3. 常见的`plugin`

- `ignore-plugin` ：忽略部分文件
- `html-webpack-plugin` ：简化 `HTML` 文件创建
- `web-webpack-plugin` ：可方便的为单页面应用输出 `HTML`
- `clean-webpack-plugin` ：清理目录
- `speed-measure-webpack-plugin` ：可以查看 `Loader` 和 `Plugin` 执行耗时
- `webpack-bundle-analyzer` ：可视化 `Webpack` 输出文件的体积
- `webpack-dashborad` ：更友好的展示相关打包信息
- `size-plugin`：监控资源体积变化

### 4. `loader`和`plugin`的区别

`Loader`的本质是一个函数，在函数内对接受到的内容进行转换，返回转换后的结果。因为`Webpack`只认识 JS，所以`Loader`充当的是一个翻译官的角色，对其他类型的资源进行预处理的工作。

`Plugin`就是插件，作用是扩展`Webpack`的功能.在`Webpack`运行生命周期中会广播出许多事件，`Plugin` 可以监听这些事件，在合适的时机通过`Webpack`提供的 API 改变输出结果。

`Loader`在`module.rules`中配置，作为模块的解析规则，类型是数组。每一项都是一个`Object`，内部包含`test`，`use`，`options`等属性.

`Plugin`在`Plugins`中单独配置，每一项是一个`Plugin`实例，参数通过构造函数传入。

### 5. `source map` 是什么？生产环境怎么用？

`source map`是将编译、打包、压缩后的代码映射回源代码的过程。打包压缩后的代码不具备良好的可读性，想要调试源码就需要 `soucce map`。

### 6. `Webpack` 的热更新原理

### 7. 如何优化 `Webpack` 的构建速度

### 8. Webpack 构建原理·

三个阶段：

1.  初始化阶段
    1. 初始化参数：从配置文件，配置对象和 `shell` 参数中读取，与默认配置结合得出最终参数
    2. 创建编译器对象：用上一步的参数创建 `Compiler` 对象
    3. 初始化编译环境：注入内置插件，注册各种模块工厂，初始化 `RuleSet` 集合，加载配置的插件
    4. 开始编译：执行 `compiler` 对象的 `run` 方法
    5. 确认入口： 根据配置文件中的 `entry` 找出所有的入口文件，调用 `compilition.addEntry` 将入口文件转换为 `dependance` 对象
2.  构建阶段
    1. 编译模块：根据 `entry` 对应的 `dependence` 创建 `module` 对象，调用 `loader`，将模块转译成标准 JS 内容，调用 JS 解释器将内容转换为 `AST` 对象，从中找到该模块依赖的模块，再递归本步骤直到所有文件都经过该步骤处理
    2. 完成模块编译：递归完所有步骤后，得到每个模块被翻译后的内容以及他们的`依赖关系图`
3.  生成阶段
    1.  输出资源(seal)：根据入口和模块间的依赖关系，组装成一个个包含多个模块的 `chunk`，再把每个 `chunk` 转换成一个单独的文件加入到输出列表。
    2.  写入文件系统：确认好输出内容后，根据配置的文件输出路径和文件名，把文件内容写入到文件系统
