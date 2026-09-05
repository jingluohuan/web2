# CSS 全面深入学习教程

## 第一部分：CSS 基础与核心概念

### 1.1 CSS 简介与历史

**CSS 发展历程：**

- 
1996年：CSS1 发布

- 
1998年：CSS2 发布

- 
1999年：CSS3 开始模块化发展

- 
2011年：CSS2.1 成为推荐标准

- 
至今：CSS3 各模块持续发展

**CSS 工作原理：**

```
/* 选择器 { 属性: 值; } */
selector {
    property: value;
    property: value;
}
```

### 1.2 CSS 引入方式

```

    
    - 
    
    
    
        body {
            margin: 0;
            padding: 20px;
        }
    
    
    
    
        @import url('imported.css');
    

    
    

        内联样式示例
    

```

### 1.3 选择器详解

#### 基础选择器

```
/* 元素选择器 */
div {
    color: blue;
}

/* 类选择器 */
.class-name {
    background: yellow;
}

/* ID 选择器 */
#unique-id {
    border: 1px solid black;
}

/* 通用选择器 */
* {
    box-sizing: border-box;
}

/* 属性选择器 */
[type="text"] {
    border: 1px solid #ccc;
}

[class^="btn-"] { /* 以 btn- 开头的类 */
    padding: 10px;
}

[href$=".pdf"] { /* 以 .pdf 结尾的 href */
    color: red;
}
```

#### 组合选择器

```
/* 后代选择器 */
.container p {
    margin: 10px 0;
}

/* 子元素选择器 */
.parent > .child {
    border: 1px solid;
}

/* 相邻兄弟选择器 */
h1 + p {
    margin-top: 0;
}

/* 通用兄弟选择器 */
h1 ~ p {
    color: gray;
}
```

#### 伪类选择器

```
/* 链接状态 */
a:link { color: blue; }
a:visited { color: purple; }
a:hover { color: red; }
a:active { color: green; }

/* 表单状态 */
input:focus {
    outline: 2px solid blue;
}

input:disabled {
    opacity: 0.5;
}

input:checked {
    background: green;
}

/* 结构伪类 */
li:first-child { font-weight: bold; }
li:last-child { border-bottom: none; }
li:nth-child(odd) { background: #f5f5f5; }
li:nth-child(even) { background: white; }
li:nth-child(3n) { color: red; }

/* 内容伪类 */
p:empty {
    display: none;
}

div:not(.special) {
    border: 1px solid #ccc;
}
```

#### 伪元素选择器

```
/* 首字母 */
p::first-letter {
    font-size: 2em;
    float: left;
}

/* 首行 */
p::first-line {
    font-weight: bold;
}

/* 前后内容 */
blockquote::before {
    content: "“";
    font-size: 3em;
}

blockquote::after {
    content: "”";
    font-size: 3em;
}

/* 选中文本 */
::selection {
    background: yellow;
    color: black;
}

/* 占位符 */
input::placeholder {
    color: #999;
}
```

### 1.4 特异性与层叠

#### 特异性计算

```
/* 特异性: 0,0,0,1 */
div { }

/* 特异性: 0,0,1,0 */
.class { }

/* 特异性: 0,1,0,0 */
#id { }

/* 特异性: 1,0,0,0 */
内联样式

/* 计算示例 */
div.container p#intro { } /* 0,1,1,2 */
#header .nav li.active { } /* 0,1,2,1 */
```

#### 层叠规则

```
/* 来源重要性 */
用户样式表 !important > 作者样式表 !important > 作者样式表 > 用户样式表 > 浏览器默认样式

/* 特异性比较 */
#content div > p { } /* 胜出 - 更高特异性 */
.container p { }

/* 源代码顺序 */
p { color: blue; }
p { color: red; } /* 生效 - 后定义 */
```

#### !important 使用

```
/* 谨慎使用 !important */
.button {
    background: blue !important;
}

/* 覆盖 !important 的唯一方法是使用更高特异性的 !important */
#special.button {
    background: red !important;
}
```

## 第二部分：盒模型与布局

### 2.1 盒模型详解

#### 标准盒模型

```
.box {
    width: 300px;        /* 内容宽度 */
    height: 200px;       /* 内容高度 */
    padding: 20px;       /* 内边距 */
    border: 10px solid;  /* 边框 */
    margin: 30px;        /* 外边距 */
    
    /* 总宽度 = 300 + 20*2 + 10*2 = 360px */
    /* 总高度 = 200 + 20*2 + 10*2 = 260px */
}
```

#### 怪异盒模型

```
.weird-box {
    box-sizing: border-box;
    width: 300px;        /* 总宽度 */
    height: 200px;       /* 总高度 */
    padding: 20px;
    border: 10px solid;
    margin: 30px;
    
    /* 内容宽度 = 300 - 20*2 - 10*2 = 240px */
    /* 内容高度 = 200 - 20*2 - 10*2 = 140px */
}
```

