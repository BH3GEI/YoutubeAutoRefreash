# YouTube Video Page Auto Refresh / YouTube 视频页面自动刷新

这是一个油猴脚本，用于在打开 YouTube 视频页面时自动刷新页面一次，防止无限刷新导致页面加载异常。  
This is a Tampermonkey script that refreshes a YouTube video page once upon the first load to prevent infinite refresh loops.

## 功能 / Features

- 自动检测 URL 包含 `/watch` 的视频页面  
  Automatically detects video pages (URLs containing `/watch`).
- 利用 sessionStorage 防止重复刷新  
  Uses sessionStorage to prevent repeated refreshes.
- 适用于首次加载页面时刷新，避免页面无法正常使用（尤其是安装了某些广告插件后导致的加载不正常）
  Designed to refresh only once on the first load to ensure the page loads properly.

## 安装 / Installation

1. 安装 [Tampermonkey](https://www.tampermonkey.net/) 扩展到您的浏览器。  
   Install the [Tampermonkey](https://www.tampermonkey.net/) extension in your browser.
2. 点击 Tampermonkey 扩展图标，选择“创建新脚本”。  
   Click the Tampermonkey icon and choose "Create a new script."
3. 删除模板代码，并将下方完整的代码复制粘贴到编辑器中，然后保存。  
   Remove any template code and paste the following complete script into the editor, then save.

```javascript
// ==UserScript==
// @name         YouTube Video Page Auto Refresh
// @namespace    http://tampermonkey.net/
// @version      0.2
// @description  仅在打开 YouTube 视频页面时执行一次自动刷新，防止页面无限刷新
// @author       Your Name
// @match        *://*.youtube.com/*
// @run-at       document-start
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    console.log("YT Refresh: 脚本已加载，当前URL:", window.location.href);

    // 如果URL中包含"/watch"，认为是视频页面
    if (window.location.pathname.indexOf("/watch") !== -1) {
        console.log("YT Refresh: 当前为视频页面。");

        // 利用 sessionStorage 防止无限刷新：如果未设置刷新标记，则刷新一次
        if (!sessionStorage.getItem('ytAutoRefreshDone')) {
            sessionStorage.setItem('ytAutoRefreshDone', 'true');
            console.log("YT Refresh: 未检测到刷新标记，执行刷新操作。");
            // reload(true) 传参确保从服务器重新加载
            window.location.reload(true);
        } else {
            console.log("YT Refresh: 已检测到刷新标记，取消刷新。");
            // 刷新后移除标记，便于下次手动刷新时生效
            sessionStorage.removeItem('ytAutoRefreshDone');
        }
    } else {
        console.log("YT Refresh: 当前页面不是视频页面，无需刷新。");
    }
})();
```

## 使用 / Usage

只需打开任何 YouTube 视频页面（例如：https://www.youtube.com/watch?v=xxx），脚本会自动检测并在首次加载时刷新一次页面。  
Simply open any YouTube video page (e.g., https://www.youtube.com/watch?v=xxx) and the script will detect the URL and refresh the page once on the initial load.

```` ▋
