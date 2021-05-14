# bliveproxy
B站直播websocket hook框架

* [greasyfork](https://greasyfork.org/zh-CN/scripts/417560-bliveproxy)
* [NGA帖子](https://bbs.nga.cn/read.php?tid=24449759)

## 使用例
### 1. 把同传弹幕改成顶端弹幕并高亮
```javascript
// ==UserScript==
// @name         bliveproxy-demo1
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  把同传弹幕改成顶端弹幕并高亮
// @author       xfgryujk
// @include      /https?:\/\/live\.bilibili\.com\/?\??.*/
// @include      /https?:\/\/live\.bilibili\.com\/\d+\??.*/
// @include      /https?:\/\/live\.bilibili\.com\/(blanc\/)?\d+\??.*/
// @run-at       document-start
// @require      https://cdn.jsdelivr.net/gh/google/brotli@5692e422da6af1e991f9182345d58df87866bc5e/js/decode.js
// @require      https://cdn.jsdelivr.net/npm/pako@2.0.3/dist/pako_inflate.min.js
// @require      https://greasyfork.org/scripts/417560-bliveproxy/code/bliveproxy.js?version=931022
// @grant        none
// ==/UserScript==

(function() {
  // 同传弹幕正则表达式
  const TRANSLATION_REG = /【/

  bliveproxy.addCommandHandler('DANMU_MSG', command => {
    let info = command.info
    if (!TRANSLATION_REG.test(info[1])) {
      return
    }

    // 显示模式：1滚动，4底部，5顶部
    info[0][1] = 5
    // 尺寸
    info[0][2] *= 1.5
    // 颜色
    info[0][3] = 0xFF0000
  })
})();

```

![截图1](https://github.com/xfgryujk/bliveproxy/blob/master/screenshots/1.png)

### 2. 把打赏全换成自己的名字
```javascript
// ==UserScript==
// @name         bliveproxy-demo2
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  把打赏全换成自己的名字
// @author       xfgryujk
// @include      /https?:\/\/live\.bilibili\.com\/?\??.*/
// @include      /https?:\/\/live\.bilibili\.com\/\d+\??.*/
// @include      /https?:\/\/live\.bilibili\.com\/(blanc\/)?\d+\??.*/
// @run-at       document-start
// @require      https://cdn.jsdelivr.net/gh/google/brotli@5692e422da6af1e991f9182345d58df87866bc5e/js/decode.js
// @require      https://cdn.jsdelivr.net/npm/pako@2.0.3/dist/pako_inflate.min.js
// @require      https://greasyfork.org/scripts/417560-bliveproxy/code/bliveproxy.js?version=931022
// @grant        none
// ==/UserScript==

(function() {
  // 用户名
  const USERNAME = 'xfgryujk'
  // 头像URL
  const AVATAR_URL = 'https://i0.hdslb.com/bfs/face/29b6be8aa611e70a3d3ac219cdaf5e72b604f2de.jpg'

  // 礼物
  bliveproxy.addCommandHandler('SEND_GIFT', command => {
    let data = command.data
    data.uname = USERNAME
    data.face = AVATAR_URL
  })
  bliveproxy.addCommandHandler('COMBO_SEND', command => {
    let data = command.data
    data.uname = USERNAME
  })
  // 醒目留言
  bliveproxy.addCommandHandler('SUPER_CHAT_MESSAGE', command => {
    let data = command.data
    data.user_info.uname = USERNAME
    data.user_info.face = AVATAR_URL
  })
  // 上舰
  // 其实上舰消息用的不是这条，懒得找了
  bliveproxy.addCommandHandler('GUARD_BUY', command => {
    let data = command.data
    data.username = USERNAME
  })
})();

```

![截图2](https://github.com/xfgryujk/bliveproxy/blob/master/screenshots/2.png)  
![截图3](https://github.com/xfgryujk/bliveproxy/blob/master/screenshots/3.png)
