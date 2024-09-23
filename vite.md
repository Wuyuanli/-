# vite自定义构建
构建过程可以通过多种 构建配置选项 来自定义构建。具体来说，你可以通过 `build.rollupOptions` 直接调整底层的 Rollup 选项：
```export default defineConfig({
  build: {
    rollupOptions: {
      // https://rollupjs.org/configuration-options/
    },
  },
})
```
***
# 产物分块策略
你可以通过配置 `build.rollupOptions.output.manualChunks` 来自定义 chunk 分割策略。如果你使用的是一个框架，那么请参考他们的文档来了解如何配置分割 chunk。