#### 外边距折叠

```
/* 相邻元素外边距折叠 */
.block-a {
    margin-bottom: 20px;
}

.block-b {
    margin-top: 30px;
}
/* 实际间距 = max(20px, 30px) = 30px */

/* 父子元素外边距折叠 */
.parent {
    margin-top: 20px;
}

.child {
    margin-top: 30px;
}
/* 实际外边距 = max(20px, 30px) = 30px */

/* 防止外边距折叠 */
.parent {
    padding: 1px; /* 任意 padding */
    /* 或 */
    border: 1px solid transparent;
    /* 或 */
    overflow: auto;
}
```

### 2.2 显示模式

```
/* 块级元素 */
.block {
    display: block;
    width: 100%; /* 默认宽度 */
}

/* 内联元素 */
.inline {
    display: inline;
    /* 不能设置宽高 */
    /* 上下外边距无效 */
}

/* 内联块元素 */
.inline-block {
    display: inline-block;
    width: 100px; /* 可以设置宽高 */
    height: 50px;
}

/* 弹性项目 */
.flex-item {
    display: flex;
}

/* 网格项目 */
.grid-item {
    display: grid;
}

/* 无显示 */
.hidden {
    display: none; /* 完全从文档流移除 */
}

.visually-hidden {
    visibility: hidden; /* 保留空间 */
}
```

### 2.3 传统布局方法

#### 浮动布局

```
.container {
    overflow: auto; /* 清除浮动 */
}

.float-left {
    float: left;
    width: 200px;
    margin-right: 20px;
}

.float-right {
    float: right;
    width: 150px;
    margin-left: 20px;
}

/* 清除浮动 */
.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

/* 现代清除浮动 */
.container {
    display: flow-root; /* 创建BFC */
}
```

#### 定位布局

```
/* 相对定位 */
.relative {
    position: relative;
    top: 10px;
    left: 20px;
    /* 保留原始空间 */
}

/* 绝对定位 */
.absolute {
    position: absolute;
    top: 0;
    right: 0;
    /* 相对于最近定位祖先 */
}

/* 固定定位 */
.fixed {
    position: fixed;
    bottom: 20px;
    right: 20px;
    /* 相对于视口 */
}

/* 粘性定位 */
.sticky {
    position: sticky;
    top: 0;
    /* 在滚动时固定 */
}

/* z-index 控制堆叠顺序 */
.modal {
    position: fixed;
    z-index: 1000;
}

.backdrop {
    position: fixed;
    z-index: 999;
}
```

## 第三部分：Flexbox 布局

### 3.1 Flex 容器属性

```
.container {
    display: flex;
    
    /* 主轴方向 */
    flex-direction: row;           /* 默认：左到右 */
    flex-direction: row-reverse;   /* 右到左 */
    flex-direction: column;        /* 上到下 */
    flex-direction: column-reverse;/* 下到上 */
    
    /* 换行 */
    flex-wrap: nowrap;            /* 默认：不换行 */
    flex-wrap: wrap;              /* 换行 */
    flex-wrap: wrap-reverse;      /* 反向换行 */
    
    /* 简写 */
    flex-flow: row wrap;
    
    /* 主轴对齐 */
    justify-content: flex-start;   /* 默认：起始对齐 */
    justify-content: flex-end;     /* 末尾对齐 */
    justify-content: center;       /* 居中对齐 */
    justify-content: space-between;/* 两端对齐 */
    justify-content: space-around; /* 周围留空 */
    justify-content: space-evenly; /* 均匀分布 */
    
    /* 交叉轴对齐 */
    align-items: stretch;         /* 默认：拉伸 */
    align-items: flex-start;      /* 起始对齐 */
    align-items: flex-end;        /* 末尾对齐 */
    align-items: center;          /* 居中对齐 */
    align-items: baseline;        /* 基线对齐 */
    
    /* 多行对齐 */
    align-content: stretch;       /* 默认：拉伸 */
    align-content: flex-start;
    align-content: flex-end;
    align-content: center;
    align-content: space-between;
    align-content: space-around;
}
```

### 3.2 Flex 项目属性

```
.item {
    /* 顺序 */
    order: 0;                    /* 默认：0 */
    order: 1;                    /* 数字越大越靠后 */
    
    /* 放大比例 */
    flex-grow: 0;                /* 默认：不放大 */
    flex-grow: 1;                /* 等分剩余空间 */
    
    /* 缩小比例 */
    flex-shrink: 1;              /* 默认：可缩小 */
    flex-shrink: 0;              /* 不缩小 */
    
    /* 基础尺寸 */
    flex-basis: auto;            /* 默认：自动 */
    flex-basis: 200px;           /* 固定宽度 */
    flex-basis: 50%;             /* 百分比 */
    
    /* 简写 */
    flex: 0 1 auto;              /* 默认值 */
    flex: 1;                     /* flex-grow: 1 */
    flex: 0 0 200px;             /* 固定200px */
    flex: 2 1 0%;                /* 比例布局 */
    
    /* 单个项目对齐 */
    align-self: auto;            /* 默认：继承 align-items */
    align-self: flex-start;
    align-self: flex-end;
    align-self: center;
    align-self: stretch;
}
```

