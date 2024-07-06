**1. 本地debug，控制台输出或打断点，判断哪里出问题**
2. 查找相关资料，解决bug

### 先定位问题，再解决问题！切忌闭门造车，本地debug重中之重！！！


不可以用try-catch判断，因为不会有报错（控制台输出可以很轻易地发现）
用service-host中的函数做判断，提取有用部分，如GetScreenScaleFactor()函数，addin没有更新会报错，可以通过这个判断
```
public getDisplayScaleFactor() {
    try {
      return this.windowManage?.GetScreenScaleFactor();
    } catch {
      const displayScale = (window.devicePixelRatio * 100) / USER_DEFAULT_SCREEN_DPI;
      return round(displayScale, 2);
    }
  }
```