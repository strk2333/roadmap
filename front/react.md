# React

用于构建用户界面的 JavaScript 库

## 基础

- 概念



- 生命周期





## 问题剖析

- 为什么不建议使用索引来用作 key 值

  react以key来唯一标识组件，

  当发现update前和update后key值没有变化，会认为update前后组件是同一个，

  不会整个重新渲染这个组件，只会对它内部的属性进行修改

- diff

  tree diff: 

  - 通过updateDepth对Virtual DOM树进行层级控制。
  - 对树分层比较，两棵树**只对同一层次节点**进行比较。如果该节点不存在时，则该节点及其子节点会被完全删除，不会再进一步比较。
  - 只需遍历一次，就能完成整棵DOM树的比较。

  component diff: 

  - 拥有相同类的两个组件 生成相似的树形结构，拥有不同类的两个组件 生成不同的树形结构。

  - 同一类型的两个组件，按原策略（层级比较）继续比较Virtual DOM树即可。

  - 同一类型的两个组件，组件A变化为组件B时，可能Virtual DOM没有任何变化，如果知道这点（变换的过程中，Virtual DOM没有改变），可节省大量计算时间，所以 用户 可以通过 shouldComponentUpdate() 来判断是否需要 判断计算。

  - 不同类型的组件，将一个（将被改变的）组件判断为dirty component（脏组件），从而替换 整个组件的所有节点。

  element diff: 

  - 对于同一层级的一组子节点，通过唯一id区分。
  
- 刷新闪烁

  - 拆分组件
  - 使用了 uuid 的问题。使用 uuid() 会获取不同的key，造成不必要的更新
  
  

## 个人实践

index.js

```jsx
import React, { Component } from 'react'; // 引入React
import { connect } from 'dva';
import testImg from '@/assets/test.png'; // jsconfig.json 配置@
import PageLoading from '@/components/PageLoading'; // 用于局部加载，整个页面的loading可以统一处理
import styles from './style.less';

@connect(({ home, user }) => ({
  ...home,
  userInfo: user.currentUser,
  isLogin: user.currentUser.userId,
}))
class Page extends Component {
  state = {
    currentTab: 0,
    counter: 0,
  }

	constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.input = React.createRef();
    this.state = {
      currentTab: 0,
    };
    this.setState((state, props) => ({
  		counter: state.counter + props.increment,
		}));
  }
  
	componentWillMount // deprecated
  componentDidMount() {
    const { dispatch } = this.props;
    dispatch({
      type: 'home/fetch',
    });
  }

	onClick = e => {
    const name = e.target.name;
    this.setState({
      [name]: e.target.value,
    });
  }

	handleSubmit = e => {
    console.log(this.input.current.value);
    e.preventDefault();
  }

	renderTabs = () => (
  	<ul>
    	<li><a href-"/">首页</a></li>
      <li><a href="/list">列表</a></li>
			<li><a href="/modify">修改</a></li>
    </ul>
  )

	renderInfo = () => {
    const { userInfo } = this.props;
    
    return (
    	<div>
      	<div>{userInfo.name}</div>
        <div>{userInfo.lv}</div>
      </div>
    )
  }
  
  renderData = data => (
    <div>
      {
        data.map(v => (
          <div>{v.name}</div>
        ))
      }
    </div>
  )
  
  render() {
  	const { isRegistered, counter, data } = this.props;
  	return (
  		<div className={styles.container}>
        {this.renderTabs()}
  			<div className={`${styles.style1} ${styles.style2}`}>
          { isRegistered ? this.renderInfo() : <a href="/login">请先登录</a> }
          { data ? this.renderData(data) : <Loading /> }
          <form onSubmit={this.handleSubmit}>
          	<input type="text" ref={this.input} defaultValue="Bob" />
          	<button>提交</button>
          </form>
        </div>
  		</div>
  	)
  }
}

export default Page;
```



model.js

```js
import { routerRedux } from 'dva/router';
import { getData } from './service';

export default {
  namespace: 'home',
  state: {
    
  },
  effects: {
    *fetch() {
      const { data, ok, msg } = yield call(getData);
      if (ok) {
        yield put({
          type: 'save',
          payload: { data },
        });
      } else {
        message.error(msg);
      }
    },
    *test() {
      yield put(routerRedux.push(’/home/list’));
    },
  },
  reducers: {
    save(state, { payload }) {
      return {
        ...state,
        ...payload,
      };
    },
  }
}
```



service.js

```js
import request from '@/utils/request';

export function getData() {
  return request('/mms/provider/authenProvider/1.3', { method: 'GET' });
}
```



styles.less

```less
@import '../../share.less';
@font-size-normal: 16px;

.container {
  padding: 8px;
  background-color: white;
  min-height: 100vh;

}
```



配置

config.js

```js
{
  routes: [
    {
      path: '/',
      menuKey: 'main',
      component: '../layouts/BasicLayout',
      routes: [
        {
          menuKey: 'home',
          name: 'home',
          path: '/',
          component: './Home',
          routes: [
            {
              name: 'list',
              path: '/list',
              component: './List',
              routes: [
                {
                  name: 'listInfo',
                  path: '/info/:id',
                  component: './List/info'
                }
              ]
            },
						{
              name: 'modify',
              path: '/modify',
              component: './Modify',
            },
            {
              component: './404',
            },
          ],
        }
      ],
    },
  ]
}
```



jsconfig.json

```json
{
  "compilerOptions": {
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    },
    "jsx": "preserve"
  },
  "exclude": [
    "dist",
    "node_modules"
  ]
}
```



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



.esliintrc

```
const { strictEslint } = require('@umijs/fabric');

module.exports = {
  ...strictEslint,
  globals: {
    page: true,
    YAPI: true,
    BASE_URL: true,
  },
  settings: {
    ...(strictEslint.settings || {}),
    'import/resolver': { node: { extensions: ['.js', '.jsx', '.ts', 'tsx'] } },
  },
  rules: {
    ...(strictEslint.rules || {}),
    'max-len': [
      'error',
      {
        code: 120,
        ignoreStrings: true,
        ignoreComments: true,
        ignoreRegExpLiterals: true,
        ignoreTemplateLiterals: true,
        ignoreTrailingComments: true,
      },
    ],
    'no-unused-expressions': 0,
    // 禁用未使用过的标签
    'no-unused-labels': 0,
    'jsx-a11y/label-has-for': 0,
    'jsx-a11y/label-has-associated-control': [
      2,
      {
        labelAttributes: ['label'],
      },
    ],
  },
};
```

.eslintignore

```
/lambda/
/scripts
/config
/public
```

npm/yarn

```
npm install --save-dev xxx
yarn add (--dev) xxx --dev

npm install -g xxx
yarn global add xxx
```



## 性能测试优化





# Antd Dva Umi























表单处理

- rules
  - pattern
  - required













Redux && Sagas

```js
 // 阻塞
 yield all([
          put.resolve({
            type: 'getTaxList',
            payload,
          }),
          put.resolve({
            type: 'getCaseList',
            payload,
          }),
        ])
```





umi-request

- 发送字符串参数时会自动转number





Umi

  loading: *loading*.effects['pcInfoSkill/fetch'],