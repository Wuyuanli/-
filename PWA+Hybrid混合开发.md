# PWA
## PWA概念
PWA 是 Google 于 2016 年提出的概念，于 2017 年正式落地，于 2018 年迎来重大突破，全球顶级的浏览器厂商，Google、Microsoft、Apple 已经全数宣布支持 PWA 技术。

PWA 全称为 Progressive Web App，中文译为渐进式 Web APP，其目的是通过各种 Web 技术实现与原生 App 相近的用户体验。

纵观现有 Web 应用与原生应用的对比差距，如离线缓存、沉浸式体验等等，可以通过已经实现的 Web 技术去弥补这些差距，最终达到与原生应用相近的用户体验效果。

## PWA特性
- 安全可靠
使用 Service Work 技术实现即时下载，当用户打开应用后，页面资源的加载不再完全依赖于网络，而是使用 Service Work 缓存离线包存在本地，确保为用户提供即时可靠的体验。

- 访问更快
首屏可以部署在服务端，节省网页请求时间，加载速度更快，拥有更平滑的动态效果和快速的页面响应。

- 响应式界面
支持各种类型的终端和屏幕。

- 沉浸式体验
在支持 PWA 的浏览器和手机应用上可以直接将 Web 应用添加到用户的主屏幕上，无需从应用商店下载安装。从主屏幕上打开应用之后，提供沉浸式的全屏幕体验。

## PWA功能
- 手机应用配置(Web App Manifest)
可以通过 manifest.json 文件配置，使得可以直接添加到手机的桌面上。

- 离线加载与缓存(Service Worker+Cache API )
可以通过 Service Worker + HTTPS +Cache Api + indexedDB 等一系列 Web 技术实现离线加载和缓存。

- 消息推动与通知(Push&Notification )
实现实时的消息推送与通知

- 数据及时更新(Background Sync )
后台同步，数据及时更新

# Hybrid App
混合模式移动应用，介于Web App、Native App这两者之间的App开发技术，兼具“Native App良好交互体验的优势”和“Web App跨平台开发的优势”（百度百科解释）

主要的原理是，由Native通过JSBridge等方法提供统一的API，然后用Html+Css实现界面，JS来写逻辑，调用API，最终的页面在Webview中显示，这种模式下，Android、iOS的API一般有一致性，Hybrid App所以有跨平台效果。

**优点：** 开发和发布都比较方便，效率介于Native App、Web App之间。

**缺点：** 学习范围较广，需要原生配合。

**举个栗子：** FanReact，我爱我家App，东方航空App，富国基金-富国钱包App

**应用技术：** PhoneGap，AppCan，Wex5，APICloud等。