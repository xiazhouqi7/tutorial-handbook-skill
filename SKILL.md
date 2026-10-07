---
name: xiazhouqi-tutorial-handbook
version: 1.1.0
description: 将教学截图、手机设置步骤、软件教程等内容，转化为「下周七.教程手帐」风格的移动端优先网页。视觉关键词：雾蓝、奶白、少量暖黄、毛玻璃、柔和立体阴影、手机界面仿真、便利贴、手绘标记、自然连续滚动。
---

# 下周七.教程手帐｜Xiazhouqi Tutorial Handbook

## 0. 视觉参考图（强制先看）

本 Skill 不允许只依赖文字规范理解风格。生成或改造页面前，必须先查看仓库中的两张视觉基准图：

- `references/tutorial-handbook-template-01.jpeg`
- `references/tutorial-handbook-template-02.jpeg`

它们用于固定「下周七.教程手帐」的整体气质、材质与信息层级。

重点观察：

- 雾蓝、奶白与少量暖黄形成的空气感
- 大面积半透明玻璃 Sheet 与卡片的层次
- 手机界面作为主视觉时的比例与悬浮阴影
- 便利贴、虚线框、手绘箭头、下划线、星星等“手帐元素”的克制用量
- 文字信息明确，但装饰不会压过教程本身
- 阴影由大范围柔影 + 近距离接触影 + 边缘高光共同形成，而不是单一黑影

参考图是**视觉锚点**，不是要求逐像素临摹。新教程可根据内容重新排版，但最终第一眼必须仍然属于同一套家族。

优先级：

1. 手机端可用性与内容正确
2. 本 Skill 的强制交互规则（不横向溢出、不劫持滚动）
3. 参考图所定义的视觉气质
4. 单张参考图里的具体位置与装饰细节

---

## 1. 适用场景

当用户提供教学截图、操作流程图、手机系统页面、App 教程或软件步骤，并要求：

- “按下周七.教程手帐做”
- “做成教程手帐网页”
- “把这些截图做成网页”
- “做成之前那个雾蓝手帐风”

默认启用本 Skill。

典型内容：

- iPhone / App Store / Apple ID 教程
- AI 工具使用教程
- 软件安装与设置
- 订阅、账号、地区切换
- 手机技巧与操作步骤
- 由多张教学图片改造成网页

---

## 2. 核心目标

不是简单把截图贴进网页，而是把截图中的“信息结构”重新网页化。

优先顺序：

1. 内容清楚
2. 手机端好滑、好读
3. 轻、透、柔和、有立体感
4. 保留教程手帐的亲近感
5. 尽量使用 HTML + CSS 重构可编辑界面

最终观感应接近：

> Apple 的干净感 × 轻科技教程 × 手帐的亲近感

禁止为了“像图片”而牺牲网页可读性和手机交互。

---

## 3. 固定视觉语言

### 3.1 色彩

主色：雾蓝、奶白。

点缀：少量暖黄、珊瑚红。

建议 CSS Token：

```css
:root {
  --ink: #2f3440;
  --muted: #6f7782;
  --blue: #75b8e7;
  --blue-soft: #cfeafb;
  --blue-mist: #edf8ff;
  --cream: #fffdf4;
  --paper: #fffef8;
  --yellow: #fff0ad;
  --red: #e86a5b;
  --line: rgba(67,83,97,.18);
}
```

颜色原则：

- 不使用大面积高饱和蓝
- 暖黄只作为便利贴、提示、小面积光晕
- 红色只用于重点框、警示、小标记
- 页面整体保持低饱和和高明度

### 3.2 背景

背景不能是单一纯色。

默认由：

- 雾蓝渐变
- 奶白过渡
- 少量暖黄光晕
- 柔和 radial-gradient（径向渐变）

组成。

示例：

```css
background:
  radial-gradient(48rem 38rem at 12% 15%, rgba(112,198,245,.48), transparent 58%),
  radial-gradient(38rem 34rem at 93% 15%, rgba(255,250,227,.90), transparent 60%),
  radial-gradient(36rem 30rem at 84% 86%, rgba(255,226,166,.36), transparent 62%),
  linear-gradient(155deg,#cfeafa 0%,#eef9ff 43%,#fffdf5 100%);
```

背景的作用是“空气感”，不能抢正文。

---

## 4. 毛玻璃与立体感

本风格的立体感不是靠一个重阴影，而是 5 层共同形成：

1. 半透明背景
2. `backdrop-filter`（背景模糊）
3. 半透明白色描边
4. 多层柔和外阴影
5. 顶部高光 / 轻微内阴影

### 推荐玻璃卡片