### 3.3 实际布局示例

```
/* 导航栏 */
.nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem 2rem;
}

.logo {
    flex: 0 0 auto;
}

.nav-links {
    display: flex;
    gap: 2rem;
    list-style: none;
}

/* 卡片布局 */
.cards {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
}

.card {
    flex: 1 1 300px;
    /* 最小宽度300px，可伸缩 */
}

/* 圣杯布局 */
.holy-grail {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}

.header, .footer {
    flex: 0 0 auto;
}

.main {
    display: flex;
    flex: 1;
}

.sidebar {
    flex: 0 0 250px;
}

.content {
    flex: 1;
}
```

## 第四部分：Grid 布局

### 4.1 Grid 容器属性

```
.container {
    display: grid;
    
    /* 定义列 */
    grid-template-columns: 100px 200px 100px;
    grid-template-columns: 1fr 2fr 1fr;           /* 分数单位 */
    grid-template-columns: repeat(3, 1fr);        /* 重复模式 */
    grid-template-columns: minmax(100px, 1fr) 2fr;/* 最小最大 */
    grid-template-columns: [col1] 1fr [col2] 1fr [col3] 1fr; /* 命名线 */
    
    /* 定义行 */
    grid-template-rows: 50px 100px 50px;
    grid-template-rows: min-content max-content;
    grid-template-rows: auto;                     /* 自动行高 */
    
    /* 简写 */
    grid-template: 
        "header header" 80px
        "sidebar content" 1fr
        "footer footer" 60px 
        / 250px 1fr;
    
    /* 自动行列 */
    grid-auto-columns: 100px;
    grid-auto-rows: minmax(100px, auto);
    grid-auto-flow: row;          /* 默认：行优先 */
    grid-auto-flow: column;       /* 列优先 */
    grid-auto-flow: row dense;    /* 密集填充 */
    
    /* 间距 */
    gap: 20px;                    /* 行列间距 */
    row-gap: 10px;
    column-gap: 30px;
    
    /* 对齐 */
    justify-items: stretch;       /* 单元格水平对齐 */
    align-items: stretch;         /* 单元格垂直对齐 */
    justify-content: start;       /* 网格水平对齐 */
    align-content: start;         /* 网格垂直对齐 */
}
```

### 4.2 Grid 项目属性

```
.item {
    /* 位置 */
    grid-column-start: 1;
    grid-column-end: 3;
    grid-row-start: 1;
    grid-row-end: 2;
    
    /* 简写 */
    grid-column: 1 / 3;
    grid-row: 1 / 2;
    grid-area: 1 / 1 / 2 / 3;    /* row-start / col-start / row-end / col-end */
    
    /* 命名区域 */
    grid-area: header;
    
    /* 跨行列 */
    grid-column: span 2;          /* 跨2列 */
    grid-row: span 3;             /* 跨3行 */
    
    /* 单个项目对齐 */
    justify-self: stretch;        /* 水平对齐 */
    align-self: stretch;          /* 垂直对齐 */
}
```

### 4.3 高级 Grid 功能

```
/* 响应式网格 */
.container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
}

/* 网格模板区域 */
.page {
    display: grid;
    grid-template-areas:
        "header header"
        "sidebar content"
        "footer footer";
    grid-template-rows: 80px 1fr 60px;
    grid-template-columns: 250px 1fr;
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.content { grid-area: content; }
.footer { grid-area: footer; }

/* 子网格 (CSS Level 3) */
.grid {
    display: grid;
    grid-template-columns: subgrid;
    grid-template-rows: subgrid;
}

/* 显式与隐式网格 */
.container {
    display: grid;
    grid-template-columns: repeat(2, 1fr);  /* 显式：2列 */
    grid-auto-columns: 100px;               /* 隐式列宽 */
    grid-auto-rows: 50px;                   /* 隐式行高 */
}
```

## 第五部分：响应式设计

### 5.1 媒体查询

```
/* 基础媒体查询语法 */
@media media-type and (media-feature) {
    /* CSS 规则 */
}

/* 视口宽度 */
@media (max-width: 768px) {
    .container {
        padding: 10px;
    }
}

@media (min-width: 769px) and (max-width: 1024px) {
    .container {
        padding: 20px;
    }
}

/* 设备特性 */
@media (orientation: portrait) {
    /* 竖屏样式 */
}

@media (orientation: landscape) {
    /* 横屏样式 */
}

@media (prefers-color-scheme: dark) {
    /* 深色模式 */
    body {
        background: #1a1a1a;
        color: white;
    }
}

@media (prefers-reduced-motion: reduce) {
    /* 减少动画 */
    * {
        animation: none;
        transition: none;
    }
}

/* 打印样式 */
@media print {
    .no-print {
        display: none;
    }
    
    body {
        font-size: 12pt;
        line-height: 1.4;
    }
}
```

