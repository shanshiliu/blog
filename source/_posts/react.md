---
title: React技术学习笔记
date: 2022-01-30 19:13:00
tags: React、hook、Redux
---
## React技术栈讲解

使用jsx语法，JavaScript 代码中将 JSX 和 UI 放在一起时，会在视觉上有辅助作用。jsx默认进行转译，以防止xss攻击
jsx经过编译之后转化为普通函数。Babel 会把 JSX 转译成一个名为 React.createElement() 函数调用。
React DOM负责更新DOM元素 ReactDOM.createRoot() root.render()  在实践中，大多数 React 应用只会调用一次 root.render() 需要更新的DOM则封装到状态组件中。

### 组件分为函数组件 和  class组件（es6）
  props 和 React.Component
  组件无论是使用函数声明还是通过 class 声明，都绝不能修改自身的 props. 所有 React 组件都必须像纯函数一样保护它们的 props 不被更改
  class组件state  constructor中定义初始化state
  componentDidMount() 方法会在组件已经被渲染到 DOM 中后运行， 通过setState修改 不要直接修改state，不更新  ps：state的更新可能异步的。出于性能考虑，React 可能会把多个 setState() 调用合并成一个调用
  如有依赖state自身的值进行更新或计算时，通过给setState传入箭头函数的方式来进行更新计算。
  自上而下的单向数据流  如果你把一个以组件构成的树想象成一个 props 的数据瀑布的话，那么每一个组件的 state 就像是在任意一点上给瀑布增加额外的水源，但是它只能向下流动

  事件处理：注意this的使用 将事件手动bind到class组件的this中
  状态提升：多个组件需要反映相同的变化数据，这时我们建议将共享状态提升到最近的共同父组件中去。


### props与state
  构建应用的静态版本时，我们需要创建一些会重用其他组件的组件，然后通过 props 传入所需的数据。
  props 是父组件向子组件传递数据的方式。即使你已经熟悉了 state 的概念，也完全不应该使用 state 构建静态版本。
  state 代表了随时间会产生变化的数据，应当仅在实现交互时使用。所以构建应用的静态版本时，你不会用到它。
  props（“properties” 的缩写）和 state 都是普通的 JavaScript 对象。它们都是用来保存信息的，这些信息可以控制组件的渲染输出，而它们的一个重要的不同点就是：props 是传递给组件的（类似于函数的形参），而 state 是在组件内被组件自己管理的（类似于在一个函数内声明的变量）state私有，受控于组件内
  React 中的数据流是单向的，并顺着组件层级从上往下传递。
  
### 状态提升


### 组合继承
Props 和组合为你提供了清晰而安全地定制组件外观和行为的灵活方式。注意：组件可以接受任意 props，包括基本数据类型，React 元素以及函数
我们并没有发现需要使用继承来构建组件层次的情况。如果你想要在组件间复用非 UI 的功能，我们建议将其提取为一个单独的 JavaScript 模块，如函数、对象或者类。组件可以直接引入（import）而无需通过 extend 继承它们

### 代码分割 Create React App开箱即用  动态import().then()  
   React.lazy   React.lazy 接受一个函数，这个函数需要动态调用 import()。它必须返回一个 Promise，该 Promise 需要 resolve 一个 default export 的 React 组件 然后应在 Suspense 组件中渲染 lazy 组件. 而在Suspense 组件上使用fallback属性接受在组件加载过程中的React元素。

### Contenxt 
  Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言。
  Context 主要应用场景在于很多不同层级的组件需要访问同样一些的数据
  API
  React.createContext() 创建一个Context对象
  Context.Provider  允许消费组件订阅 context 的变化。 对应多个消费组件Consumer  接收value属性传递给消费组件。  Object.is()检测新旧值的变化。
  Class.contextType  在 class 上的 contextType 属性可以赋值为由 React.createContext() 创建的 Context 对象。此属性可以让你使用 this.context 来获取最近 Context 上的值。你可以在任何生命周期中访问到它，包括 render 函数中。
  此API只能订阅单一的context。 订阅多个context时 使用Context组合
  Context.Consumer   一个 React 组件可以订阅 context 的变更，此组件可以让你在函数式组件中可以订阅 context
  Context.displayName
  示例
  动态 Context
  在嵌套组件中更新 Context
  使用多个 Context
  注意事项
  废弃的 API
  通过Provider传递  注意：将 undefined 传递给 Provider 的 value 时，消费组件的 defaultValue 不会生效。
  当 Provider 的 value 值发生变化时，它内部的所有消费组件都会重新渲染。从 Provider 到其内部 consumer 组件（包括 .contextType 和 useContext）的传播不受制于 shouldComponentUpdate 函数，因此当 consumer 组件在其祖先组件跳过更新的情况下也能更新
  多个消费组件组合 

### 错误边界
  static getDerivedStateFromError()
  comnponentDidCatch
    如果一个 class 组件中定义了 static getDerivedStateFromError() 或 componentDidCatch() 这两个生命周期方法中的任意一个（或两个）时，那么它就变成一个错误边界。
    当抛出错误后，请使用 static getDerivedStateFromError() 渲染备用 UI ，使用 componentDidCatch() 打印错误信息
    注意错误边界仅可以捕获其子组件的错误，它无法捕获其自身的错误
    自 React 16 起，任何未被错误边界捕获的错误将会导致整个 React 组件树被卸载

