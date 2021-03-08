## React

### JSX

- 在 JXS 中可以使用 {表达式} 嵌入表达式

- 表达式：产生值的一组代码的集合

  - 变量
  - 算术运算
  - 函数调用
  - ……
  - 注意：分清楚 表达式 与 语句 的区别，if、for、while 这些都是语句，JSX 不支持语句

- #### 输出数据类型

  - 字符串、数字：原样输出
  - 布尔值、空、未定义 会被忽略
  - 数组：去掉,号，直接输出
  - 对象：不支持对象的直接输出

#### 列表渲染

- 数组
- 对象
- 在 React 中要做列表渲染，就想想办法将 内容组织成一个数组，借助数组来进行列表渲染，做列表渲染时，一定记得设置key

#### 条件渲染

- 三元运算符
- 与或运算符

### 在属性上使用表达式

JSX 中的表达式也可以使用在属性上，但是使用的时候需要注意

- 当在属性中使用 {} 的时候，不要使用引号包含

#### JSX 使用注意事项

- 必须有,且只有一个顶层的包含元素 - React.Fragment

- JSX可以作为值使用，并不是字符串

- 它可以配合JavaScript 表达式一起使用

- Fragment 文档碎片，作为包含容器存在，并不会渲染到真实DOM中

- JSX 不是html，很多属性在编写时不一样

  - class 变为 className
  - style  变为 一个对象

  ```JS
   let style = {
     width: "100px",
     height: "100px",
     background:"red"
   }
   let div = <div 
     className={false?"div":"box"}
     style={style}
   >div</div>
  
  let div = <div 
    className={false?"div":"box"}
    style={{
    width: "100px",
    height: "100px",
    background:"red"
  }}
  ```

- 列表渲染时，必须有 key 值

- 在 jsx 所有标签必须闭合

- 组件的首字母一定大写，标签一定要小写

```JS
 {// Fragment 包含容器，不会在页面中显示 //}
let div = <Fragment> 
  <div
    className={false ? "div" : "box"}
    style={{   // 一个是差值{} 一个是对象{}
      width: "100px",
      height: "100px",
      background: "red"
    }}
  >div</div>
  <input />
  <div>div2</div>
</Fragment>;
```

### createElement & render

```JS
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>

  <script src="js/react.js"></script>
  // React 核心库：虚拟DOM 、 组件

  <script src="js/react-dom.js"></script>
  // 提供将 React 构建好的视图渲染成真实的DOM
</head>
<body>
  <div id="root"></div>
<script>
/*
  构建视图：
      元素、组件 -- ReactNodes (虚拟DOM，以对象的形式模拟DOM树)
      ReactNode createElement(type,props,children1,children2……childrenN)
      参数：
        type 类型
        props 属性 {}
        children 内容或子元素 string|ReactNode
      返回值：
        ReactNode (react 构建好的虚拟DOM)

*/  
let span = React.createElement("span",null,"span1");
let span2 = React.createElement("span",null,"span2");
let div = React.createElement("div",{className:"box",id:"box"},span,span2);

/*
  ReactDOM.render(inner,container[,callBack]) 将React构建好的视图渲染到真实DOM中
  参数：
      inner React 构建好的视图（虚拟DOM）
      container 容器，将 React 构建好的视图（挂载）渲染到那个元素中, DOM Element
      callBack 真实DOM渲染完的回调
*/
ReactDOM.render(
  div,
  document.querySelector("#root"),
  ()=>{
    console.log("渲染完成");
  }
);
</script>  
</body>
```

## React组件

### 类组件

- 创建类组件必须要继承自 React.Component
- 类组件 必须要有 render 方法,在render方法的return中定义该组件要输出的视图
- 组件关系：
  - 在 A 组件中，调用了 B 组件，则 B组件称之为 A 的子组件，A 为 B 的父组件
- props：
  当父组件调用 子组件可以将 要传递给 子组件的信息添加在子组件的属性中，在子组件中可通过 props 属性来接收父组件传递过来的信息。
- 类组件需要在挂载周期和更新周期都请求数据

```JS
import { Component } from "react"
/*
  React 中的事件添加：
    1. React 添加事件类似于 js 的行间事件
    2. 事件名，从第二个单词开始首字母大写
    3. 注意事件处理函数的 this 指向为 undefined
      1. 使用箭头函数
      2. 通过 bind 绑定
*/

// 通过 箭头函数 获取 this
// 类组件，继承自Component
class Li extends Component {
  // 定义 state
  state={
    show: false
  }
  handlerClick=()=>{
    const {show} = this.state;
    // 修改state
    this.setState({
      show: !show
    })
  }
  // 组件的render函数
  render() {
    const {show} = this.state;
    const {data} = this.props;
    const {title,children} = data;
    // renturn 是视图显示的内容
    return <li className={show?"subList-show":""}>
        <a onClick={this.handlerClick}>{title}</a>
        <ul className="subList">
            {children.map((item,index)=>{
                return <li key={index}>{item}</li>
            })}
        </ul>
      </li>
  }
}
// 导出组件
export default Li;
```

```JS
import { Component } from "react"
// 引入样式
import "./index.css"; 
// 引入组件
import Li from "./li";
// 引入数据
import data from "./data";
// 组件
class App extends Component {
  render() {
    return <ul id="menu">
        {data.map((item,index)=>{
            return <Li 
              key={index} 
              data={item}
            />
        })}
    </ul>
  }
}
export default App;
```

### 函数组件

- 函数的名称就是组件的名称
- 函数的返回值就是组件要渲染的内容
- 在 React 16.7 之前，函数式又叫做纯渲染组件或无状态组件，在组件中没有 生命周期和 state
- 函数式组件，本质就是一个常规函数，接收一个参数 props 并返回一个 reactElement
- 函数式组件中没有this和生命周期函数,不能使用 string ref
- 使用函数式组件时，应该尽量减少在函数中声明子函数，否则，组件每次更新时都会重新创建这个函数

```JS
function Title(props) {
  return <div className="title">
    <h1>todo</h1>
  </div>
}

export default Title;
```

## React hooks(钩子)

- React hooks 是React 16.8中的新增功能。它们使您无需编写类即可使用状态和其他React功能
- hook(钩子) 钩子函数 使函数组件拥有类组件相似的功能

#### Hooks 优势

- 简化组件逻辑
- 复用状态逻辑
- 无需使用类组件编写，不用考虑this问题

#### Hook 使用规则

- 只在 React 函数中调用 Hook
  - React 函数组件中
  - React 自定义 Hook 中
- 只在最顶层使用 Hook 

### 常用 hook

#### 	useState  

- 在组件中声明一个状态

`const [state, setState] = useState(initialState);`

`let [状态,修改该状态的方法] = useState(初始值);`

-  在同一个组件中可以使用 useState 定义多个状态
-  注意 useState 返回的 setState 方法，不会进行对象合并
- 注意 useState 返回的 setState 方法还是一个异步方法，多次修改状态，只执行一次更新
- 当 state 为引用类型，修改时注意要合并其他值

```JS
  // 0-引入
import { useState } from "react";

function App() {
    
  //1-在组件中声明一个状态
  const [state,setState] = useState({
    name: "小瑞瑞",
    count: 1
  });
    
  //2-获取状态
  const {name,count} = state;
    
  return ( <div className="App">
      <input 
        type="text" 
        value={name}

  //3-改变状态
        onChange={({target})=>{
          setState({
            name:target.value,
            count
          }); 
        }}
}
```

#### useEffect

- componentDidMount（挂载）、componentDidUpdate（更新） 和 componentWillUnmount（卸载）的集合
- 副作用函数，返还函数，依赖参数
- 用于DOM获取、请求数据

```JS
useEffect(()=>{
            //副作用函数
            return ()=>{
              //副作用函数的返回函数
            }
 },[依赖参数]);
```