### 5.2 响应式单位

```
.container {
    /* 相对单位 */
    font-size: 1rem;          /* 根元素字体大小 */
    font-size: 1em;           /* 父元素字体大小 */
    
    /* 视口单位 */
    width: 100vw;             /* 视口宽度 */
    height: 100vh;            /* 视口高度 */
    font-size: 5vmin;         /* 视口较小尺寸的5% */
    font-size: 5vmax;         /* 视口较大尺寸的5% */
    
    /* 容器查询单位 (CSS Level 4) */
    width: 100cqi;            /* 内联尺寸的百分比 */
    height: 100cqb;           /* 块尺寸的百分比 */
}

/* 流体排版 */
:root {
    font-size: clamp(1rem, 2.5vw, 1.5rem);
}

h1 {
    font-size: clamp(2rem, 5vw, 3rem);
}

/* 响应式间距 */
.section {
    padding: clamp(1rem, 5vw, 3rem);
}
```

### 5.3 响应式策略

```
/* 移动优先 */
.container {
    padding: 1rem;
}

@media (min-width: 768px) {
    .container {
        padding: 2rem;
    }
}

/* 桌面优先 */
.container {
    padding: 3rem;
}

@media (max-width: 767px) {
    .container {
        padding: 1rem;
    }
}

/* 弹性网格 */
.grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 1rem;
}

/* 响应式图片 */
img {
    max-width: 100%;
    height: auto;
}

.responsive-bg {
    background-image: url('small.jpg');
}

@media (min-width: 768px) {
    .responsive-bg {
        background-image: url('large.jpg');
    }
}
```

## 第六部分：视觉效果与动画

### 6.1 变换 (Transform)

```
.element {
    /* 2D 变换 */
    transform: translateX(100px);     /* X轴移动 */
    transform: translateY(50px);      /* Y轴移动 */
    transform: translate(100px, 50px);/* XY移动 */
    
    transform: scale(1.5);            /* 缩放 */
    transform: scaleX(2);             /* X轴缩放 */
    transform: scaleY(0.5);           /* Y轴缩放 */
    
    transform: rotate(45deg);         /* 旋转 */
    transform: rotateX(45deg);        /* X轴旋转 */
    transform: rotateY(45deg);        /* Y轴旋转 */
    
    transform: skew(30deg, 10deg);    /* 倾斜 */
    
    /* 3D 变换 */
    transform: perspective(500px) rotateY(45deg);
    transform: translate3d(100px, 50px, 0);
    
    /* 变换原点 */
    transform-origin: center center;  /* 默认 */
    transform-origin: left top;
    transform-origin: 20% 80%;
    
    /* 多重变换 */
    transform: translateX(100px) rotate(45deg) scale(1.2);
}
```

### 6.2 过渡 (Transition)

```
.button {
    background: blue;
    transition: all 0.3s ease-in-out;
    
    /* 单独属性 */
    transition-property: background, transform;
    transition-duration: 0.3s, 0.5s;
    transition-timing-function: ease-in, ease-out;
    transition-delay: 0s, 0.1s;
    
    /* 简写 */
    transition: background 0.3s ease-in-out 0.1s;
}

.button:hover {
    background: darkblue;
    transform: scale(1.1);
}

/* 时序函数 */
.element {
    transition-timing-function: linear;
    transition-timing-function: ease;
    transition-timing-function: ease-in;
    transition-timing-function: ease-out;
    transition-timing-function: ease-in-out;
    transition-timing-function: cubic-bezier(0.1, 0.7, 1.0, 0.1);
    transition-timing-function: steps(4, jump-end);
}
```

### 6.3 动画 (Animation)

```
/* 关键帧定义 */
@keyframes slideIn {
    from {
        transform: translateX(-100%);
        opacity: 0;
    }
    
    50% {
        opacity: 0.5;
    }
    
    to {
        transform: translateX(0);
        opacity: 1;
    }
}

@keyframes bounce {
    0%, 20%, 53%, 80%, 100% {
        transform: translate3d(0,0,0);
    }
    
    40%, 43% {
        transform: translate3d(0,-30px,0);
    }
    
    70% {
        transform: translate3d(0,-15px,0);
    }
    
    90% {
        transform: translate3d(0,-4px,0);
    }
}

/* 动画应用 */
.element {
    animation-name: slideIn;
    animation-duration: 0.5s;
    animation-timing-function: ease-out;
    animation-delay: 0.2s;
    animation-iteration-count: 1;
    animation-direction: normal;
    animation-fill-mode: both;
    animation-play-state: running;
    
    /* 简写 */
    animation: slideIn 0.5s ease-out 0.2s 1 normal both running;
}

/* 动画控制 */
.paused {
    animation-play-state: paused;
}
```

