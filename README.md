# 柏涵 · 个人名片

一个纯静态、可离线使用的个人名片网页，整体复刻《星露谷物语》(Stardew Valley) 的原生像素 UI 风格。

- 单文件：所有 HTML / CSS / JS 都在 `index.html` 里
- 零依赖：不使用任何外部图片、字体、CDN、UI 库或 npm 包
- 离线可用：双击 `index.html` 即可用浏览器打开

## 使用方法

1. 双击 `index.html`，用任意现代浏览器打开即可。
2. 手机查看：把 `index.html` 发到手机，或放到任意静态服务器后访问。

## 功能一览

- **名片卡片**：像素女生头像 + 昵称 + 一句话介绍 + 「兴趣爱好」标签（排球 / 电影 / 手工）
- **日间 / 夜间切换**：右上角按钮，背景、卡片、文字、按钮同步换色（可反复循环切换）
- **8-bit 音乐**：右上角音符按钮，用 Web Audio 离线生成星露谷风芯片音乐循环
- **农场日历**：季节（按月份自动）/ 天气 / 实时时间
- **技能面板**：像素图标 + 名称 + 分段进度条 + 等级数字
- **关于我**：生活向的个人简介
- **联系我**：虚拟邮箱 + 一键复制
- **农场日记**：待办清单，支持勾选 / 增删，浏览器本地保存
- **装饰**：太阳 / 月亮 / 云朵 / 星星 / 大树 / 蝴蝶 / 飞鸟 / 飘落花瓣 / 嫩芽小草

## 如何修改内容

所有可改的文字和配置都集中在 `index.html` 里，打开搜索对应关键词即可：

### 1. 昵称、一句话介绍、标签

在名片卡片区域（约在文件的 HTML 部分，搜索 `个人名片`）：

```html
<h1 class="name">柏涵</h1>                                  <!-- 昵称 -->
<p class="intro">正在山谷里种下新的种子...</p>               <!-- 一句话介绍 -->
<div class="tags">
  <span class="tag">排球</span>                               <!-- 兴趣标签 -->
  <span class="tag">电影</span>
  <span class="tag">手工</span>
</div>
```

### 2. 「关于我」内容

搜索 `关于我`，在 `about-body` 里按条修改，每条是：

```html
<p class="about-item"><span class="about-tag">介 绍</span>……文字……</p>
```

### 3. 技能

搜索 `var SKILLS`，数组里每一项对应一行技能：

```js
var SKILLS = [
  { name: '乐器', icon: '#i-note',   level: 8 },   // level 为 1~10
  { name: '体育', icon: '#i-ball',   level: 6 },
  // …… 增删改这里
];
```

- 想做成「满级」：加 `max: true`，进度条会变成整条黄色。
- 图标 id 在 `<defs>` 里定义（`i-note`、`i-ball` 等）。

### 4. 像素头像

搜索 `var AVATAR_MAP`。它是字符地图，每个字符对应一种颜色，`.` 为透明：

```js
var AVATAR_MAP = [
  ".....HHHHHHHH......",
  // ……
];
var AVATAR_PALETTE = {
  'H': '#6b4423',  // 头发
  'T': '#f06a9a',  // 辫子发绳
  // ……
};
```

改字符地图即可改小人造型，改 `AVATAR_PALETTE` 即可换颜色。

### 5. 整体配色

搜索 `:root`（日间）和 `body.night`（夜间），所有颜色都是 CSS 变量：

```css
:root {
  --sky-top: #6ec6e8;   /* 天空 */
  --panel: #f6e7bd;     /* 羊皮纸面板 */
  --ink: #3a2a16;       /* 文字 */
  /* …… */
}
```

### 6. 联系邮箱

搜索 `contactMail`，当前是虚拟邮箱：

```html
<p class="contact-mail" id="contactMail">hello@baihan.farm</p>
```

### 7. 音乐旋律

搜索 `var MELODY` 和 `var BASS`，改成想要的音符频率即可换曲。

## 数据保存说明

- 「农场日记」存于浏览器 `localStorage`（键名 `stardew_diary`）。
- 更换浏览器或清除站点数据会丢失；纯本地，不上传任何信息。
- 页面内不含手机号、学号、密钥、token 等隐私信息。

## 技术说明

- 像素效果：CSS `image-rendering: pixelated` + SVG `shape-rendering: crispEdges`
- 所有装饰图形为内联 SVG 或纯 CSS，无外部素材
- 音频：Web Audio API 实时合成，无音频文件