- 副作用函数&依赖参数：

  -  当 useEffect 没有依赖参数时，副作用函数，会在组件挂载完成及组件更新完成时执行

  ```JS
  //1-挂载完成及更新完都要做某件事
    useEffect(()=>{
      console.log("挂载完成及更新完都要做某件事");
    })
  ```

  - 当有依赖参数副作用函数，会在组件挂载完成及该依赖参数修改，引起的组件更新完成之后执行

  ```JS
    // 4-count组件更新完成之后或即将卸载时，要做某些事情，挂载的时候不做，要用返还函数，再要借助ref去掉即将卸载时执行。只在组件更新执行，待完善
    useEffect(()=>{
      // 返还函数
      return ()=>{
        console.log("count有更新或即将卸载时");
      }
    },[count]);
  }
  
  // 4-是在更新完成之后执行,完善版
      import { useEffect, useRef, useState } from "react";
  
  // 借助ref 定义一个变量为 false，初始化是挂载和更新执行，但是false，所以更新不执行，然后false 变为 true，更新的时候就执行了，挂载只在初始化的时候执行，所以达到只在更新的时候执行
  const isUpdate = useRef(false);
  
      useEffect(()=>{
        //console.log(isUpdate.current);
        if(isUpdate.current){
          console.log("当更新完成之后执行");
        } else {
          isUpdate.current = true;
        }
    });
  ```

  - 当依赖参数为空数组时，会在组件挂载完成之后执行

  ```JS
  // 2-组件挂载完成之后，要做某些事情
    useEffect(()=>{
      console.log("组件挂载完成之后");
      return ()=>{
        // 3-返回函数，即将卸载前要做某些事情，依赖数组要为空
        console.log("组件即将卸载");
      }
    },[]);
  ```

- 返回函数：

  - 当组件更新完成，或即将卸载时执行，一般返回函数用在即将卸载时

#### useRef

- 作用和用法类似于 createRef
- useRef 除了可以获取节点实例(真实DOM或组件实例) 之外，还可以跨组件更新阶段传递信息
- 当 ref 中保存的是数据时，组件更新之后，ref中保存的值，并不会跟随更新，需要我们手动更新

```JS
// 1-引入
import { useEffect, useRef, useState } from "react";
function Child() {
  const [count,setCount] = useState(1);//在组件中声明一个状态
  const [name,setName] = useState("小瑞瑞");

  // 2-赋值
  const app = useRef();

  // A- ref 中保存的是数据，count 初始值 赋值给 ref prevCount 在更新后 这个 count 不会更新
  const prevCount = useRef(count);

  useEffect(()=>{
    // B- 之前的值（初始值）不会更新
    console.log(prevCount);

    // 4-获取、使用 ref 
    console.log(app.current.innerHTML);

    // C- 手动更新
    prevCount.current = count;
  })
  // ref={app} 3- 生成、创建
  return ( <div 
    id="App"
    ref={app}
  >
      <input 
        type="text" 
        value={name}
        onChange={({target})=>{
           setName(target.value); 
        }}
      />
      <p>{name}</p>
      <p>{count}</p>
      <button onClick={()=>{
          setCount(count + 1);
      }}>递增</button>
  </div>);
}

export default Child;
```

### 自定义Hook 

- 自定义 hook 的命名必须以 use 开始

```JS
import {useEffect, useState} from "react";
// 1-定义自定义hook
function useScroll(){
   const [scroll,setScroll] = useState(0);
   useEffect(()=>{
     setScroll(window.scrollY);
     window.onscroll=()=>{
      setScroll(window.scrollY);
     };
     return ()=>{
      window.onscroll = null;
     }
   },[])
   return [scroll,(newScroll)=>{
      window.scrollTo(0,newScroll);
      setScroll(newScroll);
   }];
}

export {useScroll}
```

```JS
import { useEffect, useState } from "react";
import { useScroll } from "./hook";
function App() {
  // 2-引用自定义hooks
  const [scroll,setScroll] = useScroll();
  return ( <div>
      <div className="box">1</div>
      {/* 3-使用自定义hooks */}
      <span 
        className="affix"
        onClick={()=>{
          setScroll(0);
        }}
      >{scroll}</span>
  </div>);
}
export default App;

```

## props 和 state

- props 父组件传递过来的参数
- state 组件自身状态
  - setState
  - 多个 setState 合并

```JS
import { Component } from "react"
/*
  state 状态，组件会随着状态的改变，进行视图更新
  当状态需要修改时，可以调用组件的 setState 方法修改组件的状态

*/
class App extends Component {
  // 定义state
  state={
    count:1
  }
  render() {
    console.log("render");
    let {count} = this.state;
    return <div>
        <p>{count}</p>
        <button onClick={()=>{
          // 修改state
            this.setState({
              count: count+1
            })
        }}>递增</button>
    </div>
  }
}
export default App;
```

### 区别

- state 的主要作用是用于组件保存、控制、修改*自己*的可变状态，在组件内部进行初始化，也可以在组件内部进行修改，但是组件外部不能修改组件的 state
- props 的主要作用是让使用该组件的父组件可以传入参数来配置该组件，它是外部传进来的配置参数，组件内部无法控制也无法修改
- state 和 props 都可以决定组件的外观和显示状态。通常，props 做为不变数据或者初始化数据传递给组件，可变状态使用 state

## state 和 setState

setState(updater, [callback])  

- 修改state时，请使用 setState 方法
- updater: 更新数据 FUNCTION/OBJECT
- callback: 更新成功后的回调 FUNCTION
- setState 是一个异步方法：react通常会集齐一批需要更新的组件，然后一次性更新来保证渲染的性能，一个操作内多次调用 setState 会进行合并更新
- 浅合并 Object.assign()
  - setState 会对要修改的状态进行浅合并，所以只需要返回要修改的状态即可，但是如果状态中包含有引用类型的数据，只修改数据中的某一项，需要我们自己对数据进行合并
- 调用 setState 之后，会触发生命周期，重新渲染组件，setState --> 组件发生更新 --> render -->值改变
- 状态中包含有引用类型的数据，只修改数据中的某一项，需要我们自己对数据进行合并

```JS
<button onClick={()=>{
          this.setState(()=>{
            return {
              data:{
                // 先把原有的拿过来
                  ...data,
                  // 再把更新的覆盖原有的
                  count: count + 1
                }
            }
          })
      }}>报名</button>
```

## 组件间通信

在 React.js 中，数据是从上自下流动（传递）的，也就是一个父组件可以把它的 state / props 通过 props 传递给它的子组件，但是子组件不能修改 props - React.js 是单向数据流，如果子组件需要修改父组件状态（数据），是通过回调函数方式来完成的。

- 父级向子级通信
  把数据添加子组件的属性中，然后子组件中从props属性中，获取父级传递过来的数据
- 子级向父级通信
  在父级中定义相关的数据操作方法(或其他回调), 把该方法传递给子级，在子级中调用该方法父级传递消息     

### 跨组件通信 context - 扩展

```JS
// 创建context
import {createContext} from "react";
const context = createContext();

// 2个参数：消费者和提供者，这2个参数是context提供的2个组件
// Provider组件 用来向子孙后代传递消息
// Consumer组件 用来接收 Provider组件传递的消息
const {Provider,Consumer} = context;

// 导出
export default context;
export {Provider,Consumer}
```

```JS
// 引入context的组件
import { Component } from "react";
import Child from "./child";
import {Provider} from "./context";
class App extends Component {
  state={
    count: 1,
    price: 10
  }
  changeCount=(count)=>{
    this.setState({
      count
    })
  }
  changePrice=(price)=>{
    this.setState({
      price
    })
  }
  render(){
    const {count,price} = this.state;
    // 使用Provider把value传递给子孙后代
    return <Provider
      value={{
        count,
        price,
        changeCount:this.changeCount,
        changePrice:this.changePrice
      }}
    >
        <Child  />
    </Provider>
  }
}

export default App;
```

```JS
import { Component } from "react";
// 使用Consumer接收信息
import {Consumer} from "./context";
export default class SubChild extends Component {
  
  render(){
    // 使用 Consumer 接收 Provider 传递的信息
    // Consumer 接收一个函数，函数的参数是传递的context
    return <Consumer>
      {(context)=>{
          // 获取数据
          const {price,count,changeCount,changePrice} = context;
          // 返回的是这个组件要渲染的内容
          return <div>
        <p>价格：{price} --- 数量:{count}</p>
        <button onClick={()=>{
          changeCount(count + 1);
        }}>递增</button>
        <button onClick={()=>{
          changePrice(price + 10)
        }}>涨价</button>
    </div>
      }}
    </Consumer>
  }
}

```

## React生命周期

- 所谓的生命周期就是指某个事物从开始到结束的各个阶段，当然在 React.js 中指的是组件从创建到销毁的过程，React.js 在这个过程中的不同阶段调用的函数，通过这些函数，我们可以更加精确的对组件进行控制，前面我们一直在使用的 render 函数其实就是组件生命周期渲染阶段执行的函数
- 挂载阶段，更新阶段，写在卸载阶段

### 挂载阶段 - mount

- 组件创建-->把组件创建的虚拟DOM，生成真实DOM，添加到我们的DOM树中，将组件初始化，并且渲染到DOM中