### 6.4 滤镜与混合模式

```
.image {
    /* 滤镜效果 */
    filter: blur(5px);
    filter: brightness(150%);
    filter: contrast(200%);
    filter: grayscale(100%);
    filter: hue-rotate(90deg);
    filter: invert(100%);
    filter: opacity(50%);
    filter: saturate(200%);
    filter: sepia(100%);
    filter: drop-shadow(5px 5px 10px rgba(0,0,0,0.5));
    
    /* 多重滤镜 */
    filter: grayscale(50%) blur(2px) contrast(150%);
}

/* 混合模式 */
.overlay {
    mix-blend-mode: multiply;
    mix-blend-mode: screen;
    mix-blend-mode: overlay;
    mix-blend-mode: darken;
    mix-blend-mode: lighten;
    mix-blend-mode: color-dodge;
    mix-blend-mode: color-burn;
    mix-blend-mode: hard-light;
    mix-blend-mode: soft-light;
    mix-blend-mode: difference;
    mix-blend-mode: exclusion;
    mix-blend-mode: hue;
    mix-blend-mode: saturation;
    mix-blend-mode: color;
    mix-blend-mode: luminosity;
}

/* 背景混合模式 */
.hero {
    background-image: url('image.jpg'), linear-gradient(45deg, blue, red);
    background-blend-mode: overlay;
}
```

## 第七部分：现代 CSS 特性

### 7.1 CSS 自定义属性 (变量)

```
/* 定义变量 */
:root {
    --primary-color: #007bff;
    --secondary-color: #6c757d;
    --success-color: #28a745;
    --font-size-sm: 0.875rem;
    --font-size-base: 1rem;
    --font-size-lg: 1.25rem;
    --spacing-xs: 0.25rem;
    --spacing-sm: 0.5rem;
    --spacing-md: 1rem;
    --spacing-lg: 1.5rem;
    --border-radius: 0.375rem;
    --box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    --transition: all 0.3s ease;
}

/* 使用变量 */
.button {
    background: var(--primary-color);
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--border-radius);
    font-size: var(--font-size-base);
    transition: var(--transition);
}

.button:hover {
    background: color-mix(in srgb, var(--primary-color) 80%, black);
}

/* 备用值 */
.element {
    color: var(--undefined-var, fallback-value);
    background: var(--custom-bg, var(--default-bg, white));
}

/* 在 JavaScript 中使用 */
document.documentElement.style.setProperty('--primary-color', '#ff0000');
```

### 7.2 容器查询

```
/* 定义容器 */
.card-container {
    container-type: inline-size;
    container-name: card-container;
}

/* 简写 */
.card-container {
    container: card-container / inline-size;
}

/* 容器查询 */
@container card-container (min-width: 400px) {
    .card {
        display: flex;
        gap: 1rem;
    }
    
    .card-image {
        flex: 0 0 150px;
    }
    
    .card-content {
        flex: 1;
    }
}

@container (max-width: 399px) {
    .card {
        text-align: center;
    }
}

/* 容器查询单位 */
.card {
    padding: 1rem;
    
    @container (min-width: 500px) {
        padding: 2cqi; /* 容器内联尺寸的2% */
    }
}
```

### 7.3 层叠层 (CSS Layers)

```
/* 定义层 */
@layer base, components, utilities;

/* 基础层 */
@layer base {
    h1 {
        font-size: 2rem;
        margin-bottom: 1rem;
    }
    
    p {
        line-height: 1.6;
        margin-bottom: 1rem;
    }
}

/* 组件层 */
@layer components {
    .button {
        padding: 0.5rem 1rem;
        border: 1px solid #ccc;
        border-radius: 0.25rem;
        background: white;
    }
    
    .button-primary {
        background: blue;
        color: white;
        border-color: blue;
    }
}

/* 工具层 */
@layer utilities {
    .text-center {
        text-align: center;
    }
    
    .mt-4 {
        margin-top: 1rem;
    }
}

/* 导入到层 */
@import url('components.css') layer(components);
```

### 7.4 现代选择器

```
/* :is() 伪类 */
:is(header, footer, section) h1 {
    font-size: 2rem;
}

/* 等价于 */
header h1, footer h1, section h1 {
    font-size: 2rem;
}

/* :where() 伪类 */
:where(header, footer, section) h1 {
    font-size: 2rem;
    /* 特异性为0 */
}

/* :has() 父类选择器 */
.card:has(.featured) {
    border: 2px solid gold;
}

.menu:has(> li:hover) {
    background: #f5f5f5;
}

.form:has(input:invalid) .submit-btn {
    opacity: 0.5;
    pointer-events: none;
}

/* 逻辑组合 */
.card:is(.featured, .popular):has(img) {
    box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}
```

## 第八部分：排版与字体

