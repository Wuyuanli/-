# React 常用hooks

# useContext(SomeContext)

在组件的顶层调用 `useContext` 来读取和订阅 context。

```
import { useContext } from 'react';

function MyComponent() {
  const theme = useContext(ThemeContext);
  // ...
```

**参数**

- SomeContext：先前用 createContext 创建的 context。context 本身不包含信息，它只代表你可以提供或从组件中读取的信息类型。

**返回值**  
`useContext` 为调用组件返回 context 的值。它被确定为传递给树中调用组件上方最近的 `SomeContext.Provider` 的 `value`。如果没有这样的 provider，那么返回值将会是为创建该 context 传递给 `createContext` 的 `defaultValue`。返回的值始终是最新的。如果 context 发生变化，React 会自动重新渲染读取 context 的组件。

**注意事项**

- 组件中的 `useContext()` 调用不受 同一 组件返回的 provider 的影响。相应的 `<Context.Provider>` 需要位于调用 `useContext()` 的组件 之上。
- 从 provider 接收到不同的 `value` 开始，React 自动重新渲染使用了该特定 context 的所有子级。先前的值和新的值会使用 `Object.is` 来做比较。使用 `memo` 来跳过重新渲染并不妨碍子级接收到新的 context 值。
- 如果您的构建系统在输出中产生重复的模块（可能发生在符号链接中），这可能会破坏 context。通过 context 传递数据只有在用于传递 context 的 `SomeContext` 和用于读取数据的 `SomeContext` 是完全相同的对象时才有效，这是由 `===`比较决定的。

**实例**

```
import { createContext, useContext } from 'react';

const ThemeContext = createContext(null);
export default function MyApp() {
  return (
    <ThemeContext.Provider value="dark">
      <Form />
    </ThemeContext.Provider>
  )
}
function Form() {
  return (
    <Panel title="Welcome">
      <Button>Sign up</Button>
      <Button>Log in</Button>
    </Panel>
  );
}
function Panel({ title, children }) {
  const theme = useContext(ThemeContext);
  const className = 'panel-' + theme;
  return (
    <section className={className}>
      <h1>{title}</h1>
      {children}
    </section>
  )
}
function Button({ children }) {
  const theme = useContext(ThemeContext);
  const className = 'button-' + theme;
  return (
    <button className={className}>
      {children}
    </button>
  );
}
```

# useEffect

useEffect 是一个 React Hook，它允许你 将组件与外部系统同步。

```
useEffect(setup, dependencies?)
```

> useEffect是React提供的一个钩子，它允许你在函数组件中执行**副作用操作**。副作用操作是指那些对外部世界产生影响的行为，比如**数据获取**、**订阅**或**手动更改React组件中的DOM**。

## 参数

- `setup`：处理 Effect 的函数。setup 函数选择性返回一个 清理（cleanup） 函数。当组件被添加到 DOM 的时候，React 将运行 setup 函数。在每次依赖项变更重新渲染后，React 将首先使用旧值运行 cleanup 函数（如果你提供了该函数），然后使用新值运行 setup 函数。在组件从 DOM 中移除后，React 将最后一次运行 cleanup 函数。
    
- 可选 `dependencies`：`setup` 代码中引用的所有响应式值的列表。响应式值包括 props、state 以及所有直接在组件内部声明的变量和函数。如果你的代码检查工具 配置了 React，那么它将验证是否每个响应式值都被正确地指定为一个依赖项。依赖项列表的元素数量必须是固定的，并且必须像 `[dep1, dep2, dep3]` 这样内联编写。React 将使用 `Object.is` 来比较每个依赖项和它先前的值。如果省略此参数，则在每次重新渲染组件之后，将重新运行 Effect 函数。
    

> ### 传递依赖数组、空数组和不传递依赖项之间的区别
> 
> 1.  如果指定了依赖项，则 Effect 在 初始渲染后以及依赖项变更的重新渲染后 运行。
> 2.  如果你的 Effect 确实没有使用任何响应式值，则它仅在 初始渲染后 运行。
> 3.  如果完全不传递依赖数组，则 Effect 会在组件的 每次单独渲染（和重新渲染）之后 运行。

## 返回值

useEffect 返回 undefined。

## 注意事项

- useEffect 是一个 Hook，因此只能在 组件的顶层 或自己的 Hook 中调用它，而不能在循环或者条件内部调用。如果需要，抽离出一个新组件并将 state 移入其中。
    
- 如果你 没有打算与某个外部系统同步，那么你可能不需要 Effect。
    
- 当严格模式启动时，React 将在真正的 setup 函数首次运行前，运行一个开发模式下专有的额外 setup + cleanup 周期。这是一个压力测试，用于确保 cleanup 逻辑“映射”到了 setup 逻辑，并停止或撤消 setup 函数正在做的任何事情。如果这会导致一些问题，请实现 cleanup 函数。
    
- 如果你的一些依赖项是组件内部定义的对象或函数，则存在这样的风险，即它们将 导致 Effect 过多地重新运行。要解决这个问题，请删除不必要的 对象 和 函数 依赖项。你还可以 抽离状态更新 和 非响应式的逻辑 到 Effect 之外。
    
- 如果你的 Effect 不是由交互（比如点击）引起的，那么 React 会让浏览器 在运行 Effect 前先绘制出更新后的屏幕。如果你的 Effect 正在做一些视觉相关的事情（例如，定位一个 tooltip），并且有显著的延迟（例如，它会闪烁），那么将 useEffect 替换为 useLayoutEffect。
    