```css
.glass-card {
  position: relative;
  background: linear-gradient(
    145deg,
    rgba(255,255,255,.62),
    rgba(255,255,255,.32)
  );
  border: 1px solid rgba(255,255,255,.72);
  border-radius: 28px;
  backdrop-filter: blur(18px) saturate(120%);
  -webkit-backdrop-filter: blur(18px) saturate(120%);
  box-shadow:
    0 22px 55px rgba(58,77,92,.14),
    0 5px 16px rgba(58,77,92,.07),
    inset 0 1px 0 rgba(255,255,255,.88),
    inset 0 -8px 18px rgba(180,200,220,.06);
}
```

### 顶部玻璃高光

```css
.glass-card::before {
  content: "";
  position: absolute;
  inset: 1px;
  border-radius: inherit;
  background: linear-gradient(
    180deg,
    rgba(255,255,255,.46) 0%,
    rgba(255,255,255,.10) 24%,
    transparent 56%
  );
  pointer-events: none;
}
```

### 阴影原则

正确：柔、散、轻、分层。

错误：黑、硬、浓、像悬浮按钮。

---

## 5. 页面结构

默认采用“连续教程页”，不要做 PPT 式强制翻页。

推荐层级：

```text
页面背景
└── 教程页 section
    └── 半透明主 Sheet
        ├── 标题区
        ├── 手机 / 软件界面主视觉
        ├── 说明玻璃卡片
        ├── 黄色便利贴
        ├── 手绘箭头 / 下划线 / 星星
        └── 步骤标记 / 页码
```

每一个步骤可以是一块独立 `section`。

内容较多时自然向下延伸，不强制一屏塞完。

---

## 6. 手机界面重构规则

用户给出的系统设置、App 页面，如果结构不复杂，优先用 HTML + CSS 重构。

例如 iPhone 设置页可以拆为：

- 手机外框
- 顶部标题
- 搜索框
- Apple 账户卡
- 设置列表
- 左侧图标
- 右侧箭头
- 红色重点框

优先重构的原因：

- 文本可改
- 内容可复制
- 红框位置可调整
- 后续可加入交互
- 清晰度不依赖截图分辨率

### 什么时候保留真实截图

以下情况可以直接嵌入截图：

- UI 极复杂且重构成本明显过高
- 教程依赖真实 App 内容
- 截图本身就是需要展示的信息证据
- 用户明确要求保留原始截图

即使使用截图，也应放入带圆角、描边、阴影的设备或玻璃容器中，不要裸贴。

---

## 7. 手机外框

手机模型必须有“厚度”，但不能像 3D 渲染器。

```css
.phone-frame {
  background: linear-gradient(180deg,#fffdf9 0%,#f6f2eb 100%);
  border: 1px solid rgba(225,220,210,.9);
  border-radius: 34px;
  box-shadow:
    0 26px 55px rgba(103,128,160,.18),
    0 10px 18px rgba(255,255,255,.70) inset,
    0 -10px 20px rgba(210,210,220,.12) inset;
}
```

要求：

- 边缘奶白而非死白
- 投影比普通卡片稍明显
- 手机本体应成为主视觉，不使用纯黑厚边框抢画面

---

## 8. 手帐元素

### 黄色便利贴

用途：

- 补充说明
- 小提示
- 路径提示
- 注意事项

特点：

- 奶黄色而不是荧光黄
- 可有 1°–3° 轻微旋转
- 阴影比玻璃更短、更像纸张浮起
- 每屏最多 1–2 个，避免满屏贴纸

### 手绘箭头 / 下划线

用途：强调视觉路线。

原则：

- 宁少勿多
- 不遮挡正文
- 颜色以蓝灰、珊瑚红为主
- 可以使用 SVG，而不是复杂图片

### 重点框

默认使用珊瑚红圆角描边。

不要用纯正红、霓虹红。

---

## 9. 移动端规则（强制）

本 Skill 以手机端为第一优先级。

### 9.1 必须防止左右晃动

```css
* { box-sizing: border-box; }

html,
body {
  margin: 0;
  padding: 0;
  width: 100%;
  max-width: 100%;
  overflow-x: hidden;
}
```

外层容器：

```css
.wrapper {
  width: 100%;
  max-width: 760px;
  margin: 0 auto;
  padding-left: 16px;
  padding-right: 16px;
}
```

禁止无理由使用：

```css
width: 100vw;
```

优先使用：

```css
width: 100%;
```

绝对定位的便签、箭头、装饰圆必须检查是否越过视口。

### 9.2 禁止抢用户滚动

不要使用：

```css
scroll-snap-type: y mandatory;
```

