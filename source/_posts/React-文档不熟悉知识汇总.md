---
title: React 文档不熟悉知识汇总
date: 2018-10-24 20:09:08
tags:
description: "总结了对文档内出现不熟知的知识点，希望之后能一一深入学习理解"
preload_comment: true
show_count: false
---

## React 官方文档

### 理解：
* React 可以将多个setState() 调用合并成一个调用来提高性能，所以 this.props​ 与 this.state 是异步更新的，若想根据上个state来更新state，则如下：
  ```
  this.setState((prevState, props) => ({
    count: prevState.count + props.num
  }));
  ```
* React 中，事件处理函数中阻止默认行为不能返回 false ，而是用  e.preventDefault(); 来解决 , e 是个合成事件，已解决了跨浏览器兼容问题；通过箭头函数，e 必须显示进行传递，但是通过 bind 的方式，事件对象以及更多的参数将会被隐式进行传递。
* 数组元素必须为每个项配置一个key，并且在其兄弟中是独一无二的，并不需要在全局中唯一
* **状态提升**：任何可变的数据理应只有一个单一数据源，应在应用中保持自上而下的数据流，也就是把两个及以上组件共同使用的数据提升到其父组件上，而不是在不同组件中同步状态。
* 如果某些数据既可用 state 又可用 props 提供，那么它很有可能不应该在 state 中出现。
* 如果一个数据不是动态改变并且影响到渲染的，那么就不应该添加在 state 里。
* **单一功能原则**：尽量维持一个组件只做一件事情，如果这个组件的功能不断丰富，它应该被分成更小的组件
* JSX 只是为了 `React.createElement(component, props, children)` 方法提供了语法糖。
* Context 提供了一种在组件之间共享数据的方式，而不必通过组件树的每个层级显式地传递 props 。
  ```
  const {Provider, Consumer} = React.createContext(defaultValue);
  
  <Provider value={/* some value */}>
  
  <Consumer>
    {value => /* render something based on the context value */}
  </Consumer>
  ```
  每当Provider的值发送改变时, 作为Provider后代的所有Consumers都会重新渲染。 从Provider到其后代的Consumers传播不受shouldComponentUpdate方法的约束，因此即使祖先组件退出更新时，后代Consumer也会被更新。
  
  高阶组件封装context
  ```
  const ThemeContext = React.createContext('light');

  // 在函数中引入组件
  export function withTheme(Component) {
    // 然后返回另一个组件
    return function ThemedComponent(props) {
      // 最后使用context theme渲染这个被封装组件
      // 注意我们照常引用了被添加的属性
      return (
        <ThemeContext.Consumer>
          {theme => <Component {...props} theme={theme} />}
        </ThemeContext.Consumer>
      );
    };
  }
  ```

### 了解：
* 使用propTypes进行props类型检查，如 `Demo.propTypes = {...};` ，出于性能原因， propTypes只在开发模式下进行检查
* **Refs**: 不能在函数式组件上使用 ref 属性，因为它们没有实例；当 ref 属性被用于一个普通的 HTML 元素时，React.createRef() 将接收底层 DOM 元素作为它的 current 属性以创建 ref；当 ref 属性被用于一个自定义类组件时，ref 对象将接收该组件已挂载的实例作为它的 current 。ref 的更新会发生在componentDidMount 或 componentDidUpdate 生命周期钩子之前

  ```
  class CustomTextInput extends React.Component {
    constructor(props) {
      super(props);
      // 创建 ref 存储 textInput DOM 元素
      this.textInput = React.createRef();
      this.focusTextInput = this.focusTextInput.bind(this);
    }

    focusTextInput() {
      // 直接使用原生 API 使 text 输入框获得焦点
      // 注意：通过 "current" 取得 DOM 节点
      this.textInput.current.focus();
    }

    render() {
      // 告诉 React 我们想把 <input> ref 关联到构造器里创建的 `textInput` 上
      return (
      <div>
        <input
        type="text"
        ref={this.textInput} />
        
        <input
        type="button"
        value="Focus the text input"
        onClick={this.focusTextInput}
        />
      </div>
      );
    }
  }
  ```
  
  一个关于渲染属性API的问题是 refs 不会自动的传递给被封装的元素。为了解决这个问题，使用 React.forwardRef
  ```
  class FancyButton extends React.Component {
    focus() {
      // ...
    }
  
    // ...
  }
  
  // 使用 context 传递当前的 "theme" 给 FancyButton.
  // 使用 forwardRef 传递 refs 给 FancyButton 也是可以的.
  export default React.forwardRef((props, ref) => (
    <ThemeContext.Consumer>
      {theme => (
        <FancyButton {...props} theme={theme} ref={ref} />
      )}
    </ThemeContext.Consumer>
  ));
  ```
* **协同算法**：
  1. 两个不同类型的元素将产生不同的树。
  2. 通过渲染器附带key属性，开发者可以示意哪些子元素可能是稳定的。
  3. 当对比两棵树时，React首先比较两个根节点。每当根元素有不同类型，React将卸载旧树并重新构建新树。
  4. 当比较两个相同类型的React DOM元素时，React则会观察二者的属性，保持相同的底层DOM节点，并仅更新变化的属性。当组件更新时，实例仍保持一致，以让状态能够在渲染之间保留
  5. 当递归DOM节点的子节点，React仅在同一时间点递归两个子节点列表，并在有不同时产生一个变更。为解决该问题，React支持了一个key属性。当子节点有key时，React使用key来匹配原本树的子节点和新树的子节点。
  6. Keys应该是稳定的，可预测的，且唯一的。不稳定的key（类似由Math.random()生成的）将使得大量组件实例和DOM节点进行不必要的重建，使得性能下降并丢失子组件的状态。
* Fragments: React 中一个常见模式是为一个组件返回多个元素。Fragments 可以让你聚合一个子元素列表，并且不在DOM中增加额外节点。
  ```
  render() {
    return (
      <>   // fragments
        <ChildA />
        <ChildB />
        <ChildC />
      </>
    );
  }
  ```
  


###初次知道：
* 静态类型检查：使用 Flow 和 TypeScript 静态类型检查器可以在运行代码之前识别某些类型问题
* Create React App
* uglify-js-brunch
* Browserify
* Rollup
* 使用 Chrome Performance 归档组件
* Immutable.js是解决这个问题的另一种方法。它通过结构共享提供不可突变的，持久的集合：
* Portals 提供了一种很好的将子节点渲染到父组件以外的 DOM 节点的方式。第一个参数（child）是任何可渲染的 React 子元素，例如一个元素，字符串或碎片。第二个参数（container）则是一个 DOM 元素。对于 portal 的一个典型用例是当父组件有 overflow: hidden 或 z-index 样式，但你需要子组件能够在视觉上“跳出（break out）”其容器。例如，对话框、hovercards以及提示框：
 `ReactDOM.createPortal(child, container)`
* **错误边界:** 如果一个类组件定义了一个名为 componentDidCatch(error, info): 的新方法，则其成为一个错误边界
```
  componentDidCatch(error, info) {
    // Display fallback UI
    this.setState({ hasError: true });
    // You can also log the error to an error reporting service
    logErrorToMyService(error, info);
  }
```
* web组件 
* 虚拟DOM（VDOM）是一种编程概念，是指虚拟的视图被保存在内存中，并通过诸如ReactDOM这样的库与“真实”的DOM保持同步。这个过程被称为和解。fiber是React 16中新的和解引擎。它的主要目的是使虚拟DOM能够进行增量渲染
