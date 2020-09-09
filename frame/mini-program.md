# 小程序

## weapp







## Taro

### Tips

- 使用Taro组件，而不是html标签
- div -> View
- 常用组件: View, Text, Image, Button, Input



```
import Taro, { Component } from '@tarojs/taro'
import { connect } from '@tarojs/redux'
import { View, Text, Button } from '@tarojs/components'
import { AtTabBar } from 'taro-ui'
import './index.scss'
```





```
# 使用 npm 安装 CLI
$ npm install -g @tarojs/cli
# OR 使用 yarn 安装 CLI
$ yarn global add @tarojs/cli
# OR 安装了 cnpm，使用 cnpm 安装 CLI
$ cnpm install -g @tarojs/cli

taro init myApp



# weapp
# yarn
$ yarn dev:weapp
$ yarn build:weapp

# npm script
$ npm run dev:weapp
$ npm run build:weapp

# 仅限全局安装
$ taro build --type weapp --watch
$ taro build --type weapp

# npx 用户也可以使用
$ npx taro build --type weapp --watch
$ npx taro build --type weapp
```









```
$ taro init project_name

$ taro create page page_name

$ yarn dev:weapp

$ yarn add dva-core dva-loading

Taro.navigateTo({

 url: '/pages/aoo/aoo',

})

```



dva.js

```
import { create } from 'dva-core';
import { createLogger } from 'redux-logger';
import createLoading from 'dva-loading';
 
let app;
let store;
let dispatch;
 
function createApp(opt) {
  // redux日志
  // opt.onAction = [createLogger()];
  app = create(opt);
  app.use(createLoading({}));
 
  if (!global.registered) opt.models.forEach(model => app.model(model));
  global.registered = true;
  app.start();
 
  store = app._store;
  app.getStore = () => store;
 
  dispatch = store.dispatch;
 
  app.dispatch = dispatch;
  return app;
}
 
export default {
  createApp,
  getDispatch() {
    return app.dispatch;
  }
}
```





app.jsx

```
import dva from './utils/dva'
const dvaApp = dva.createApp({
  initialState: {},
  models: models,
});
const store = dvaApp.getStore();
```



缩进

.editorconfig

```
# http://editorconfig.org
root = true


[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true


[*.md]
trim_trailing_whitespace = false


[Makefile]
indent_style = tab
```



Warning

- React 可以使用 ... 拓展操作符来传递属性，但在 Taro 中你不能这么做 <Greeting {...props} />
- wxss优先级 !important(inf.) style=“”(1000) #id(100) .class(10) element(1)