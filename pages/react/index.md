## 1.虚拟dom和diff算法

**React 虚拟 DOM：**

React 虚拟 DOM 是一个用 JavaScript 对象模拟 DOM 树结构的轻量级结构，它包含了组件的属性和状态信息，和组件的 render 函数返回的 JSX 元素的信息。当组件状态变化时，React 会重新构建一颗新的虚拟 DOM 树，并且比较新旧两颗树的差异，这个过程称为 Reconciliation，然后再根据差异信息，更新 DOM，使页面呈现出最新的状态。

**Diff 算法：** 原理：https://juejin.cn/post/6940942549305524238#heading-25

Diff 算法是用于比较新旧两颗虚拟 DOM 树的算法。当组件状态改变时，React 首先会生成一颗新的虚拟 DOM 树，然后通过 Diff 算法比较新旧两颗树之间的差异，找出需要更新的节点。Diff 算法的核心思想是只比较同级节点之间的差异，不跨级比较，这样可以尽可能地减少比较次数，提高比较效率。同时，Diff 算法还会尽可能地复用旧节点，避免不必要的 DOM 操作，进一步提高性能。

总的来说，React 虚拟 DOM 和 Diff 算法是 React 构建高效、快速、可靠应用的重要手段，它们可以减少不必要的 DOM 操作，提高页面渲染效率和用户体验。

## 2. useEffect 与 useLayoutEffect 的区别
（1）共同点

运用效果： useEffect 与 useLayoutEffect 两者都是用于处理副作用，这些副作用包括改变 DOM、设置订阅、操作定时器等。在函数组件内部操作副作用是不被允许的，所以需要使用这两个函数去处理。

使用方式： useEffect 与 useLayoutEffect 两者底层的函数签名是完全一致的，都是调用的 mountEffectImpl方法，在使用上也没什么差异，基本可以直接替换。

（2）不同点

使用场景： useEffect 在 React 的渲染过程中是被异步调用的，用于绝大多数场景；而 useLayoutEffect 会在所有的 DOM 变更之后同步调用，主要用于处理 DOM 操作、调整样式、避免页面闪烁等问题。也正因为是同步处理，所以需要避免在 useLayoutEffect 做计算量较大的耗时任务从而造成阻塞。

使用效果： useEffect是按照顺序执行代码的，改变屏幕像素之后执行（先渲染，后改变DOM），当改变屏幕内容时可能会产生闪烁；useLayoutEffect是改变屏幕像素之前就执行了（会推迟页面显示的事件，先改变DOM后渲染），不会产生闪烁。useLayoutEffect总是比useEffect先执行。

在未来的趋势上，两个 API 是会长期共存的，暂时没有删减合并的计划，需要开发者根据场景去自行选择。React 团队的建议非常实用，如果实在分不清，先用 useEffect，一般问题不大；如果页面有异常，再直接替换为 useLayoutEffect 即可。

## 3.usememo和useCallback区别
useMemo 用于缓存计算结果，返回一个 memoized 值，只有当依赖项列表中的值发生变化时，才会重新计算该值；
useCallback 用于缓存函数，返回一个 memoized 函数，只有当依赖项列表中的值发生变化时，才会重新创建该函数。


## 3.key的作用
https://juejin.cn/post/6940942549305524238#heading-26

## 2.redux的使用
1.创建 store

我们可以使用 Redux 提供的 createStore 函数来创建一个 store，通常在应用入口文件中创建，并且只需要创建一次，例如：
```javascript
import { createStore } from 'redux';
import rootReducer from './reducers';

const store = createStore(rootReducer);


```
2.创建 action

Action 是描述发生了什么的纯 JavaScript 对象，可以使用函数来创建，例如
```javascript
export const increment = () => {
  return {
    type: 'INCREMENT'
  };
};

export const decrement = () => {
  return {
    type: 'DECREMENT'
  };
};

```

3.使用 Provider 包裹组件

在渲染根组件时，使用 Redux 提供的 Provider 组件将创建好的 store 传递给子组件，例如：
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import App from './App';
import store from './store';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);

```

4.在组件中使用 useSelector 和 useDispatch 钩子
```javascript
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from './actions';

function Counter() {
  const count = useSelector(state => state.count);
  const dispatch = useDispatch();

  const handleIncrement = () => {
    dispatch(increment());
  };

  const handleDecrement = () => {
    dispatch(decrement());
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleIncrement}>+</button>
      <button onClick={handleDecrement}>-</button>
    </div>
  );
}

export default Counter;

```

## 3.redux中间件
https://juejin.cn/post/6940942549305524238#heading-6

## 4.mobx和redux区别
https://juejin.cn/post/6940942549305524238#heading-9

## 5.mobx的使用流程
1.安装 mobx

2.创建 store 对象

MobX 的核心是 store 对象。在函数式组件中，你可以使用 useLocalStore hook 来创建 store 对象。这个 hook 接受一个函数作为参数，在这个函数中返回一个包含可观察状态和动作的对象。示例代码：

```javascript

import { useLocalStore } from 'mobx-react';

const useStore = () => {
  return useLocalStore(() => ({
    count: 0,
    increment() {
      this.count++;
    },
    decrement() {
      this.count--;
    },
  }));
};

```

3.将 store 对象传递给组件

MobX 中的组件可以分为两种：观察者组件和非观察者组件。观察者组件可以自动响应 store 中的可观察状态的变化，而非观察者组件则需要手动触发渲染。在函数式组件中，你可以使用 observer 函数来将组件转换为观察者组件。示例代码：

```javascript
import { observer } from 'mobx-react';

const Counter = observer(() => {
  const store = useStore();

  const handleIncrement = () => {
    store.increment();
  };

  const handleDecrement = () => {
    store.decrement();
  };

  return (
    <div>
      <p>Count: {store.count}</p>
      <button onClick={handleIncrement}>Increment</button>
      <button onClick={handleDecrement}>Decrement</button>
    </div>
  );
});

```

4.使用 Provider 组件

和 Redux 一样，MobX 也提供了 Provider 组件来将 store 对象传递给整个应用程序。示例代码：

```javascript
import { Provider } from 'mobx-react';

const App = () => {
  const store = useStore();

  return (
    <Provider store={store}>
      <Counter />
    </Provider>
  );
};

```