- constructor 构造函数 最先执行 - 拿到参数props
- static  getDerivedStateFromProps(props) - 从属性获取派生状态（当接收到新的属性想去修改 state的时候使用），props关联入state
  - 注意 this 问题
- render - 生成虚拟DOM
- componentDidMount - 组件挂载完成: 异步请求数据，获取真实DOM节点

```JS
import { Component } from "react";
export default class Child extends Component {
    
  // 构造函数constructor拿到参数props
  constructor(props){
      super(props);
      
      // this是组件实例
      this.state = {
        state:"11"
      };
      console.log("挂载第一步 - 初始化组件");
  }
    
  // getDerivedStateFromProps 从属性获取派生状态
  static getDerivedStateFromProps(props){
      //将 props 中的数据关联到 state 中
      console.log("2 - 将props关联入state")
      //console.log(this); 静态方法没有this
    
      return props;
  }
    
  componentDidMount(){
    // 组件挂载完成: 执行副作用(异步操作,获取真实DOM节点)
    console.log("4-已经将 render 构建的虚拟DOM，生成真实DOM，并放入DOM树中");
    //console.log(document.querySelector("#box"));
  }
    
  render(){
    const {price,count,changeCount,changePrice} = this.props;
    //console.log(this.state);
    console.log("3 - 生成虚拟DOM");
    return <div id="box">
      <p>价格：{price} --- 数量:{count}</p>
      <button onClick={()=>{
        changeCount(count + 1);
      }}>递增</button>
      <button onClick={()=>{
        changePrice(price + 10)
      }}>涨价</button>
    </div>
  }
}
```

### 更新阶段 - update 

- update 组件发生更新重新渲染，并且生成新的虚拟DOM，并根据新的虚拟DOM，完成真实DOM的修改
- static getDerivedStateFromProps(props, state) - 将props关联入state
   - shouldComponentUpdate()  -- 判断是否跟新，返回值 为 true 则继续执行组件更新，并更新视图
   - render() - 生成虚拟DOM
   - getSnapshotBeforeUpdate(prevProps,prevState) - 参数是之前的属性和state，返回值会作为第三个参数传给componentDidUpdate, 用于获取更新前的 dom 快照

- componentDidUpdate(prevProps,prevState,snapshot) 参数是之前的属性和state以及snapshot（就是getSnapshotBeforeUpdate的返回值） -- 组件更新完成，异步请求数据

```JS
import { Component } from "react";
export default class Child extends Component {
  constructor(props){
      super(props);
      this.state = {
        state:"11"
      };
  }
    
  static getDerivedStateFromProps(props){
      console.log("1 - 将props关联入state")
      return props;
  }
    
  shouldComponentUpdate(nextProps,nextState){
    // 是否更新组件视图,注意在该生命周期函数中，state 和 props 还未更改，如果要获取更新后的state 和 props 请使用 nextProps,nextState
    console.log("2 - 判断组件是否需要更新");
    return true; // 返回值 为 true 则继续执行组件更新，并更新视图，为 false 则不继续执行生命周期，不更新视图，做优化，react父组件更新会导致子组件全部更新，不希望子组件更新，这里判断子组件是否需要更，返回true、false，提升性能
  }
    
  getSnapshotBeforeUpdate(prevProps,prevState){
    // 已经修改 state 和 props，并生成了新的虚拟DOM，即将修改真实DOM ，如果需要获取更新前的 props 和 state 需要通过参数 prevProps,prevState 获取
    // 用于获取更新前的 dom 快照
    console.log("4-获取更新前的DOM快照");
    return document.querySelector("#box").innerHTML; // 该返回值会传递给 componentDidUpdate
  }
  
  componentDidUpdate(prevProps,prevState,prevDOM){
    //console.log(prevDOM);
    console.log("5 - 组件更新完成")
  }
  
  render(){
    const {price,count,changeCount,changePrice} = this.props;
    console.log("3 - 生成虚拟DOM");
    return <div id="box">
      <p>价格：{price} --- 数量:{count}</p>
      <button onClick={()=>{
        changeCount(count + 1);
      }}>递增</button>
      <button onClick={()=>{
        changePrice(price + 10)
      }}>涨价</button>
    </div>
  }
}
```

### 卸载阶段 - unMount 

- 将组件从真实DOM中删除
- componentWillUnmount  - 组件即将卸载
- 删除添加在全局的一些信息或操作

```JS
import { Component } from "react";
export default class Child extends Component {
  componentDidMount(){
    let info = document.querySelector("#info");
    info.innerHTML = window.innerWidth;
 
  // 添加事件
    window.onresize=function(){
      let info = document.querySelector("#info");
      info.innerHTML = window.innerWidth;
    };
  }
    
  // 卸载阶段 ，删除事件
  componentWillUnmount(){
    window.onresize = null;
  }
```

## 受控组件 - 非受控组件

- 当想要获取表单的一些内部状态时，就可以将表单的内部状态和组件的状态进行绑定，这样就形成受控组件
- 受控组件: 让 表单控件 的内部状态  和我们 state 保持一致
  - setState
- 非受控组件: 我们不需要同步 value 值(defaultValue，defaultChecked)，只需要表单的初始值和我们状态一致
  - defaultValue

```JS
import {Fragment,Component } from "react";
class App extends Component {
  // 定义状态
  state={
    val: "",
    checked:false,
    val2: "初始值"
  }
  render() {
    const {val,checked,val2} = this.state;
    return <Fragment>
        <input 
          type="text" 
          value={val}
          // target 事件源
          onChange={({target})=>{
              // 修改状态，同步状态，受控组件
              this.setState({
                val:target.value
              })
          }}
        />
        <br/>
        <input 
          type="checkbox" 
          checked={checked}
          onChange={({target})=>{
            // 修改状态，同步状态，受控组件
            this.setState({
              checked:target.checked
            })
          }} 
        />
        <br/>
        <input 
          type="text" 
          // defaultValue，非受控组件 
          defaultValue={val2}
        />
        <p>{val}</p> 
        <button onClick={()=>{
          // val,checked 受控组件 、 val2 非受控组件
          console.log(val,checked,val2);
        }}>提交</button>
    </Fragment> 
  }
}
export default App;
```

## key 的问题

- 在 React ，组件每次更新时，会生成一个 虚拟DOM，和原有的虚拟DOM进行对比。
- 如果是批量生成的一组元素，那React就会根据 key 值去做对比
- 同一个列表中 key 唯一（key 不能重复）
- 组件更新前后，key 要保持不变
- 通过 key 来判断更新前后是否是同一个元素
- 组件更新后
  - 生成新的虚拟DOM
  - 对比新旧虚拟DOM，找出不一样的点
  - 对不一样点进行真实的DOM更新

## PureComponent

- PureComponent 提供了一个具有浅比较的 `shouldComponentUpdate` 方法，当`props`或者`state`改变时，`PureComponent`将对`props`和`state`进行浅比较
- 其他和 Component 完全一直,Component 不会比较，因此，每当Component 的`shouldComponentUpdate`被调用时，组件默认的会重新渲染。
- 在 React 中，父组件更新，会引起所有的子组件一块更新。
- shouldComponentUpdate - 判断子组件是否需要更新

```JS
// 手动判断
class Todo extends Component {
  /*
      在 React 中，父组件更新，会引起所有的子组件一块更新。
  */
  shouldComponentUpdate(nextProps,nextState){
    // console.log("重新渲染了",nextProps.data.id);
    // 判断之前的状态和现在的状态不一致时再更新
    if(
      this.props.data.done !== nextProps.data.done
      || this.props.data.todo !== nextProps.data.todo
    ){
      return true;
    }
    // 前后一致不更新
    return false;
  }
```

```JS
// PureComponent 自动判断，注意前后的引用类型
import { createRef, PureComponent } from "react";
/*
  PureComponent 功能和 Component 完全一致，只是在组件更新时会进行浅对比，浅对比如果 props 或 state 没有修改则 不进行组件更新，状态为引用类型返回也要是引用类型
*/
class Todo extends PureComponent {
    ...
  }
```

## ref

- createRef()
- 注意：在 组件挂载完成之后及更新之后使用
- Ref 用于获取 ReactNodes 的实例：用在组件上则获取 组件 实例，用于节点上则获取 DOM实例

