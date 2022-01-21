## Yarn 独有的命令

- `npm install` === `yarn` —— install安装是默认行为
- `npm install taco --save` === `yarn add taco` —— taco包立即被保存到 `package.json` 中。
- `npm uninstall taco --save` === `yarn remove taco`
- `npm install taco --save-dev` === `yarn add taco --dev`
- `npm update --save` === `yarn upgrade`
- 
- `npm install taco@latest --save` === `yarn add taco`
- `npm install taco --global` === `yarn global add taco` —— 一如既往，请谨慎使用 global 标记。

> 注意：使用 `yarn` 或 `yarn install` 安装全部依赖时是根据 `package.json` 里的 `dependencies` 字段来决定的

- 
- `npm init` === `yarn init`
- `npm init --yes/-y` === `yarn init --yes/-y`
- `npm link` === `yarn link`
- `npm outdated` === `yarn outdated`
- `npm publish` === `yarn publish`
- `npm run` === `yarn run`
- `npm cache clean` === `yarn cache clean`
- `npm login` === `yarn login`
- `npm test` === `yarn test`

## Yarn 独有的命令

- `yarn licenses ls` —— 允许你检查依赖的许可信息
- `yarn licenses generate` —— 自动创建依赖免责声明 license
- `yarn why taco` —— 检查为什么会安装 taco，详细列出依赖它的其他包
- `yarn why vuepress` —— 检查为什么会安装 vuepress，详细列出依赖它的其他包

# 特性

Yarn 除了让安装过程变得更快与更可靠，还添加了一些额外的特性，从而进一步简化依赖管理的工作流。

- 同时兼容 `npm` 与 `bower` 工作流，并支持两种软件仓库混合使用
- 可以限制已安装模块的协议，并提供方法输出协议信息
- 提供一套稳定的共有 JS API，用于记录构建工具的输出信息
- 可读、最小化、美观的 CLI 输出信息