不要用 JavaScript 强行计算滚动位置、自动吸附到上一屏或下一屏。

默认：

```css
html { scroll-behavior: auto; }
.stage {
  min-height: 100svh;
  height: auto;
  overflow-y: visible;
  scroll-snap-type: none;
}
```

网页必须做到：

> 手指滑多少，页面走多少。

### 9.3 iPhone 安全区

```css
.page {
  padding-top: max(28px, env(safe-area-inset-top));
  padding-bottom: max(34px, env(safe-area-inset-bottom));
}
```

---

## 10. 动效规则

本风格不依赖动效成立。

如果加入动效，只允许轻量：

- 卡片进入：淡入 + 8–16px 位移
- 箭头：短距离绘制
- 高光：非常轻的透明度变化
- 点击步骤：局部展开

禁止：

- 大幅弹跳
- 频繁漂浮
- 页面强制自动滚动
- 3D 翻转
- 花哨粒子
- 会影响手机滚动的动画

`prefers-reduced-motion`（减少动态效果偏好）开启时应尽量关闭非必要动画。

---

## 11. 字体与排版

默认字体栈：

```css
font-family:
  -apple-system,
  BlinkMacSystemFont,
  "Segoe UI",
  "PingFang SC",
  "Hiragino Sans GB",
  "Microsoft YaHei",
  sans-serif;
```

标题：清楚、有呼吸感，避免超粗黑。

正文：深灰而不是纯黑。

辅助信息：低对比度蓝灰。

手写感只用于少量标记，不把正文全部换成手写字体。

---

## 12. 内容转换工作流

当用户提供 1 张或多张教程截图时：

### Step 1｜读图

识别：

- 页面标题
- 操作路径
- 手机 / App 界面
- 用户强调区域
- 注释、箭头、红框
- 多张图片之间的先后步骤

### Step 2｜重组信息

不要机械照抄画面位置。

重新整理成：

- 这一屏要做什么
- 主操作在哪里
- 辅助说明是什么
- 下一步是什么

### Step 3｜决定“重构还是截图”

简单系统 UI → HTML + CSS。

复杂真实内容 → 保留截图并做好容器。

### Step 4｜搭移动端布局

先验证 375–430px 宽度，再考虑桌面端。

### Step 5｜增加风格层

依次添加：

1. 雾蓝奶白背景
2. 玻璃卡片
3. 手机立体阴影
4. 暖黄便利贴
5. 手绘标记

### Step 6｜QA

必须实际检查滚动、横向溢出、文字换行和安全区。

---

## 13. 必做 QA Checklist

交付前逐项确认：

- [ ] 375px 手机宽度不横向溢出
- [ ] 页面不会左右晃动
- [ ] 上下滑动不跳回顶部
- [ ] 未使用强制 `scroll-snap`
- [ ] 不存在 JavaScript 滚动劫持
- [ ] iPhone 顶部 / 底部安全区有留白
- [ ] 玻璃卡片有半透明、描边、高光、柔和阴影
- [ ] 阴影不黑、不硬、不厚重
- [ ] 黄色便利贴数量克制
- [ ] 红框 / 箭头不会挡住正文
- [ ] 主标题 > 操作主体 > 说明 > 装饰
- [ ] 真实截图若使用，边缘和清晰度合格
- [ ] 桌面端打开不会被无限放大

---

## 14. 禁止事项

不要：

- 把整张教程图片直接当网页背景
- 只做一个平平的白色卡片
- 使用大量纯白 + 黑重阴影
- 使用荧光蓝、荧光黄
- 一屏塞满箭头和便利贴
- 为了“高级”把正文做得很淡
- 使用固定 430px 宽度导致小屏溢出
- 用 `100vw + padding` 制造横向滚动
- 用 `scroll-snap-type: mandatory` 做强制翻页
- 用脚本监听触摸然后替用户滚动

---

## 15. 参考实现

仓库中的视觉参考图：

- `references/tutorial-handbook-template-01.jpeg`
- `references/tutorial-handbook-template-02.jpeg`

以及可运行参考实现：

`examples/appstore-account-switch.html`

是「下周七.教程手帐」v1.1 的基准参考页。

重点参考：

- 移动端连续自然滚动
- 雾蓝奶白暖黄背景
- 毛玻璃主 Sheet
- 手机模型的多层阴影
- 页面不横向晃动
- 不使用强制滚动吸附

后续迭代可以提高精致度，但不能破坏这些基础行为。

---

## 16. 一句话标准

> 像一份可以滑动、可以编辑、带着一点手作温度的精致手机教程，而不是把教程图片搬进浏览器。