```JS
// 1-引入
import { createRef, PureComponent } from "react";

class Todo extends PureComponent {
  constructor(props){
    super(props);
    this.state = {
      isEdit: false,
      editVal: props.data.todo // 将 todo 的内容备份一份
    }
  }
    
  // 2-赋值ref对象
  editText = createRef();

  componentDidUpdate(prevProps,prevState){
    // 判断是否是刚进入编辑状态 上一次为 false 当前次为 true 代表进入编辑状态
    if((!prevState.isEdit)
      &&this.state.isEdit){
        
        // 4- 使用ref生成的实例
        this.editText.current.focus();
        
    }
  }
  render(){
    const {data,removeTodo,changeDone,editTodo} = this.props;
    const {id,done,todo} = data;
    const { isEdit, editVal } = this.state;
    return <li className={isEdit?"editing":""}>
      <div className={"todo "+(done?"done":"")}>
        <div className="display">
          <input 
            className="check" 
            type="checkbox" 
            checked={done}
            onChange={({target})=>{
                changeDone(id,target.checked);
            }}
          />
          <div 
            className="todo-content"
            // 双击
            onDoubleClick={()=>{
              this.setState({
                isEdit: true
              })
            }}
          >{todo}</div>
          <span className="todo-destroy"
            onClick={()=>{
              removeTodo(id);
            }}
          ></span>
        </div>
        <div className="edit">
          <input 
            className="todo-input" 
            type="text" 
            value={editVal}
            onChange={({target})=>{
             // console.log(target.value);
                //editTodo(id,target.value);
                // 编辑时，不修改原有的todo 而是修改复制出来的
                this.setState({
                  editVal:target.value
                })
                
            }}
            
      // 3-ref 生成，创建，想获取谁的实例就在谁里面用ref生成实例
      // 添加 ref ，再使用 ref 对象 来 生成这个 div  的实例
            ref={this.editText}

            // 失去焦点
            onBlur={()=>{
              // 退出编辑时，判断当前的修改是否为空
              if(editVal.trim()){
                // 如果不为空，则 修改 todo
                  editTodo(id,editVal);
              } else {
                // 如果为空，则将 editTodo 恢复为 todo
                this.setState({
                  editVal:todo
                })
              }

              this.setState({
                isEdit: false
              })
            }}
          />
        </div>
      </div>
    </li>
  }
  
}

export default Todo;
```

## children

- children: 调用子组件时，如果在标签对之间，填写内容，该内容会加给 props 的 children 属性
- 可以自定义结构的组件的常用形式
  - children
  - 传递函数
  - 传递子组件

### dangerouslySetInnerHTML

- 直接设置标签的 innerHTML
- 用于给 React 元素添加 innerHTML
- 接收的值是一个对象，该对象的 `__html` 属性中，是放 innerHTML 的地方

```JS
// 子组件
import {Component} from "react";
export default class Child extends Component {
  render(){
    // 这里接收
    const {children} = this.props;
    //console.log(this.props);
    return <div>
        <h1>child</h1>
        {/* 这里使用 */}
        {children}
    </div>
  }
}
```

```JS
import { Component } from "react";
import Child from "./Child";
const data = `
  <h2>又又想造反</h2>
  <p>今天晚上又又罢工了</p>
`;
export default class App extends Component {
  render() {
    return <div>
      {/* 子组件 */}
        <Child>
          {/* div中直接写data会把标签也显示出来，dangerouslySetInnerHTML显示内容 */}
            <div
              dangerouslySetInnerHTML = {{
                __html:data
              }}
            ></div>
        </Child>
    </div>
  }
}
```

## React Router

- 路由：根据不同的url规则，给用户展示不同的视图(页面)

### SPA

Single Page Application : 单页面应用，整个应用只加载一个页面（入口页面），后续在与用户的交互过程中，通过 DOM 操作在这个单页上动态生成结构和内容

**优点：**

- 有更好的用户体验（减少请求和渲染和页面跳转产生的等待与空白），页面切换快
- 重前端，数据和页面内容由异步请求（AJAX）+ DOM 操作来完成，前端处理更多的业务逻辑

**缺点：**

- 首次进入处理慢
- 不利于 SEO

### SPA 的页面切换机制

-  SPA 的内容都是在一个页面通过 JavaScript 动态处理的

- 但是还是需要根据需求在不同的情况下分内容展示
- 如果仅仅只是依靠 JavaScript 内部机制去判断，逻辑会变得过于复杂
- 通过把 JavaScript 与 URL 进行结合的方式：JavaScript 根据 URL  的变化，来处理不同的逻辑，交互过程中只需要改变 URL 即可。
- 这样把不同 URL 与 JavaScript 对应的逻辑进行关联的方式就是路由

/*
    http//:www.baidu.com:80/search#hans?name=lll&age=8
    : 后 端口

  `#`   后 hash

​    ？后 search
*/

### 前端路由

- 前端路由只是改变了 URL 或 URL 中的某一部分，
- 但一定不会直接发送请求，可以认为仅仅只是改变了浏览器地址栏上的 URL 而已，
- JavaScript 通过各种手段处理这种 URL 的变化，然后通过 DOM 操作动态的改变当前页面的结构

- URL 的变化不会直接发送 HTTP 请求
- 业务逻辑由前端 JavaScript 来完成

#### 目前前端路由主要的模式：

- 配置整个项目，使用的路由模式:Router是路由模式
  -  HashRouter 基于 hash 的路由模式，有#
  -  BrowserRouter 基于 History 的路由模式

```JS
import React from 'react';
import ReactDOM from 'react-dom';
// 引入路由
import {BrowserRouter} from "react-router-dom";
import App from './App';
/*
  配置整个项目，使用的路由模式:Router是路由模式
    HashRouter 基于 hash 的路由模式，有#
    BrowserRouter 基于 History 的路由模式
*/
ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
);

```

### 基于 Web 的 React Router

- 基于 web 的 React Router 为：react-router-dom

### Route 组件

- path 要匹配的路由
- 注意 ReactRouter 默认是模糊匹配，及 url 是以当前 path 为开始时则匹配成功
- exact 精确匹配，url 必须和 当前path 一致时匹配，如 path="/about"，url 为 "/about" 和 "/about/" 会匹配成功
-  strict 严格匹配，严格匹配基于精确匹配，如 path="/about"，url 为 "/about" 会匹配成功
- 多 url 匹配,[path1,path2,path3……]
- component 路径匹配成功之后要显示的视图
- 当 path 为空或 path = ""，则代表所有路径都匹配

```JS
import JoinPage from "./view/join/index";
import AboutPage from "./view/about/index";
import Nav from "./component/nav";
const { Route } = require("react-router-dom");
const { default: IndexPage } = require("./view/index");

function App() {
  return (
    <div className="App">
        <Nav />
        <hr />
      
      	// 路由匹配
        <Route path={["/","/index","/home"]} exact component={IndexPage} />
        <Route path="/join" component={JoinPage} />
        <Route path="/about" component={AboutPage} />
            
    </div>
  );
}
export default App;
```

### Link 组件

- Link 组件用来处理 a 链接 类似的功能（它会在页面中生成一个 a 标签）
- react-router-dom 拦截了实际 a 标签的默认动作
- 然后根据所有使用的路由模式（Hash 或者 HTML5）来进行处理
- 改变了 URL，但不会发生请求，同时根据 Route 中的设置把对应的组件显示在指定的位置

#### to 属性

- to 属性类似 a 标签中的 href

```JS
import { Link } from "react-router-dom";

// a标签会刷新页面
// function Nav() {
//     return <nav>
//         <a href="/">首页</a>
//         <span> | </span>
//         <a href="/join">加入我们</a>
//         <span> | </span>
//         <a href="/about">关于我们</a>
//         <span> | </span>
//         <a href="/about/details">加入我们详情</a>
//     </nav>
// }


/*
  Link 组件 用于（单页应用）项目内视图的跳转，不会刷新页面，不能跳转外部
*/
function Nav() {
  return <nav>
      <Link to="/">首页</Link>
      <span> | </span>
      <Link to="/join">加入我们</Link>
      <span> | </span>
      <Link to="/about">关于我们</Link>
      <span> | </span>
      <Link to="/about/details">加入我们详情</Link>
      <span> | </span>
      {/* <Link to="https://www.baidu.com">百度</Link> */}
      <a href="https://www.baidu.com" target="_blank">百度</a>
  </nav>
}

export default Nav;
```

### React中图片的引入方式

- 基于 http|https 的绝对路径
- require 相对路径 (CRA 4 之前 require("路径")，CRA 4 require("路径").default)
- import 相对路径

```JS
// 2.0 import Img from "./img4.png";

function IndexPage() {
    return <div>
        <h1>首页视图</h1>
        {/* 1 */}
        <img 
          src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1605373886202&di=a3edadac6ac25483b330f4272ca6afab&imgtype=0&src=http%3A%2F%2Fattach.bbs.miui.com%2Fforum%2F201308%2F23%2F220651x9b0h4kru904ozre.jpg" 
          width={400}
        />
              
        {/* 2.1 */}
        {/* <img src={Img} width={400} />
        
        3
        <img src={require("./img4.png").default} width={400} /> */}
    </div>
}

export default IndexPage;
```

