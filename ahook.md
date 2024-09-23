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

# usePagination
usePagination 基于 useRequest 实现，封装了常见的分页逻辑。与 useRequest 不同的点有以下几点：

1. service 的第一个参数为 { current: number, pageSize: number }
2. service 返回的数据结构为 { total: number, list: Item[] }
3. 会额外返回 pagination 字段，包含所有分页信息，及操作分页的函数。
4. refreshDeps 变化，会重置 current 到第一页，并重新发起请求，一般你可以把 pagination 依赖的条件放这里

### 基础用法
默认用法与 useRequest 一致，但会多返回一个 pagination 参数，包含所有分页信息，及操作分页的函数。

下面的代码演示了，增加了性别参数，在修改性别时，重置分页到第一页，并重新请求数据。
```
import { Pagination } from 'antd';
import Mock from 'mockjs';
import React, { useEffect, useState } from 'react';
import { usePagination } from 'ahooks';

interface UserListItem {
  id: string;
  name: string;
  gender: 'male' | 'female';
  email: string;
  disabled: boolean;
}

const userList = (current: number, pageSize: number) =>
  Mock.mock({
    total: 55,
    [`list|${pageSize}`]: [
      {
        id: '@guid',
        name: '@name',
        'gender|1': ['male', 'female'],
        email: '@email',
        disabled: false,
      },
    ],
  });

async function getUserList(params: {
  current: number;
  pageSize: number;
  gender: string;
}): Promise<{ total: number; list: UserListItem[] }> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(userList(params.current, params.pageSize));
    }, 1000);
  });
}

export default () => {
  const [gender, setGender] = useState<string>('male');

  const { data, loading, pagination, run, params } = usePagination(
    ({ current, pageSize }, g: string) => {
      return getUserList({
        current,
        pageSize,
        gender: g,
      });
    },
    {
      manual: true,
    },
  );

  useEffect(() => {
    run(
      {
        current: 1,
        pageSize: params[0]?.pageSize || 10,
      },
      gender,
    );
  }, [gender]);

  return (
    <div>
      <select
        value={gender}
        style={{ width: 180, marginBottom: 24 }}
        onChange={(e) => setGender(e.target.value)}
        placeholder="select gender"
      >
        <option value="male">male</option>
        <option value="female">female</option>
      </select>
      {loading ? (
        <p>loading</p>
      ) : (
        <ul>
          {data?.list?.map((item) => (
            <li key={item.email}>
              {item.name} - {item.email}
            </li>
          ))}
        </ul>
      )}
      <Pagination
        current={pagination.current}
        pageSize={pagination.pageSize}
        total={data?.total}
        onChange={pagination.onChange}
        onShowSizeChange={pagination.onChange}
        showQuickJumper
        showSizeChanger
        style={{ marginTop: 16, textAlign: 'right' }}
      />
    </div>
  );
};
```

链接地址：https://ahooks.js.org/zh-CN/hooks/use-pagination/