### 8.1 字体属性

```
.text {
    /* 字体系列 */
    font-family: "Inter", -apple-system, BlinkMacSystemFont, sans-serif;
    font-family: serif, sans-serif, monospace, cursive, fantasy;
    
    /* 字体大小 */
    font-size: 16px;
    font-size: 1rem;
    font-size: 100%;
    font-size: larger;
    font-size: smaller;
    
    /* 字体粗细 */
    font-weight: normal;
    font-weight: bold;
    font-weight: 100; /* 100-900 */
    font-weight: lighter;
    font-weight: bolder;
    
    /* 字体样式 */
    font-style: normal;
    font-style: italic;
    font-style: oblique;
    
    /* 字体变体 */
    font-variant: normal;
    font-variant: small-caps;
    font-variant-numeric: oldstyle-nums;
    font-variant-ligatures: common-ligatures;
    
    /* 简写 */
    font: italic small-caps bold 1.2rem/1.5 "Helvetica", sans-serif;
}
```

### 8.2 文本属性

```
.text {
    /* 颜色 */
    color: #333;
    color: rgb(51, 51, 51);
    color: hsl(0, 0%, 20%);
    
    /* 对齐 */
    text-align: left;
    text-align: right;
    text-align: center;
    text-align: justify;
    text-align: start;
    text-align: end;
    
    /* 装饰 */
    text-decoration: none;
    text-decoration: underline;
    text-decoration: overline;
    text-decoration: line-through;
    text-decoration: underline wavy red;
    
    /* 变换 */
    text-transform: none;
    text-transform: uppercase;
    text-transform: lowercase;
    text-transform: capitalize;
    
    /* 间距 */
    letter-spacing: 0.1em;
    word-spacing: 0.2em;
    line-height: 1.5;
    
    /* 阴影 */
    text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
    text-shadow: 
        1px 1px 0 #000,
        -1px -1px 0 #000,
        1px -1px 0 #000,
        -1px 1px 0 #000;
    
    /* 溢出 */
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    
    /* 换行 */
    word-wrap: break-word;
    word-break: break-all;
    overflow-wrap: anywhere;
    
    /* 书写模式 */
    writing-mode: horizontal-tb;
    writing-mode: vertical-rl;
    writing-mode: vertical-lr;
}
```

### 8.3 现代排版特性

```
.article {
    /* 字体特性设置 */
    font-feature-settings: "kern", "liga", "clig", "calt";
    font-feature-settings: "smcp", "onum"; /* 小型大写字母，旧式数字 */
    
    /* 字体变体 */
    font-variant-numeric: lining-nums;      /* 等高数字 */
    font-variant-numeric: oldstyle-nums;    /* 旧式数字 */
    font-variant-numeric: proportional-nums;/* 比例数字 */
    font-variant-numeric: tabular-nums;     /* 等宽数字 */
    font-variant-numeric: diagonal-fractions;/* 分数 */
    font-variant-numeric: stacked-fractions;
    
    /* 字体变体帽子 */
    font-variant-caps: small-caps;          /* 小型大写 */
    font-variant-caps: all-small-caps;      /* 全部小型大写 */
    font-variant-caps: petite-caps;         /* 特小型大写 */
    font-variant-caps: all-petite-caps;     /* 全部特小型大写 */
    font-variant-caps: unicase;             /* 混合大小写 */
    
    /* 连字 */
    font-variant-ligatures: common-ligatures;       /* 普通连字 */
    font-variant-ligatures: no-common-ligatures;    /* 无普通连字 */
    font-variant-ligatures: discretionary-ligatures;/* 自由连字 */
    font-variant-ligatures: historical-ligatures;   /* 历史连字 */
    font-variant-ligatures: contextual;             /* 上下文替代 */
    
    /* 文本渲染 */
    text-rendering: auto;
    text-rendering: optimizeSpeed;
    text-rendering: optimizeLegibility;
    text-rendering: geometricPrecision;
    
    /* 字体合成 */
    font-synthesis: weight style small-caps;
    font-synthesis: none;
}
```

### 8.4 可变字体

```
@font-face {
    font-family: 'InterVariable';
    src: url('Inter.var.woff2') format('woff2-variations');
    font-weight: 100 900;
    font-stretch: 75% 125%;
    font-style: oblique 0deg 10deg;
}

.heading {
    font-family: 'InterVariable', sans-serif;
    font-weight: 650; /* 任意值在100-900之间 */
    font-stretch: 110%;
}

.dynamic-text {
    font-variation-settings: 
        "wght" 400,
        "wdth" 100,
        "slnt" 0,
        "opsz" 16;
    
    transition: font-variation-settings 0.3s ease;
}

.dynamic-text:hover {
    font-variation-settings: 
        "wght" 700,
        "wdth" 120,
        "slnt" -10,
        "opsz" 24;
}
```

## 第九部分：高级技巧与最佳实践