### 路由守卫：

- 在页面跳转的时候去判断有没有登录

### 路由组件传递 props

#### component 

```jsx
<Route exact path='/' component={Home}
```

- 如果 Route 使用的是 component 来指定组件
- component 的路由参数直接在子组件props

#### Route : render

```jsx
    <Route exact path='/' render={(props) => <Home items={this.state.items} />} />
```

- render 接收一个回调函数，回调函数的返回值是其要渲染的视图
- 通过 render 属性来指定渲染函数，render 属性值是一个函数，当路由匹配的时候指定该函数进行渲染
- 路由参数是render函数的props，子组件才有路由信息props

```JS
function IndexPage(props) {
  //console.log(props); 接收父组件传递过来的数据
  return <h1>首页视图</h1>
}
export default IndexPage;
```

### NavLink 组件

- NavLink 与 Link 类似，都是提供了一个视图跳转链接
- NavLink 可以给当前选中项，添加选中效果
- NavLink 在匹配当前项时，默认也是 模糊匹配，精确加exact
- 但是它提供了两个特殊属性用来处理页面导航：

#### activeStyle

- 当当前 URL 与 NavLink 中的 to 匹配的时候，激活 activeStyle 中的样式
- 同 activeClassName

#### activeClassName

- 当前选中项的class，默认为 active
- 修改默认要自定义class，再写好样式，activeClassName="样式名calss"

#### isActive

- isActive 接收一个回调函数，通过该回调函数的返回值，决定项当前要不要选中
- 返回值 为 true 则 一直选中当前项，false 一直不选中

```JS
const { NavLink } = require("react-router-dom");

const activeStyle = {
  // 文字加粗
    fontWeight: "bold"
}

function Nav() {
  return <nav className="nav">
      <NavLink 
        to="/"
        exact
        activeClassName="selected"
        activeStyle={activeStyle}
      >首页</NavLink>
  </nav>
}
export default Nav;
```

#### Switch 组件

- 该组件只会渲染首个被匹配的组件
- 当其中一项匹配成功之后，则不在向下匹配

#### Redirect 组件 重定向 

- from 当前 url 
- to 跳转到 url
- 当 Route render 的返回值 为 Redirect 时直接跳转

### 路由组件

- 路由组件：
  - 当组件是被 Route 的 Component 调用时，该组件就会被称之为 路由组件
  - 路由组件在被调用，Route 会传递给该组件相关路由信息，该信息被称之路由参数

```JS
const { Route, Switch, Redirect } = require("react-router-dom");

function App() {
  const user = "小瑞瑞";
  return (
    <div className="wrap">
      
      {/* 引入导航，实现页面导航 */}
      <Nav />
          
      <Switch>
          
      {/* render向子组件传递数据 */}
        <Route path="/" exact render={(props)=>{
          return <IndexPage 
              {...props}
              user={user}
          />
        }} />
		{/* 重定向 */}
        <Redirect from="/index" to="/" />

        {/* component的路由参数直接在子组件props  */}
        <Route path="/about" exact component={AboutPage} />
        <Route path="/join" exact component={JoinPage} />

        {/* render的路由参数，在这里的props  */}
            /*
            list 的 path 规则:
            /list
            /list/分类
            /list/分类/页码
            */
        <Route path={["/list","/list/:type","/list/:type/:page"]} exact render={(props)=>{
          
          // 获取动态路由参数
          const {match} = props;
          // 添加默认值是字符串
          const {type="good",page="1"} = match.params;
          // 规定3个子路由
          // + "" 是转成字符串 再和page本身对比
          if(["good","share","ask"].includes(type)
            && parseInt(page) + "" === page 
            && page>0
          ){
            return <ListPage />
          }
          return <Redirect to="/404" />
        }}/>
        {/* 借助Switch 不写path 导航404页面 */}
        <Route component={Page404} />
      </Switch>
    </div>
  );
}
export default App;
```

- location 对象
  -  hash: "" 	当前 url 中的 hash 值
  -  pathname: "/join"      当前 url 地址
  - search: ""      当前 url 中的 search 值
  - state: null      上一步跳转传递的参数 
- history 对象
  - go: go(n) 跳转 n 步历史记录
  - goBack: ƒ goBack() 返回历史记录上一步
  - orward: () 前进历史记录下一步 
  - length 当前源在历史记录中记录了多少条
  - push(url,state) 跳转视图

- match 对象
  - isExact: true 是否是精确匹配
  - params: {} 动态路由的参数
  - path: "/about" 当前 Route 的 path
  - url: "/about" 当前浏览器的 Url
  - 动态路由: 路由中某一段是不固定的
    - path = "/list/:type"

```JS
function JoinPage(props) {
  //console.log(props);  路由参数
  const {history} = props;
  //console.log(history.length);
  return <Fragment>
      <h1>加入视图</h1>
      <button onClick={()=>{
        history.goForward();
      }}>前进</button>
      <button onClick={()=>{
        history.goBack();
      }}>后退</button>
      <button onClick={()=>{
        history.go(-3);
      }}>跳转多项</button>

      {/* 1-第二个参数是传递给要跳转到的页面的数据 */}
      <button onClick={()=>{
        history.push("/about",{
          info:"加入传递的信息"
        });
      }}>关于</button>
  </Fragment>
}
export default JoinPage;
```

```JS
function AboutPage(props) {
  //console.log(props.location.state);  2-上一步跳转传递的参数
  return <h1>关于视图</h1>
}
export default AboutPage;
```

### 非路由组件获取路由参数的2种方式：

#### withRouter ：高阶路由（高阶组件）

- withRouter 接收一个组件，并返回一个新的组件，函数组件和类组件都可以用

- 如果一个组件不是路由绑定组件，那么该组件的 props 中是没有路由相关对象的，
- 虽然我们可以通过传参的方式传入，但是如果结构复杂，这样做会特别的繁琐。
- 幸好，我们可以通过 withRouter 方法来注入路由对象

```JS
 const newListNav = withRouter(ListNav);

 export default newListNav;

// export default withRouter(ListNav);  简洁写法

// 调用newListNav接收了route的context传递下来的信息，接收后，再调用了ListNav，并且把信息传递给ListNav，就是ListNav的props等于路由信息
```

#### Router hooks

- 只能用在函数组件上

- useHistory  返回 history 对象
- useLocation  返回 location 对象
- useParams   返回动态路由参数
- useRouteMatch   返回 match 对象

## Redux

### 使用规则：

- 只有一个store，
- state是只读的，修改state通过dispatch发起action，
- 在纯函数中完成对state的修改
- 用户通过 View 发出 Action ，调用 dispatch，然后 Store 自动调用 reducer ，并且传入 2 个参数，当前state 和 收到的action，reducer返回新的state，state一旦变化，store就会调用监听函数，更新 view

### store 

store 仓库 - 提供获取状态和发起状态修改命令，监听状态修改的方法

- 创建 store 仓库，在这里定义 reducer 和 action ，

- 在需要修改数据的地方，发起dispatch ，定义action的type，以及action的type需要的数据或方法，完成修改

- ```JS
  // 创建 store 仓库
  import {createStore} from "redux";
  // reducer函数接收action
  const reducer = (state={
    count: 1,
    name: "开课吧"
  },action)=>{
    switch (action.type) {
      case "PLUS":
        return {
          ...state,
          count: state.count + 1,
        }
    }
    return state;
  };
  // 创建 store
  const store = createStore(reducer);
  export default store;
  ```

- dispatch(action)  发起一个状态修改指令，同步方法

- getState() 获取状态

- replaceReducer(nextReducer) 替换掉 reducer 函数 

- subscribe:  subcaribe(listen) 监听以及取消监听状态的变化，每次状态更改就会触发监听

  - ```JS
    // 监听 state 更新
    const unsubscribe = store.subscribe(() =>
      console.log(store.getState())
    )
    // 停止监听 state 更新
    unsubscribe();
    ```

#### createStore

```JS
// 创建 store
const store = createStore(reducer);
```

```JS
// 1- applyMiddleware异步程序中间件，在这添加中间件
// 2- thunk 异步action中间件，实现异步action
const store = createStore(combineReducers({
  count,
  todos,
  info
}),applyMiddleware(thunk));
```

#### dispatch

