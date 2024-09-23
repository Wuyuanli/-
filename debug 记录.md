## Widget Container

问题描述：  
useSmallerSize功能，在切换屏幕缩放率的时候偶尔会出现问题，例如从125%切换到250%时，不会自动勾选“缩小”按钮，或者从250%切换到125%时，会自动勾选上“缩小”按钮。

问题解决：  
在change事件绑定的回调函数中打印输出发现，change事件没有成功触发，在MDN上查找得知，change事件的返回值只有true和false，如果绑定的屏幕缩放值改变了或者从别的值变为绑定的缩放值才会返回true，其他情况都返回false。网上搜索js监听屏幕缩放率方法，发现change事件只对返回true的结果做监听，要解决不监听的问题只能在回调函数里，执行完逻辑操作后解绑再绑定change事件，这样change事件每次屏幕缩放率变化都会成功监听。

* * *

问题描述： 在displayScale不变的情况下，调整textScale从250%到110%左右会出现widget显示不完全的情况。

问题解决：  
textScale变化会首先被textScale相关事件监听器监听到，widget大小变化，而后由于DPR也发生了变化，会随即触发displayScale相关事件回调，widget大小又会变化。可能是因为两者都对widget大小有操作，导致冲突产生bug。对setTimeout定时器做单例操作，即同一时刻只有一个定时器存在。

## scrollTop不生效

scrollTop 与 scroll事件绑定，只有被scroll事件绑定的容器才能取到scrollTop值。

## Search 组件

问题描述：当我第一次在搜索框搜索关键词时，会正常显示搜索结果。但第二次使用搜索框搜索时，搜索结果没有发生变化，必须再点击第三次才会变。

问题解决：原有逻辑是在useEffect函数中监听loading的变化，如果变化就重新调用onSearch函数。不同的逻辑耦合在一起，并且由于依赖项是引用类型，导致useEffect不会按照预期被调用。并且keywords是用useState创建的，会引起不必要的性能消耗。使用useRef创建keywords变量，并且useEffect仅在组件初次加载时执行一次。把状态逻辑和搜索逻辑解耦。

## Network subapp

问题描述：enterSubApp和leaveSubApp的相关埋点操作没有正常调用，测试取不到这两个操作的埋点记录。

问题解决：通过打断点debug的方式，发现相关代码中有一个解构操作，这个解构操作的结果可能是object也可能是null，而当结果是null时解构会出错，这个错误没有被捕获导致后续代码都不会执行。放弃在函数开头解构，而把相应的解构操作放在各自的case里执行，这样不会影响到别的代码块，enterSubApp和leaveSubApp的相关埋点操作恢复正常。

```
const { userGuid, userName } = (appMessage.payload || {}) as any;
```

## Vantage offers

问题描述：本地访问线上网址会遇到跨域问题，无法访问。

问题解决：使用proxy代理访问网址进行测试。

## modern preload

问题描述：链接在高对比度模式下颜色显示有误

问题解决：

```
'@media (forced-colors: active)': {
    forcedColorAdjust: 'none',
    borderRadius: 0,
    color: 'Highlight !important',
    background: 'none !important',
  },
```

* * *

问题描述：一排文字中的\`:\`会被narrator单独访问到但不会被读出来

问题解决：下面这种写法的代码会被解析器解析成三段，导致三段文字都可以被单独访问到：

```
<Typography fontWeight={300} variant="body2" id={`modern-preload-app-size-${udcId}`}>
    {t('modernPreload.sizeText')}: {appInfo.size}
</Typography>
```

用一个变量提前组合字符串即可解决。