### 9.1 CSS 架构方法论

#### BEM (Block Element Modifier)

```
/* 块 */
.button { }

/* 元素 */
.button__icon { }
.button__text { }

/* 修饰符 */
.button--primary { }
.button--large { }
.button--disabled { }

/* 使用示例 */

    ★
    点击我

```

#### SMACSS (可扩展模块化CSS)

```
/* 基础规则 */
html, body { }
input, textarea { }

/* 布局规则 */
.l-header { }
.l-footer { }
.l-grid { }

/* 模块规则 */
.module { }
.module-title { }
.module-body { }

/* 状态规则 */
.is-hidden { }
.is-active { }
.has-error { }

/* 主题规则 */
.theme-dark { }
.theme-light { }
```

#### ITCSS (倒三角形CSS)

```
/* 设置层 - 变量、配置 */
:root { --color-primary: blue; }

/* 工具层 - mixins、函数 */
@mixin sr-only { }

/* 通用层 - 重置、规范化 */
* { box-sizing: border-box; }

/* 元素层 - 原生HTML元素样式 */
h1, p, a { }

/* 对象层 - 通用设计模式 */
.o-container { }
.o-grid { }

/* 组件层 - 具体UI组件 */
.c-button { }
.c-card { }

/* 工具层 - 工具类 */
.u-text-center { }
.u-mt-4 { }
```

### 9.2 性能优化

```
/* 减少重绘重排 */
.optimized {
    /* 使用 transform 和 opacity 进行动画 */
    transform: translateX(100px);
    opacity: 0.5;
    
    /* 避免 */
    margin-left: 100px;
    width: 200px;
}

/* 硬件加速 */
.accelerated {
    transform: translateZ(0);
    backface-visibility: hidden;
    perspective: 1000px;
}

/* 选择器性能 */
/* 快 - 直接选择器 */
.button { }
#header { }

/* 慢 - 复杂选择器 */
nav ul li a { }
div > p + span { }

/* 内容可见性 */
.lazy-content {
    content-visibility: auto;
    contain-intrinsic-size: 0 500px;
}

/* 减少布局抖动 */
.stable-layout {
    width: 100%;
    height: 200px;
    /* 避免依赖内容的高度 */
}
```

### 9.3 可访问性

```
/* 焦点样式 */
button:focus {
    outline: 2px solid #005fcc;
    outline-offset: 2px;
}

/* 减少动画 */
@media (prefers-reduced-motion: reduce) {
    * {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}

/* 高对比度 */
@media (prefers-contrast: high) {
    .text {
        color: black;
        background: white;
    }
}

/* 屏幕阅读器专用 */
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}

/* 跳过链接 */
.skip-link {
    position: absolute;
    top: -40px;
    left: 6px;
    padding: 8px;
    background: #000;
    color: #fff;
    text-decoration: none;
    z-index: 1000;
}

.skip-link:focus {
    top: 6px;
}
```

## 第十部分：实际项目应用

### 10.1 实用工具类

```
/* 间距工具 */
.m-0 { margin: 0; }
.m-1 { margin: 0.25rem; }
.m-2 { margin: 0.5rem; }
.m-3 { margin: 1rem; }
.m-4 { margin: 1.5rem; }
.m-5 { margin: 3rem; }

.mt-0 { margin-top: 0; }
.mb-0 { margin-bottom: 0; }
.ml-0 { margin-left: 0; }
.mr-0 { margin-right: 0; }
.mx-0 { margin-left: 0; margin-right: 0; }
.my-0 { margin-top: 0; margin-bottom: 0; }

.p-0 { padding: 0; }
.p-1 { padding: 0.25rem; }
/* 类似定义其他padding */

/* 显示工具 */
.d-none { display: none; }
.d-block { display: block; }
.d-inline { display: inline; }
.d-inline-block { display: inline-block; }
.d-flex { display: flex; }
.d-grid { display: grid; }

/* 文本工具 */
.text-left { text-align: left; }
.text-center { text-align: center; }
.text-right { text-align: right; }
.text-justify { text-align: justify; }

.text-uppercase { text-transform: uppercase; }
.text-lowercase { text-transform: lowercase; }
.text-capitalize { text-transform: capitalize; }

.font-weight-normal { font-weight: normal; }
.font-weight-bold { font-weight: bold; }
.font-italic { font-style: italic; }

/* 颜色工具 */
.text-primary { color: var(--primary-color); }
.text-secondary { color: var(--secondary-color); }
.text-success { color: var(--success-color); }
.text-danger { color: var(--danger-color); }
.text-warning { color: var(--warning-color); }
.text-info { color: var(--info-color); }
.text-light { color: var(--light-color); }
.text-dark { color: var(--dark-color); }
.text-muted { color: var(--muted-color); }

.bg-primary { background-color: var(--primary-color); }
.bg-secondary { background-color: var(--secondary-color); }
/* 类似定义其他背景色 */

/* 响应式工具 */
@media (min-width: 576px) {
    .d-sm-none { display: none; }
    .d-sm-block { display: block; }
    /* 其他sm断点工具 */
}

@media (min-width: 768px) {
    .d-md-none { display: none; }
    .d-md-block { display: block; }
    /* 其他md断点工具 */
}

@media (min-width: 992px) {
    .d-lg-none { display: none; }
    .d-lg-block { display: block; }
    /* 其他lg断点工具 */
}

@media (min-width: 1200px) {
    .d-xl-none { display: none; }
    .d-xl-block { display: block; }
    /* 其他xl断点工具 */
}
```