- dispatch(action)  发起一个状态修改指令，同步方法

  - dispatch 接收的是一个 action

  - dispatch  发起状态修改指令(action)，store 会 action 传递给 reducer，reducer 接收 action，根据 action 的 type 返回对应的新 state ，完成对 state 的修改

  - 在 store index.js 创建 store 仓库，定义 reducer 和 action ，在需要修改数据的地方，发起dispatch ，定义action的type，以及action的type需要的数据或方法，完成修改

  - ```JS
    // 需要修改数据的地方
    import { useDispatch } from "react-redux"
    import TodoList from "./todoList"
    
    function Todos() {
        const dispatch = useDispatch();
        return <div>
            <TodoList />
            <button onClick={()=>{
                dispatch({
                  // 1-这里就是action，会传递给reducer函数
                  type:"TODOS_PLUS",
                  newTodo:{
                    id: Date.now(),
                    todo: "这是新的todo" +  Date.now()
                  }
                })
            }}>添加todo</button>
        </div>
    }
    export default Todos
    ```

  - ```JS
    //  store 仓库
    import {createStore, combineReducers, applyMiddleware} from "redux";
    // count todos info 都是 reducer 函数
    const count = (count = 1,action)=>{
        switch (action.type) {
            case "COUNT_PLUS":
              return count + 1
          }
        return count;
    };
    // 在不同 reducer 函数中，action.type 不能重名 
    // 3-这里才能用action中的数据、方法
    const todos = (todos = [],action)=>{
        switch (action.type) {
          case "TODOS_PLUS":
            return [
              ...todos,
              action.newTodo
            ]
          case "TODOS_REMOVE":
              return todos.filter(item=>item.id!==action.id);
        }
        return todos;
    };
    const info = (info = {
      loading: true,
      info: ""
    },action)=>{
      switch (action.type) {
        case "INFO_LOAD":
          return {
            loading: false,
            info: action.info
          }
        case "INFO_LOADING":
          return {
            loading: true,
            info: ""
          }
      }
      return info;
    };
    
    // 2-这里是合并reducer函数，reducer函数会接收action
    const store = createStore(combineReducers({
      count,
      todos,
      info
    }));
    export default store;
    ```

### state

- 状态、数据

### reducer 纯函数

reducer 纯函数 - 提供修改状态的方法，reducer 就是一个纯函数，接收旧的 state 和 action，返回新的 state。

- ```JS
  (previousState, action) => newState
  ```

- ```JS
  // 纯函数
  const reducer = (state={
    count: 1
  },action)=>{
    switch (action.type) {
      case "PLUS":
        return {
          count: state.count + 1
        }
    }
    return state;
  };
  ```

- 纯函数

  - 相同的输入永远返回相同的输出，state 和 action 一样时返回的新值永远是一样的
  - 不修改函数的输入值，不修改原有的state，返还新的state
  - 不依赖外部环境状态，用的是它自己的参数state action
  - 无任何副作用（不能在纯函数中 修改DOM 发起请求 定时器 等等）
    - 使用纯函数的好处
      - 便于测试
      - 有利重构

### action

- action 状态修改指令 
- 默认是一个普通对象
  - type 属性 : type 属性是对 state 做什么样修改的 描述，是一个描述
  - payload属性 : 操作 state 的同时传入的数据
  
- 需要注意的是，我们不直接去调用 Reducer 函数，而是通过 Store 对象提供的 dispatch 方法来调用 

## react-redux 

- react项目中的 redux 绑定库,将 redux 引入 react 项目的绑定库

- 安装：npm i react-redux

```JS
import ReactDOM from "react-dom";
import App from "./App";
import {Provider} from "react-redux";
import store from "./store";
/*
    react-redux: 将 redux 引入 react 项目的绑定库
*/

ReactDOM.render(
  // Provider把信息store传给子孙后代
  <Provider store={store}>
    <App />
  </Provider>,
  document.querySelector("#root")
);
```

#### connect()

- 在组件中获取 state 和 dispatch

```JS
import {connect} from "react-redux";
/*
  在组件中 获取 state 和 dispatch：
  1. connect （类组件和函数组件）
*/
function Count(props) {
  console.log(props);
  return <div>
      
  </div>
}

// connect 接收一个回调函数
// 获取到全部 state 之后，从这些 state 中，提取我们想要的部分
const NewCount = connect(state=>{  
  console.log(state);  // 全部的
  // connect 的返回值，必须返回一个对象
  return {
    count: state.count  // 提取我们想要的
  } 
}); 

// connect 接收了 Provider 传递下来的数据，找到我们想要的，并返回一个高阶组件NewCount。调用 connect 的返回值NewCount，并把我们要使用 state 的组件Count传给这个高阶组件NewCount，会再返回一个新组件NewCount2，调用NewCount2组件会在内部调用这个传进来的组件Count，并且将 connect 节选出 state 和 dispatch 方法 传递给组件Count，在组件Count的props中拿到传递过来的数据

const NewCount2 = NewCount(Count);
export default NewCount2;

// 简洁写法
export default connect(state=>({count:state.count}))(Count);
```

#### HOOKS

- 在组件中获取 state 和 dispatch

- useDispatch：获取 store 的 dispatch 方法
- useSelector：从 state 获取想要的数据，接收回调函数，返还想要的
- useStore ：获取 store 对象

```JS
import {useDispatch, useSelector, useStore} from "react-redux";
function Count() {

    const count = useSelector(state=>state.count);
    
    const dispatch = useDispatch();

    console.log(useStore());
    
    return <div>
        <p>{count}</p>
        <button onClick={()=>{
            dispatch({
                type: "PLUS"
            });
        }}>递增</button>
    </div>
}
export default Count;
```

#### reducer 拆分、合并

- 先拆分

```JS
// reducer 拆分
const reducer = (state={},action)=>{
  return {
    count: count(state.count,action),
    todos: todos(state.todos,action)
  };
};

const count = (count = 1,action)=>{
    switch (action.type) {
        case "PLUS":
          return count + 1
      }
    return count;
};

const todos = (todos = [],action)=>{
    return todos;
};
```

- 后合并 - combineReducers

```JS
const count = (count = 1,action)=>{
    switch (action.type) {
        case "PLUS":
          return count + 1
      }
    return count;
};

const todos = (todos = [],action)=>{
    return todos;
};

// reducer 的合并
const reducer = combineReducers({
  count: count,
  todos: todos
});
```

### 中间件

- 更新的过程中，去做一些其他的事情
- dispatch ---> reducer 更新state
- dispatch --> 中间件 --> reducer

#### redux-thunk  异步中间件

- redux 使用了 thunk 中间件后，dispatch 可以接收两种不同类型的参数（action）
  -  普通对象，该 action 还是跟之前一样，调用 dispatch 会直接 调用 reducer 函数，将 action 传递给 redux 修改 state
    - 对象形式action： dispatch --> reducer 
  - 函数：当 action 是个函数时， dispatch 会 执行该函数，并且将 getState 和 dispatch 传递给该函数
    - 函数形式action： dispatch --> 函数(中间件)(完成异步操作后) --> 再次dispatch --> reducer或者下一个中间件

```JS
store.js 创建 store
import thunk from "redux-thunk"; // 实现异步action

// 1- applyMiddleware异步程序中间件，在这添加中间件
// 2- thunk 异步action中间件，实现异步action
const store = createStore(combineReducers({
  count,
  todos,
  info
}),applyMiddleware(thunk));
```

```JS
action.js  写异步 action
// 3-异步action 
function getInfo(dispatch) {
  dispatch({
    type: "INFO_LOADING"
  })
  fetch("https://api.apiopen.top/getTangPoetry?page=1&count=20")
    // 请求中
    .then(res=>{
      return res.json();
    })
    .then(res=>{
      // 请求完成得到数据，请求之后
      dispatch({
        type: "INFO_LOAD",
        info: res.result
      })
    })
}

export {getInfo};
```

```JS
info.js 请求异步数据的文件

import { useEffect } from "react";import { useDispatch, useSelector } from "react-redux";

// A - 引入 action
import { getInfo } from "./action";

function Info() {
  const info = useSelector(state=>state.info);
  const dispatch = useDispatch();
  // 4-异步actionS，挂载后
  useEffect(()=>{
    // B - dispatch 发起 引入的 action，这里没有用到getState
    dispatch(getInfo);
    /*	异步使用方法
        dispatch(function(dispatch,getState){
          console.log("异步action")
        })
    */ 
  },[]);
  return <h2>{info.loading?"数据请求中……":info.info}</h2>
}

export default Info;
```

#### hooks实现异步请求数据