### Refs转发 大多数组件无需使用，用于可复用组件或组件库。
  Ref 转发是一个可选特性，其允许某些组件接收 ref，并将其向下传递（换句话说，“转发”它）给子组件   勿多度使用refs
  React.createRef()  非受控组件使用：例如input type file
  React.fowardRef() props ref作为第二个参数
  高阶组件中使用 ref将不会进行透传
  下面是几个适合使用 refs 的情况：
      管理焦点，文本选择或媒体播放。
      触发强制动画。
      集成第三方 DOM 库。


  当你开始在组件库中使用 forwardRef 时，你应当将其视为一个破坏性更改，并发布库的一个新的主版本。出于同样的原因，当 React.forwardRef 存在时有条件地使用它也是不推荐的：它改变了你的库的行为，并在升级 React 自身时破坏用户的应用。
### Fragments
  在组件中返回多个元素的 Fragments允许你将子列表分组，并无需向DOM添加额外节点。添加key唯一属性
  <React.Fragment></React.Fragment> 或短语法 <> </>
### 高阶组件 HOC
  React用户组件复用逻辑的高级技巧  不是React API的一部分 是基于React组合特性形成的一种设计模式。 具体而言，高阶组件是参数为组件，返回值为新组件的函数
  使用HOC横切解决关注点问题  替代mixins 
  不要改变原始组件，应该使用组合
  您可能已经注意到 HOC 与容器组件模式之间有相似之处。容器组件担任将高级和低级关注点分离的责任，由容器管理订阅和状态，并将 prop 传递给处理 UI 的组件。HOC 使用容器作为其实现的一部分，你可以将 HOC 视为参数化容器组件。
  约定： 
  讲不相关的props传递给被包裹组件
  最大化可组合性
  包装显示名称以便轻松调试
  注意事项⚠️：
    不要在render方法中使用HOC   每次调用render()会创建新的组件  这不仅仅是性能问题 - 重新挂载组件会导致该组件及其所有子组件的状态丢失。
    务必复制静态方法  可使用 hoist-non-react-statics 自动拷贝所有非 React 静态方法  或将所有静态方法 额外导出
    使用箭头函数 或bind(this)绑定

### Render Props


### Hook简介
  Hook 是React16.8版本新增特性，它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。
  使用HOOK的原因：
    Hook 使你在无需修改组件结构的情况下复用状态逻辑
    复杂组件变得难以理解，Hook 将组件中相互关联的部分拆分成更小的函数（比如设置订阅或请求数据）
    我们还发现 class 是学习 React 的一大屏障。你必须去理解 JavaScript 中 this 的工作方式。还不能忘记绑定事件处理器。Hook 使你在非 class 的情况下可以使用更多的 React 特性
    而 Hook 则拥抱了函数，同时也没有牺牲 React 的精神原则。Hook 提供了问题的解决方案，无需学习复杂的函数式或响应式编程技术。
  Hook 就是 JavaScript 函数，但是使用它们会有两个额外的规则：
    只能在函数最外层调用 Hook。不要在循环、条件判断或者子函数中调用。
    只能在 React 的函数组件中调用 Hook。不要在其他 JavaScript 函数中调用。（还有一个地方可以调用 Hook —— 就是自定义的 Hook 中）
    每个组件间的 state 是完全独立的。Hook 是一种复用状态逻辑的方式，它不复用 state 本身。事实上 Hook 的每次调用都有一个完全独立的 state —— 因此你可以在单个组件中多次调用同一个自定义 Hook。

### Redux
  state 初始化定义
  action 普通js对象，描述发生了什么的指示器。
  reducer  将action和state串起来，开发一些函数。 接收state和action参数 并返回新的state

  应用的整体全局状态以对象树的方式存放于单个 store。 唯一改变状态树（state tree）的方法是创建 action，一个描述发生了什么的对象，并将其 dispatch 给 store。 要指定状态树如何响应 action 来进行更新，你可以编写纯 reducer 函数，这些函数根据旧 state 和 action 来计算新 state


  Redux Toolkit 有一个名为 createSlice 的函数，它负责生成 action 类型字符串、action creator 函数和 action 对象的工作。您所要做的就是为这个切片定义一个名称，编写一个包含 reducer 函数的对象，它会自动生成相应的 action 代码。

  用 Thunk 编写异步逻辑。chunk 是一种特定类型的 Redux 函数，可以包含异步逻辑。Thunk 是使用两个函数编写的：
      一个内部 thunk 函数，它以 dispatch 和 getState 作为参数
      外部创建者函数，它创建并返回 thunk 函数
 异步逻辑： 
	Redux 有多种异步中间件，每一种都允许您使用不同的语法编写逻辑。
	最常见的异步中间件是 redux-thunk，它可以让你编写可能直接包含异步逻辑的普通函数。
	Redux Toolkit 的 configureStore 功能默认自动设置 thunk 中间件，我们推荐使用 thunk 作为 Redux 开发异步逻辑的标准方式。
	
### Fiber
 React16版本后 增加异步中断更新和 双缓存概念
 一个FiberNode当中存在很多属性，我们大体将他们分为三类：
 
 静态属性：保存当前Fiber节点的 标签，类型等；
 关联属性：用于连接其他Fiber节点形成Fiber树；
 工作属性：保存当前Fiber节点的动态工作单元；
 而多个Fiber节点之间正是通过关联属性的连接形成一个Fiber树；因为每一个Fiber节点都是相互独立的，因此Fiber节点之间通过指针指向的方式产生联系，return指向的是父级节点，child指向的是子节点，sibling指向的是兄弟节点
 
 
 
      