### 10.2 组件库示例

```
/* 按钮组件 */
.btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    padding: 0.5rem 1rem;
    border: 1px solid transparent;
    border-radius: 0.375rem;
    font-size: 0.875rem;
    font-weight: 500;
    line-height: 1.5;
    text-decoration: none;
    cursor: pointer;
    transition: all 0.15s ease-in-out;
    user-select: none;
}

.btn:focus {
    outline: 2px solid transparent;
    outline-offset: 2px;
    box-shadow: 0 0 0 2px #fff, 0 0 0 4px #007bff;
}

.btn:disabled {
    opacity: 0.6;
    pointer-events: none;
}

/* 按钮变体 */
.btn-primary {
    background: var(--primary-color);
    color: white;
    border-color: var(--primary-color);
}

.btn-primary:hover {
    background: color-mix(in srgb, var(--primary-color) 85%, black);
    border-color: color-mix(in srgb, var(--primary-color) 85%, black);
}

.btn-secondary {
    background: var(--secondary-color);
    color: white;
    border-color: var(--secondary-color);
}

.btn-outline-primary {
    background: transparent;
    color: var(--primary-color);
    border-color: var(--primary-color);
}

.btn-outline-primary:hover {
    background: var(--primary-color);
    color: white;
}

/* 按钮尺寸 */
.btn-sm {
    padding: 0.25rem 0.5rem;
    font-size: 0.75rem;
}

.btn-lg {
    padding: 0.75rem 1.5rem;
    font-size: 1rem;
}

/* 卡片组件 */
.card {
    background: white;
    border: 1px solid var(--border-color);
    border-radius: 0.5rem;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
    overflow: hidden;
}

.card-header {
    padding: 1rem 1.5rem;
    border-bottom: 1px solid var(--border-color);
    background: var(--light-bg);
}

.card-body {
    padding: 1.5rem;
}

.card-footer {
    padding: 1rem 1.5rem;
    border-top: 1px solid var(--border-color);
    background: var(--light-bg);
}

/* 网格系统 */
.container {
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 1rem;
}

.row {
    display: flex;
    flex-wrap: wrap;
    margin: 0 -0.5rem;
}

.col {
    flex: 1 0 0%;
    padding: 0 0.5rem;
}

.col-auto {
    flex: 0 0 auto;
    width: auto;
}

.col-1 { flex: 0 0 auto; width: 8.333333%; }
.col-2 { flex: 0 0 auto; width: 16.666667%; }
.col-3 { flex: 0 0 auto; width: 25%; }
.col-4 { flex: 0 0 auto; width: 33.333333%; }
.col-5 { flex: 0 0 auto; width: 41.666667%; }
.col-6 { flex: 0 0 auto; width: 50%; }
.col-7 { flex: 0 0 auto; width: 58.333333%; }
.col-8 { flex: 0 0 auto; width: 66.666667%; }
.col-9 { flex: 0 0 auto; width: 75%; }
.col-10 { flex: 0 0 auto; width: 83.333333%; }
.col-11 { flex: 0 0 auto; width: 91.666667%; }
.col-12 { flex: 0 0 auto; width: 100%; }

/* 响应式网格 */
@media (min-width: 576px) {
    .col-sm-1 { flex: 0 0 auto; width: 8.333333%; }
    /* 其他sm列 */
}

@media (min-width: 768px) {
    .col-md-1 { flex: 0 0 auto; width: 8.333333%; }
    /* 其他md列 */
}

@media (min-width: 992px) {
    .col-lg-1 { flex: 0 0 auto; width: 8.333333%; }
    /* 其他lg列 */
}

@media (min-width: 1200px) {
    .col-xl-1 { flex: 0 0 auto; width: 8.333333%; }
    /* 其他xl列 */
}
```

这个完整的 CSS 教程涵盖了从基础到高级的所有重要概念。建议的学习路径：

**基础阶段**：掌握 1-2 部分的选择器、盒模型和传统布局

- 
**布局阶段**：深入学习 3-4 部分的 Flexbox 和 Grid 布局

- 
**响应式阶段**：掌握第 5 部分的响应式设计

- 
**高级阶段**：学习 6-10 部分的动画、现代特性和最佳实践

每个概念都建议通过实际项目练习，以加深理解和应用能力。