```JS
action.js
import { useDispatch } from "react-redux";
// 1-hooks实现异步请求数据
function useInfoData() {
  const dispatch = useDispatch();
  return ()=>{
    dispatch({
      type: "INFO_LOADING"
    })
    fetch("https://api.apiopen.top/getTangPoetry?page=1&count=20")
      .then(res=>{
        return res.json();
      })
      .then(res=>{
        dispatch({
          type: "INFO_LOAD",
          info: res.result
        })
      })
  }
}

export {useInfoData};
```

```JS
info.js
import { useEffect } from "react";
import { useSelector } from "react-redux";
import { useInfoData } from "./action";

function Info() {
  const info = useSelector(state=>state.info);
    
  // getInfoData 是 useInfoData 中 return 的函数，异步请求函数
  const getInfoData = useInfoData();
  
  // 挂载后
  useEffect(()=>{
    // 2-调用,拿到请求的info，替换上面的info，得到新请求的info
    getInfoData();
  },[]);
  // 根据 info的属性 判断请求前后，显示对应的视图
  return <h2>{info.loading?"数据请求中……":info.info}</h2>
}
export default Info;
```

## React 项目

### CSS Modules

- 在 create-react-app 中的使用：
  - .css  .sass 正常文件
  - [name].module.css  [name].module.sass 需要模块化的文件

- CSS Modules 使用
  - 局部 声明:local(.className) .className 使用 styled.className 
  - 全局 :global(.className)
  - composes 引入其他样式

```JS
// 局部
.box {
  color: green;
}
.div {
  composes: box;
  width: 100px;
  height: 100px;
  background: green;
}

/* 与上面局部等价*/
:local(.box) {
   color: green;
}

/* 定义全局样式*/
:global(.btn) {
   color: red;
}
```

- 文件名字加 module 、引入、使用

```JS
// 引入
import style from "./app.module.css";
const { default: Child } = require("./child");
console.log(style);
function App() {
    return <div>
        {/* 使用 */}
        <div className={style.box}>
            App
        </div>
        <div className={style.div}>
            <div></div>
        </div>
        <Child />
    </div>
}
export default App;

```

### Ant Design

- 最基本使用
  - 安装 npm install antd
  - 引入组件和样式
    import { DatePicker } from 'antd';
    import 'antd/dist/antd.css';

###  CNode 项目

- public
  - index.html 页面挂载点
- .gitignore  git管理文件
- package.json  项目描述文件

- src
  - index.js 入口文件（配置项目环境、路由、redux）
  - App.js 根组件
  - static 静态文件 (图片、css)
  - component 通用组件
  - view 各个视图
    - 视图1
      index.js
      视图的私有组件
    - 视图2
  - routers 路由相关配置（视图view放在routers下或单独拿出来）
  - store 仓库-全部状态管理
    - reducer 
    - action
  -  hooks 公共hooks方法
- 配置入口文件

```JS
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import {BrowserRouter} from "react-router-dom";
import { Provider } from "react-redux";
import store from "./store/index";
// 引入 antd 的样式sss
import "antd/dist/antd.css";
// 样式放在antd后面，覆盖antd样式
import "./static/css/index.css";
ReactDOM.render(
  // 路由配置
  <BrowserRouter>
  {/* store  */}
      <Provider store={store}>
        {/* 根组件 */}
          <App />
      </Provider>
  </BrowserRouter>,
  document.getElementById('root')
);
```

- 配置路由

```JS
// 路由配置表 routes  的 config.js
import IndexPage from "./index/index";
import APIPage from "./api/index";
import AboutPage from "./about/index";
import UndefinedPage from "./404/index";
const routes = [
  {
    path: "/",
    exact: true,
    // 传递路由参数
    render(props) {
      return <IndexPage {...props} />
    }
  }, {
    path: "/api",
    exact: true,
    render(props) {
      return <APIPage {...props} />
    }
  }, {
    path: "/about",
    exact: true,
    render(props) {
      return <AboutPage {...props} />
    }
  }, {
    path: "",
    render(props) {
      return <UndefinedPage {...props} />
    }
  }
];

const navs = [
  {
    to: "/",
    title: "首页"
  },{
    to: "/api",
    title: "API"
  },{
    to: "/about",
    title: "关于"
  }
];

export { routes, navs };
```

```JS
// routes 的 index.js
import {Route, Switch} from "react-router-dom";
import { routes } from "./config";
function Routes(props) {
    // 生成路由
   return <Switch>
      {
        routes.map((item,index)=>{
            return <Route 
                key={index}
                path={item.path}
                exact={item.exact}
                // 传递参数
                render={(Routerprops)=>{
                    return item.render({...props,...Routerprops});
                }}
            />
        })
      }
   </Switch>
}
export default Routes;
```

- 配置store
  - action 放异步请求

    - ```JS
      import axios from "axios";
      import {useDispatch} from "react-redux";
      const HTTP = axios.create({
        baseURL: "http://localhost:8080/api"
      });
      
      function useGetTopics() {
        const dispatch = useDispatch();
        return (page="1",tab="all",limit="20")=>{
            dispatch({
              type:"TOPICS_LOADING"
            })
            HTTP.get(`/topics?page=${page}&tab=${tab}&limit=${limit}`)
            .then((res)=>{
                console.log(res);
                dispatch({
                  type:"TOPICS_LOAD",
                  data:res.data.data
                })  
            })
        }
      }
      
      export {useGetTopics}
      ```

  - reducer 放纯函数

    - ```JS
      export default function topics(topics = {
          loading: true,
          data: []
      }, action) {
          switch (action.type) {
              case "TOPICS_LOADING":
                  return {
                      loading: true,
                      data: []
                  }
              case "TOPICS_LOAD":
                  return {
                      loading: false,
                      data: action.data
                  }
          }
          return topics;
      }
      ```

  - store    index.js

    - ```JS
      store index.js
      import {createStore,combineReducers} from "redux";
      import topics from "./reducer/topics";
      // 创建并导出 store 
      // combineReducers 合并reducer
      export default createStore(combineReducers({
        topics
      }));
      ```

- 配置App根组件搭建视图框架

```JS
// 引入 antd 的布局组件
import { Layout } from "antd";

import { Link } from "react-router-dom";
import Header  from "./component/header";
import { navs } from "./routes/config";
import Routes from "./routes/index";
function App() {
    // 使用组件
    return <Layout>
                <Header />
                <Layout.Content>
                    <Routes />
                </Layout.Content>
                <nav>
                    {navs.map(item=>{
                        return <Link to={item.to} key={item.to}>{item.title}</Link>
                    })}
                </nav>
    </Layout>
}
export default App;

```

- 配置less & 定制主题

  - 安装 $ yarn add @craco/craco

  - 替换package.json的3条配置

  - ```JS
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    ```

  - 在项目根目录创建一个 craco.config.js 用于修改默认配置。

  - ```JS
    /* craco.config.js */
    module.exports = {
      // ...
    };
    ```

  - static新建less文件，再建 index.less 引入以下代码。~ 是指根目录就是node_modules

  - ```JS
    @import '~antd/dist/antd.less';
    ```

  - 入口文件index.js引入static下的less文件

  - ```JS
    import "./static/less/index.less";
    ```

  - 安装 $ yarn add craco-less

  - craco.config.js 如下，'@primary-color': '#1DA57A'  修改主题色，然后在使用组件的地方替换成刚刚修改好的主题名字

  - ```JS
    <Button type="primary">按钮</Button>
    ```

  - ```JS
    const CracoLessPlugin = require('craco-less');
    
    module.exports = {
      plugins: [
        {
          plugin: CracoLessPlugin,
          options: {
            lessLoaderOptions: {
              lessOptions: {
                modifyVars: { 
                    '@primary-color': '#1DA57A' 
                    @link-color: #1890ff; // 链接色
                    @success-color: #52c41a; // 成功色
                    @warning-color: #faad14; // 警告色
                    @error-color: #f5222d; // 错误色
                    @font-size-base: 14px; // 主字号
                    。。。自行添加
                },
                javascriptEnabled: true,
              },
            },
          },
        },
      ],
    };
    ```

### render props

- render props 是一个用于告知组件要渲染什么内容的函数属性。该函数返回一个组件，是渲染出来的内容。
- hooks

```JS
// component pop.js

import { useState } from "react";
// 公用 弹窗组件
function Popup(props){
  const {render} = props;
  const [show,setShow] = useState(true);
  // 1-订义关闭按钮
  const Clos = ()=>{
    return <button onClick={()=>{
      setShow(false);
    }}>关闭</button>
  }
  return <div>
    {/* 2-把Clos传给render */}
      {show?render(Clos):""}
  </div>
}

export default Popup;
```

