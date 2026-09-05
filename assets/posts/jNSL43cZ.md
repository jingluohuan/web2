# AI 绘图提示词教程：从“小白乱输”到“稳定出图”

现在很多人都在玩 AI 绘图，比如：

- 
OpenAI 的 DALL·E

- 
Stable Diffusion

- 
ComfyUI

- 
Midjourney

- 
Fooocus

但很多新手一开始都会遇到一个问题：

> “为什么别人一句提示词能出神图，我的却像抽象派？”

其实 AI 绘图最核心的东西，就是——提示词（Prompt）。

这篇文章会从零开始，教你真正理解 AI 绘图提示词到底怎么写。

---

# 一、什么是 AI 绘图提示词？

简单来说：

提示词 = 你给 AI 的“画面描述”。

你描述得越清晰：

- 
AI 理解越准确

- 
出图越稳定

- 
细节越丰富

例如：

❌ 错误示范：

```
一个女孩
```

AI 根本不知道：

- 
长什么样

- 
什么风格

- 
什么衣服

- 
什么场景

- 
什么光影

所以结果会随机。

---

而真正专业的提示词会像这样：

```
1girl, silver hair, blue eyes, school uniform, sitting by window, sunset light, anime style, detailed face, soft lighting
```

翻译：

- 
一个女孩

- 
银色头发

- 
蓝色眼睛

- 
校服

- 
坐在窗边

- 
黄昏光线

- 
二次元风格

- 
面部细节

- 
柔和光影

这样 AI 才知道你真正想画什么。

---

# 二、AI 提示词的核心结构

真正实用的提示词，通常由这几个部分组成：

```
主体 + 外观 + 动作 + 场景 + 光影 + 风格 + 画质
```

---

## 1. 主体（最重要）

先告诉 AI：
 “谁是主角？”

例如：

```
1girl
1boy
cat
robot
cyberpunk girl
anime girl
```

---

## 2. 外观描述

比如：

```
silver hair
long hair
blue eyes
glasses
white dress
hoodie
```

外观越详细，人物越稳定。

---

## 3. 动作

例如：

```
smiling
looking at viewer
sitting
running
holding sword
```

动作会直接影响构图。

---

## 4. 场景

例如：

```
classroom
street
rainy night
coffee shop
library
space station
```

场景决定氛围。

---

## 5. 光影

很多人忽略这一点。

但真正高级的 AI 图：
 核心其实是光。

例如：

```
cinematic lighting
soft lighting
sunset light
neon light
volumetric lighting
```

---

## 6. 风格

例如：

```
anime style
realistic
oil painting
watercolor
cyberpunk
studio ghibli style
```

不同风格差距会非常大。

---

## 7. 画质关键词

这些词很多时候是“神图关键”。

例如：

```
masterpiece
best quality
ultra detailed
8k
highly detailed
```

虽然有些模型已经不太依赖这些词，
 但很多时候仍然有效。

---

# 三、最常见的提示词写法

## 二次元风格

```
1girl, long hair, blue eyes, school uniform, cherry blossoms, anime style, soft lighting, masterpiece
```

---

## 写实风格

```
beautiful woman, realistic skin, detailed face, cinematic lighting, DSLR photo, 85mm lens
```

---

## 赛博朋克风格

```
cyberpunk city, neon lights, rainy night, futuristic girl, glowing eyes
```

---

## 风景类

```
mountain, sunset, lake reflection, ultra detailed, cinematic atmosphere
```

---

# 四、负面提示词（Negative Prompt）

这个非常重要。

它的作用是：

> 告诉 AI “不要画什么”。

例如：

```
low quality, blurry, bad hands, extra fingers, deformed face
```

作用：

- 
修复崩坏

- 
减少畸形

- 
提升稳定性

---

## 为什么 AI 总是画不好手？

因为 AI 对人体结构理解并不是真正“懂”。

尤其是：

- 
手指

- 
眼睛

- 
牙齿

- 
多人互动

特别容易崩。

所以很多人都会加：

```
bad hands
extra fingers
missing fingers
```

---

# 五、提示词不是越长越好

很多新手会这样：

```
masterpiece, best quality, ultra detailed, amazing quality, absurdres...
```

堆几十行。

实际上：

> 提示词的核心是“有效信息”。

不是越长越强。

---

## 真正高质量提示词：

重点明确。

例如：

```
1girl, white hair, blue eyes, black hoodie, rainy street, neon lighting
```

已经能很好出图了。

---

# 六、不同 AI 的提示词习惯不同

## Midjourney

更偏自然语言：

```
A girl standing in the rain, cinematic lighting, anime style
```

---

## Stable Diffusion

更偏“标签式”。

例如：

```
1girl, silver hair, blue eyes, hoodie
```

---

## Flux

理解自然语言更强。

甚至可以：

```
一个站在雨中的少女，霓虹灯倒映在积水里
```

直接出不错的图。

---

# 七、提示词万能公式

你可以直接套：

```
主体，
外观，
动作，
场景，
光影，
风格，
画质
```

例如：

```
1girl,
silver hair,
blue eyes,
sitting by window,
sunset lighting,
anime style,
masterpiece
```

---

# 八、新手最容易犯的错误

## 1. 提示词太乱

一会写写实，
 一会写二次元。

AI 会冲突。

---

## 2. 风格不统一

例如：

```
anime style + realistic skin + oil painting
```

容易抽象。

---

## 3. 关键词太少

AI 无法理解你想画什么。

---

## 4. 完全复制别人提示词

很多时候：
 模型不同，
 效果会完全不一样。

---

# 九、推荐新手练习的方法

## 方法一：模仿优秀提示词

去看：

- 
Civitai

- 
Lexica

- 
PromptHero

别人怎么写。

---

## 方法二：拆解提示词

例如：

```
1girl, school uniform, classroom, sunset
```

你可以改：

- 
school uniform → hoodie

- 
classroom → cafe

- 
sunset → rainy night

慢慢理解每个词的作用。

---

## 方法三：少量修改

不要一次改几十个词。

否则你根本不知道：
 到底哪个词影响了结果。

---

# 十、AI 绘图真正的核心

很多人以为：

> AI 绘图 = 拼提示词。

其实不是。

真正核心是：

- 
审美

- 
构图

- 
光影

- 
氛围

- 
风格理解

提示词只是“工具”。

真正决定作品质量的，
 依然是人的想法。

---

# 结尾

AI 绘图并不只是“输入一句话”。

它更像：

> 你在和 AI 一起创作。

当你真正理解提示词以后，
 你会发现：

AI 不再只是“随机生成图片”，
 而是真正能把你脑海里的画面具现化。
