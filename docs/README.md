## 项目介绍

IOT物联网平台V1.0.0基础功能开发已完成，已满足交付各产品线应用的需求。前端参与了两个服务的开发，物联网前端基础管理服务以及物联网后端(node)基础管理服务

仓库地址如下，欢迎大家一起交流

物联网前端基础管理服务：https://git.fpi-inc.site/product/public-products/iot/iot-base-manage-web

物联网后端(node)基础管理服务：https://git.fpi-inc.site/product/public-products/iot/iot-base-manage-server


## 项目技术栈

- React（v16.9.11）
- React-Router-Dom（路由管理）
- Mobx（状态管理）
- Antd（UI组件库）
- TypeScript
- Egg.js（Node框架）


## 项目运行

```
# 克隆到本地
git clone https://git.fpi-inc.site/product/public-products/iot/iot-base-manage-web

# 安装依赖
yarn

# 开启本地服务器localhost:8080
yarn start

# 发布环境
yarn build

```

## 代码检查

ESLint + Prettier来统一我们的前端代码风格

Eslint可以帮我们校验代码，给整个项目的代码定义一个规范，我们写的代码必须按照这个规范写，否则在校验阶段会报错

Prettier是一个Javascript的格式化工具，可以完全统一整个团队的代码风格执行一行命令，即可全局格式化代码，并统一风格

```js
    /**
    * Preitter配置
    */

    {
        singleQuote: true,
        trailingComma: 'all',
        printWidth: 100,
        semi: false,
        tabWidth: 4,
        jsxBracketSameLine: false,
        overrides: [
            {
                files: '.prettierrc',
                options: {
                    parser: 'json',
                }
            },
        ]
    }
```

```js

    /**
    * Pre-commit 脚本配置
    */

    "scripts": {
        "eslint": "eslint --ext .tsx,.ts --fix ./src"
    },

    "husky": {
        "hooks": {
            "pre-commit": "lint-staged",
            "commit-msg": "commitlint -e $GIT_PARAMS"
        }
    },

    "lint-staged": {
        "*.{ts,tsx}": [
            "npm run eslint"
        ]
    },
```


## 主题配置

Antd 的样式使用了 Less 作为开发语言，并定义了一系列全局/组件的样式变量，你可以根据需求进行相应调整

所有样式变量可以在 https://github.com/ant-design/ant-design/blob/master/components/style/themes/default.less  找到

```js
    /**
    * Theme 覆盖
    */

    module.exports = {
        '@primary-color': '#2B6DFF',
        '@success-color': '#2FCA65',
        '@primary-collocation-color': '#F0F7FF',
        '@info-color': '#7C7C7C',
        '@warning-color': '#FFBA1B',
        '@error-color': '#F86161',
        '@highlight-color': '#F5222D',
        '@font-size-base': '12px',
        '@text-color': '#333',
        '@menu-item-font-size': '14px',
        '@breadcrumb-font-size': '14px',
        '@table-row-hover-bg': '#fafafa',
        '@table-selected-row-bg': '#f1f7fe',
        '@table-padding-vertical': '14px',
        '@table-header-color': '#080808',
        '@btn-height-base': '28px',
        '@btn-padding-base': '0 11px',
        '@input-placeholder-color': '#BFBFBF',
        '@input-border-color': '#E6E6E6',
        '@select-border-color': '#E6E6E6',
        '@menu-icon-size': '14px',
    }
```

## Why Hooks

为什么要放弃 Class，转用 Hooks 呢？在内部外部有很多争论，包括知乎也有类似提问。我们也不免俗套的要对比下 Class 和 Hooks 了

### Class 学习成本高

