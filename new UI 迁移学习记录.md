场景：search 结果页面有5种可能的情况出现，如成功、超时、失败、加载等等，需要区分并识别这几种状态。
原本想法：用Number属性，0代表loading，1代表成功，以此类推。
优化想法：使用 enum 枚举属性

<hr>
<br>

场景：search-result 组件需要实现响应式布局
原本想法：全靠flex布局实现
优化想法：父组件高度100vh，header为固定的80px，由于父组件高度会随页面缩放而变化，使用 CSS calc函数，通过父组件高度减去 80px 来动态计算出此组件的高度，并赋值给height属性。
组件内部使用 flex 布局，撑满整个组件。通过这种方式实现响应式布局。（这种方式有点缺陷，当页面高度比较高的时候，由于图片的大小确定，导致图片小小的在页面中央显示，看起来不太美观。结合媒体查询做可能会更好）

<hr>
<br>

场景：封装button组件，接收传入的color值与disabled值，动态赋值给组件
原本想法：在sx属性内用 “||” 符号赋值，形如：

```
sx={{color:color || 'text.secondary'}}
```
优化想法：运用解构赋值，在props中直接给定默认值：

```
const backToTop = ({color='text.secondary',disabled = false})=>{
	...
}
```
<hr>
<br>
场景：需要跨层级获取并操作dom元素

原本想法：直接使用原生js：

```
const content = document.getElementById("content")
```

优化想法：

```
// 利用useRef 和 useEffect 的生命周期特性，避免重复调用dom方法赋值
const contentRef = useRef<HTMLElement | null>(null)

useEffect(()=>{
	contentRef.current = document.getElementById("content")
})
```

<hr>
<br>
场景：mui的Card组件需要在高对比度条件下显示对应颜色边框，满足accessibility要求。

原本想法：直接给Card组件加上：
```
border: 1px solid
```
 但这样做有问题，在非高对比度环境下Card组件也会有边框，不符合所需样式。
 
 优化想法：
 把mui的Card组件再封装一层，在sx属性中添加一个媒体查询CSS：
 ```
 sx={{
 	'@media (forced-colors: active)': {
      border: '1px solid',
    },
 }}
 ```
 这样Card组件在非高对比度环境下显示正常的无边框阴影效果，而在高对比度环境下显示对应颜色边框。
 
 <hr>
 <br>
 
 场景：封装的 back-to-top 组件要滚动的元素必须从外部传入，导致props需要改变，interface的定义不清楚怎么做
 
 原本想法：直接把ButtonProps和scrollElement写在一起，发现报错。原因：react 一般只接收 props 作为传入参数。
 
 优化想法：定义一个BackToTopProps，extends ButtonProps，在里面写上scrollTop的类型定义：
 ```
 interface BackToTopProps extends ButtonProps {
  scrollElement: HTMLElement | null;
}
 ```
 
 在子组件中这样写来给color属性赋初值：
 ```
 const BackToTop = ({
  scrollElement,
	color:'inherit',
	...props
 }:BackToTopProps)=>{
 	//具体组件逻辑 
 }
 ```
 
 <hr>
 <br>
 