```JS
// 页面 index.js
import { Route } from "react-router-dom";
// 引入组件
const { default: Popup } = require("../../component/pop");
const { default: IndexNav } = require("./indexNav");

function IndexPage() {
   return <div className="view">
         <div className="wrap">
            <IndexNav />
            <Route render={(routerProps)=>{
               return <div>视图</div>
            }} />
			// 调用组件Popup
            <Popup
               // 3-render接收Popup组件传递的关闭按钮Clos
               render={(Clos)=>{
                  // 返回的视图
                  return <div>
                     <h1>首页弹窗</h1>
                     <p>首页的弹窗内容</p>
                     <IndexNav />
                     {/* 4-调用关闭按钮 */}
                     <Clos />
                  </div>
               }}
            />
         </div>
   </div>
}
export default IndexPage;
```

### qs

```JS
// qs.parse 将search转成序列化对象，再截取想要的数据
const { tab = "all" } = qs.parse(search.slice(1));
```

### 按需加载(懒加载)处理

- 可以用在组件上也可以用在路由上

- suspense 和 lazy 进行懒加载设置

```
    const Child = lazy(()=>import("./child"));
    <Suspense fallback={<div>视图请求中</div>} >
        <Child />
    </Suspense>
```

```JS
// 路由-config.js
import IndexPage from "./index/index";
import AboutPage from "./about/index";
import UndefinedPage from "./404/index";

// 1-路由懒加载
import { lazy, Suspense } from "react";
// 2-路由懒加载
const NewAPIPage = lazy(() => import("./api/index"));

const routes = [
  {
    path: "/",
    exact: true,
    render(props) {
      return <IndexPage {...props} />
    }
  }, {
    path: "/api",
    exact: true,
    render(props) {
      // 3-路由懒加载，Suspense包裹，fallback属性是组件没有请求成功之前显示什么内容
      return <Suspense fallback={<div>视图请求中</div>} >
        <NewAPIPage {...props} />
      </Suspense>
    }
  }
];


export { routes, indexNavs };
```

```JS
App.js

import React, { Fragment, lazy, Suspense } from 'react';
import "./index.css";
import { Switch, Route } from 'react-router-dom';;

// 1-路由懒加载/按需加载
const View404 = lazy(()=>import(`./view404`));
const IndexView = lazy(()=>import(`./view/index`));
const DetailView = lazy(()=>import(`./view/details/index`));

const types = ["all","good","share","ask"];
function App() {
  return (<div className="wrap">
        <Switch>
            <Route 
                path="/detail/:id"
                render = {()=>{
                  // 2-Suspense 引入， fallback配置
                    return <Suspense fallback={<div>loading……</div>}>
                        <DetailView />
                    </Suspense>
                }}
            />
            <Route 
                path={["/","/:type","/:type/:page"]} 
                exact
                render={(props)=>{
                    let {params} = props.match;
                    let {type="all",page=1} = params;
                    if(types.includes(type)&&page>=1){
                      return  
                        // 3-使用
                        <Suspense fallback={<div>loading……</div>}><IndexView /></Suspense>
                    }
                    return  
                        <Suspense fallback={<div>loading……</div>}><View404 /></Suspense>
                }}
            />
            <Route render={()=>{
              return <Suspense fallback={<div>loading……</div>}><View404 /></Suspense>
            }} />
        </Switch>
  </div>);
}

export default App;
```

### 部署的小细节

history 路由一定记得配置 404
xampp 

### 组件封装

- 视图一致，逻辑不同
  - 封装成一个组件，将不同的逻辑当做参数传入
- 逻辑相似，但视图不同时:
  - Hooks

```JS
// 封装
import { Menu } from "antd";
import { Link, useLocation } from "react-router-dom";
function RootNav(props) {
    /*
        数据不一样，
        选中项方法不一样
        theme 不一样  
    */
   const {data,theme,toSelect} = props;
   const location = useLocation();
   const selectKey = toSelect(location);
   return  <Menu
      theme={theme}
      mode="horizontal"
      selectedKeys={[selectKey]}
   >
        {
          data.map((item,index)=>{
            return <Menu.Item key={index.toString()}>
                <Link to={item.to}>{item.title}</Link>
            </Menu.Item>
          })
        }
   </Menu>
}

export default RootNav;
```

```JS
// 使用
import { navs } from "../routes/config";
import RootNav from "./rootNav";
function Nav() {
  return <RootNav
    theme={"dark"}
    data={navs}
    toSelect={(location) => {
      const { pathname } = location;
      return navs.findIndex(item => (item.to === pathname)) + "";
    }}
  />
}

export default Nav;
```

```JS
// 使用
import { indexNavs } from "../config";
import qs from "qs";
import RootNav from "../../component/rootNav";
function IndexNav() {
  return <RootNav
    theme={"light"}
    data={indexNavs}
    toSelect={(location) => {
      const { search } = location;
      const { tab = "all" } = qs.parse(search.slice(1));
      return indexNavs.findIndex(item => (item.to.split("=")[1] === tab)) + "";
    }}
  />
}
export default IndexNav;
```

## React  其他方法

### ReactDOM

- createPortal 

  - 提供一种将子节点渲染到DOM节点的方式，该节点存在于DOM组件的层次结构之外，将虚拟DOM渲染到其他的地方

    - ```JS
      import React from 'react';
      import ReactDOM from "react-dom";
      function Child() {
        // 渲染到 root2 中
        return ReactDOM.createPortal(<h1>Hello Child</h1>,document.querySelector("#root2"))
      }
      
      export default Child;
      
      ```

  - findDOMNode

    - 如果组件已经被挂载到DOM上，此方法会返回浏览器中相应的原生DOM元素，检测虚拟DOM有没有渲染到DOM节点中

    - ```JS
      console.log(ReactDOM.findDOMNode(<App />));
      ```

  - unmountComponentAtNode

    -  从DOM中强制卸载组件，会将其事件处理器(event handlers) 和 state 一并清除，容器下所有的节点清空

    - ```JS
      ReactDOM.unmountComponentAtNode(document.querySelector("#root"));
      ```

  - unstable_batchedUpdates

    - 提升性能优化，手动合并更新，React 中 Promise.then(fn) setTimeout xhr 等，异步程序中React没有控制权,2个 set 只触发一次更新

    - ```JS
      <button onClick={()=>{
                setTimeout(()=>{
                  // unstable_batchedUpdates 手动合并更新
                  ReactDOM.unstable_batchedUpdates(()=>{
                    setName("开课吧");
                    setAge(++age);
                  });
                });
              }}>按钮</button>
      ```

  - 强制更新 `this.forceUpdate();`

### useContext

```JS
// 1 createContext
import {createContext} from  "react";
const context = createContext();
const {Provider} = context;
export default context;
export {Provider};
```

```JS
import React, { useState } from 'react';
// 2 Provider value 传递信息
import {Provider} from "./store/index";
import Child from "./child";
function App() {
  let [name, setName] = useState("kkb");
  return <Provider value={{
      name,
      setName
  }}>
      <Child />
  </Provider>
}
export default App;

```

```JS
import React,{Fragment, useContext, useState} from 'react';
import context from "./store/index";
function Child(){
  let [age,setAge] = useState(9);
  // 3 接收信息，useContext接收context返回的值就是传递的信息
  const data = useContext(context);
  let {name,setName} = data;
  return <Fragment>
    <p>{age}</p>
    <p>{name}</p>
    <button
      onClick={()=>{
        age++;
        setAge(age);
      }}
    >修改age</button>
    <button onClick={()=>{
      setName("开课吧");
    }}>修改name</button>
  </Fragment>
}
export default Child;

```

## VUE & React

### 相同点

- 都有组件化开发和虚拟DOM
- 都支持 props 进行父子组件间数据通信
- 都支持数据驱动视图，不直接操作真实DOM，更新状态数据视图就更新

### 不同点

- 数据绑定
  - VUE实现了数据的双向绑定
  - React 是 自上而下的单向数据流 
- 组件写法不一样
  - React是JSX，把 html css 写进 js 中
  - VUE 是单文件，html css js 写在同一个文件
- state 对象
  - React 中 state 不可变，需要使用 setState 更新状态
  - VUE 中 state 对象不是必须的，数据由 data 属性 在 vue 对象中管理
- 虚拟DOM
  - VUE 会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树
  - React 状态改变时，全部组件都会重新渲染，需要使用 `shouldComponentUpdate` 这个生命周期进行控制