- 如果你的 Effect 是由一个交互（比如点击）引起的，React 可能会在浏览器重新绘制屏幕之前执行 Effect。通常情况下，这样是符合预期的。但是，如果你必须要推迟 Effect 执行到浏览器绘制之后，和使用 alert() 类似，可以使用 setTimeout。
    
- 即使你的 Effect 是由一个交互（比如点击）引起的，React 也可能允许浏览器在处理 Effect 内部的状态更新之前重新绘制屏幕。通常，这样是符合预期的。但是，如果你一定要阻止浏览器重新绘制屏幕，则需要用 useLayoutEffect 替换 useEffect。
    
- Effect 只在客户端上运行，在服务端渲染中不会运行。
    

**实例**

```
import { useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [serverUrl, roomId]);
  // ...
}
```

# useMemo

useMemo 是一个 React Hook，它在每次重新渲染的时候能够缓存计算的结果。

```
const cachedValue = useMemo(calculateValue, dependencies)
```

## 参数

- `calculateValue`：要缓存计算值的函数。它应该是一个没有任何参数的纯函数，并且可以返回任意类型。React 将会在首次渲染时调用该函数；在之后的渲染中，如果 `dependencies` 没有发生变化，React 将直接返回相同值。否则，将会再次调用 `calculateValue` 并返回最新结果，然后缓存该结果以便下次重复使用。
    
- `dependencies`：所有在 `calculateValue` 函数中使用的响应式变量组成的数组。响应式变量包括 props、state 和所有你直接在组件中定义的变量和函数。如果你在代码检查工具中 配置了 React，它将会确保每一个响应式数据都被正确地定义为依赖项。依赖项数组的长度必须是固定的并且必须写成 `[dep1, dep2, dep3]` 这种形式。React 使用 `Object.is` 将每个依赖项与其之前的值进行比较。
    

## 返回值

- 在初次渲染时，useMemo 返回不带参数调用 calculateValue 的结果。
    
- 在接下来的渲染中，如果依赖项没有发生改变，它将返回上次缓存的值；否则将再次调用 calculateValue，并返回最新结果。
    

## 注意事项

- `useMemo` 是一个 React Hook，所以你只能 **在组件的顶层** 或者自定义 Hook 中调用它。你不能在循环语句或条件语句中调用它。如有需要，将其提取为一个新组件并使用 state。
- 在严格模式下，为了 帮你发现意外的错误，React 将会 **调用你的计算函数两次**。这只是一个开发环境下的行为，并不会影响到生产环境。如果计算函数是一个纯函数（它本来就应该是），这将不会影响到代码逻辑。其中一次的调用结果将被忽略。
- 除非有特定原因，React **不会丢弃缓存值**。例如，在开发过程中，React 会在你编辑组件文件时丢弃缓存。无论是在开发环境还是在生产环境，如果你的组件在初始挂载期间被终止，React 都会丢弃缓存。在未来，React 可能会添加更多利用丢弃缓存的特性——例如，如果 React 在未来增加了对虚拟化列表的内置支持，那么丢弃那些滚出虚拟化列表视口的缓存是有意义的。你可以仅仅依赖 `useMemo` 作为性能优化手段。否则，使用 state 变量 或者 ref 可能更加合适。

**实例**

```
export default function TodoList({ todos, tab, theme }) {
  // 告诉 React 在重新渲染之间缓存你的计算结果...
  const visibleTodos = useMemo(
    () => filterTodos(todos, tab),
    [todos, tab] // ...所以只要这些依赖项不变...
  );
  return (
    <div className={theme}>
      {/* ... List 也就会接受到相同的 props 并且会跳过重新渲染 */}
      <List items={visibleTodos} />
    </div>
  );
}
```

# useReducer

useReducer 是一个 React Hook，它允许你向组件里面添加一个 reducer。

```
const [state, dispatch] = useReducer(reducer, initialArg, init?)
```

## 参数

- reducer：用于更新 state 的纯函数。参数为 state 和 action，返回值是更新后的 state。state 与 action 可以是任意合法值。
- initialArg：用于初始化 state 的任意值。初始值的计算逻辑取决于接下来的 init 参数。
- 可选参数 init：用于计算初始值的函数。如果存在，使用 init(initialArg) 的执行结果作为初始值，否则使用 initialArg。

## 返回值

useReducer 返回一个由两个值组成的数组：

- 当前的 state。初次渲染时，它是 init(initialArg) 或 initialArg （如果没有 init 函数）。
- dispatch 函数。用于更新 state 并触发组件的重新渲染。

## 注意事项

- useReducer 是一个 Hook，所以只能在 组件的顶层作用域 或自定义 Hook 中调用，而不能在循环或条件语句中调用。如果你有这种需求，可以创建一个新的组件，并把 state 移入其中。
- 严格模式下 React 会 调用两次 reducer 和初始化函数，这可以 帮助你发现意外的副作用。这只是开发模式下的行为，并不会影响生产环境。只要 reducer 和初始化函数是纯函数（理应如此）就不会改变你的逻辑。其中一个调用结果会被忽略。

**实例**

```
import { useReducer } from 'react';

function reducer(state, action) {
  if (action.type === 'incremented_age') {
    return {
      age: state.age + 1
    };
  }
  throw Error('Unknown action.');
}

export default function Counter() {
  const [state, dispatch] = useReducer(reducer, { age: 42 });

  return (
    <>
      <button onClick={() => {
        dispatch({ type: 'incremented_age' })
      }}>
        Increment age
      </button>
      <p>Hello! You are {state.age}.</p>
    </>
  );
}
```