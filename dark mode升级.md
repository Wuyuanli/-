问题：使用extendTheme替换createTheme后，原先themeOptions中配置的样式不知道放在哪里，如果放在light配置中会报错。
解决：在palette中配置的参数统一放到extendTheme中对应的palette中，而其他配置则挪出colorSchemes，放在外层。

问题：想使用extendTheme扩展一个原有的theme，这样写会报错：
```
	const updatedTheme = extendTheme(theme,{
		// 新增配置
	})
```

改为下列样式则不报错：
```
	const updatedTheme = extendTheme({
		// 新增配置
	},theme)
```
官方迁移文档写的有点简略，也跟我自己不太熟悉theme相关api有关系。