![](//upload-images.jianshu.io/upload_images/16775500-8d325f8093591c76.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/740/format/webp)

这个是 React v15 的生命周期，你都掌握了吗？你知道 v16 有什么变化吗？

之前无论你去哪里面试，基本都会有几个必问问题：

- 讲讲 React 生命周期？React v15 和 React v16 生命周期有啥变化？
- 如何优化 Class 组件？shouldComponentUpdate 是做什么的？如何用？
......

生命周期最重要，但是有很高的学习成本，需要大量实践才能积累足够的经验。当然，这几个问题回答不好，百分之八十以上的几率会挂掉。

当然不止是生命周期，this 也是一个很大的问题。你有没有在组件写很多 bind？或者所有的函数都用箭头函数定义？

```js
    this.someFunction = this.someFunction.bind(this);
    // 或
    someFunction = () => {}
```

### hooks 学习成本低

对比 Class，Hooks 的学习成本可就太低了！掌握了 useState 和 useEffect，80% 的事情就搞定了

### Class 业务逻辑分散

实现一个功能，我要写在不同的生命周期里面，不聚合， 比如，如果你有个定时器，你一定要在 componentWillUnMount 去卸载

```js

    /**
    * Class组件实现
    */

    componentDidMount() {
        this.timer = setTimeout(() => {
            // todo
        }, 1000)
    }

    componentWillUnmount() {
        if (this.timer) {
            clearTimeout(this.timer)
        }
    }

```

再比如，我们要写一个数据请求组件，当id变化时，要重新发起请求。我们就要在两个生命中期中写请求的逻辑

```js

    /**
    * Class组件实现
    */
    componentDidMount() {
        const { id } = this.props
        this.loadData(id)
    }

    componentDidUpdate(prevProps) {
        const currentId = this.props.id
        if(prevProps.id !== currentId) {
            this.loadData(currentId)
        }
    }

```

### Hooks 业务逻辑聚合

而 Hooks 的业务逻辑就非常聚合了,而且实现起来非常简单。上面的两个例子，写法如下：


```js

    /**
    * Hooks组件实现
    */
    useEffect(() => {
        const timer = setTimeout(() => {
            //todo
        }, 1000)
        return () => {
            clearTimeout(timer)
        }
    }, [])


    useEffect(() => {
        loadData(id)
    }, [id])

```


### Class 逻辑复用困难

Class 的 Render Props 和 HOC（高阶组件）可以做逻辑复用，但是具体的实现会有很多的弊端

我们看看 Render Props 首先我们想复用监听 window size 变化的逻辑

```jsx
    <windowSize>
        (size) => <Component size={size}></Component>
    </windowSize>
```

然后，我又想复用监听鼠标位置的逻辑，我只能这么写了

```jsx
    /**
    * Render Props实现逻辑复用
    */
    <windowSize>
        (size) => (
            <Mouse>
                (position) => <Component size={size} position={position}></Component>
            </Mouse>
        )
    </windowSize>
```

如果还有更多的逻辑要复用，只能这样继续嵌套...

而通过HOC来实现复杂的逻辑复用容错率非常差，大家都往组件的 props 上面挂属性，如果有重名的属性，就会产生很多的未知的错误

### Hooks 逻辑复用简单

Hooks 逻辑复用简单


```js

    /**
    * Hooks实现逻辑复用
    */
    const size = useSize()

    const position = UseMouse()

    ...

```

推荐react-use 常用的hooks组件 https://github.com/streamich/react-use

## 项目核心目录

- component 组件目录
    - ITree
    - ITable
    - IForm
    - IRangerPicker
    - IConfirm
    - IModal
    - ...
- hooks 重复逻辑抽取
    - useFetch
    - useForm
    - useInput
    - useIsMobile
    - useModal
    - useStore
    - useTable
    - ...
- pages 业务页面
    - dashboard
    - ...
- layout 视图拆分
    - BaseLayout
    - BreadCrumb
    - LayoutContent
    - SiderMenu
    - LayoutHeader
- rules 校验规则
    - site
    - ...
- services 请求模块化
    - dashboard
    - ...
- store 状态管理
    - global
    - ...
- routes 路由管理
    - ...
- utils 工具管理
    - ...


## 页面实现介绍

我们对User组件的实现代码进行讲解

```tsx

    /**
    * User组件
    */
    function User(props: RouteComponentProps): JSX.Element {
        const logout = async () => {
            await userService.logout()
            removeToken()
            redirectLogout()
        }

        const [userInfo, setUserInfo] = useState<any>(null)

        const [confirmVisible, setConfirmVisible] = useState<boolean>(false)

        const { menuPosition, themeType } = useStore()

        useEffect(() => {
            ;(async () => {
                const token = getToken()
                if (token) {
                    const result: any = await userService.info(token)
                    setUserInfo(result)
                }
            })()
        }, [])

        const menu = (
            <Menu style={{ width: '100%', float: 'right' }}>
                <Menu.Item
                    style={{ textAlign: 'center' }}
                    onClick={() => {
                        setConfirmVisible(true)
                    }}
                >
                    退出登录
                </Menu.Item>
            </Menu>
        )
        const UserIcon = (
            <Icon
                component={() =>
                    menuPosition === 'top' && themeType !== 'default' ? <DarkUserSvg /> : <UserSvg />
                }
                style={{ fontSize: 28 }}
            />
        )

        return (
            <React.Fragment>
                <Dropdown
                    overlay={menu}
                    overlayClassName={styles.overlay}
                    getPopupContainer={node => node}
                    placement="bottomRight"
                >
                    <div
                        className={classNames(styles.user, {
                            [styles.darkIcon]: menuPosition === 'top' && themeType === 'dark',
                            [styles.blueIcon]: menuPosition === 'top' && themeType === 'blue',
                        })}
                    >
                        <Avatar
                            icon={UserIcon}
                            size={28}
                            style={{
                                backgroundColor:
                                    menuPosition === 'top' && themeType != 'default'
                                        ? 'rgba(255,255,255,0.2)'
                                        : '#fff',
                            }}
                        />
                        <span className={styles.name}>{userInfo ? userInfo.name : ''}</span>
                    </div>
                </Dropdown>
                <IConfirm
                    visible={confirmVisible}
                    onOk={logout}
                    onCancel={() => setConfirmVisible(false)}
                    content=""
                    title={<div style={{ paddingTop: 2 }}> 您确定要退出登录吗? </div>}
                />
            </React.Fragment>
        )
    }
```

## 开发中遇到的问题

- 使用单个 state 变量还是多个 state 变量

```js
    /**
    * 多个State
    */
    const [width, setWidth] = useState(100)
    const [height, setHeight] = useState(100)
    const [left, setLeft] = useState(0)
    const [top, setTop] = useState(0)
```

```js
    /**
    * 单个个State
    */
    const [state, setState] = useState({
        width: 100,
        height: 100,
        left: 0,
        top: 0
    })
```

将完全不相关的 state 拆分为多组 state

如果某些 state 是相互关联的，或者需要一起发生改变，就可以把它们合并为一组 state

- deps 依赖过多，导致 Hooks 难以维护


依赖数组依赖的值最好不要超过 3 个，否则会导致代码会难以维护

如果发现依赖数组依赖的值过多，我们应该采取一些方法来减少它，去掉不必要的依赖。


```js
    /**
    * deps 依赖过多
    */
    useEffect(() => {
        id && doSearch({name, address, status, personA, personB, progress})
    }, [id, name, address, status, personA, personB, progress])

```

将 Hook 拆分为更小的单元，每个 Hook 依赖于各自的依赖数组

通过合并相关的 state，将多个依赖值聚合为一个
