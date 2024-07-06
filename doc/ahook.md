# useEventEmitter
通过 props 或者 Context ，可以将 event$ 共享给其他组件。然后在其他组件中，可以调用 EventEmitter 的 emit 方法，推送一个事件，或是调用 useSubscription 方法，订阅事件。
在组件多次渲染时，每次渲染调用 useEventEmitter 得到的返回值会保持不变，不会重复创建 EventEmitter 的实例。
`event$.emit('hello');`
```
event$.useSubscription(val => {
    console.log(val);
});
```
useSubscription 会在组件创建时自动注册订阅，并在组件销毁时自动取消订阅。

**实例**：通过父组件点击来触发子组件中的事件，以及子组件中子组件的事件
父组件中：
```
import { useEventEmitter } from 'ahooks';

const focus$: any = useEventEmitter();

// 父组件调用方法
const hanlderReload = () => {
    // 推送子组件的事件
    focus$.emit('刷新');
};

<div className='father' onClick={hanlderClick}>
   <Son focus$={focus$} />
</div>
```

子组件中：
```
// 在子组件中进行使用
const { focus$ } = props; 

focus$.useSubscription((a: string) => {
   console.log(a)
});

<div className='son'>
  <Sun focus$={focus$}/>
</div>
```

子孙组件中
```
const { focus$ } = props; 

focus$.useSubscription((a: string) => {
    console.log(a)
});
```

## 总结
```
const result: Result = useEventEmitter<T>();
```
- emit：发送一个事件通知
- useSubscription：订阅事件


# useDebounce
用来处理防抖值的 Hook。
### 基础用法
DebouncedValue 只会在输入结束 500ms 后变化。
```
import React, { useState } from 'react';
import { useDebounce } from 'ahooks';

export default () => {
  const [value, setValue] = useState<string>();
  const debouncedValue = useDebounce(value, { wait: 500 });

  return (
    <div>
      <input
        value={value}
        onChange={(e) => setValue(e.target.value)}
        placeholder="Typed value"
        style={{ width: 280 }}
      />
      <p style={{ marginTop: 16 }}>DebouncedValue: {debouncedValue}</p>
    </div>
  );
};
```

# useThrottle
用来处理节流值的 Hook。
### 基础用法
ThrottledValue 每隔 500ms 变化一次。
```
import React, { useState } from 'react';
import { useThrottle } from 'ahooks';

export default () => {
  const [value, setValue] = useState<string>();
  const throttledValue = useThrottle(value, { wait: 500 });

  return (
    <div>
      <input
        value={value}
        onChange={(e) => setValue(e.target.value)}
        placeholder="Typed value"
        style={{ width: 280 }}
      />
      <p style={{ marginTop: 16 }}>throttledValue: {throttledValue}</p>
    </div>
  );
};
```

