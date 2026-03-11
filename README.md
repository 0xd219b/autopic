# Gemini Image Toolkit（Gemini 图片工具箱）

一个纯前端的 Web 工具集，用于 **Gemini AI 生成图片的水印去除**、**图片网格切分编辑** 和 **九宫格切图**。

> 建议仓库名：`gemini-image-toolkit`

主要用途：为 [Confirmo](https://confirmo.love/) 自定义形象生成所需的图片素材进行处理。感谢 [yetone](https://github.com/yetone/confirmo-releases) 提供如此优秀的插件！

## 页面

| 页面 | 文件 | 说明 |
|------|------|------|
| 图片调整工具 | `index.html` | 去水印 + 自定义网格切分 + 块编辑 |
| 九宫格切图 | `grid9.html` | 1:1 正方形裁切 + 3×3 九宫格切分导出 |

## 功能

### 水印去除
- 基于 [gemini-watermark-remover](https://github.com/GargantuaX/gemini-watermark-remover) 算法的反向 Alpha 混合去水印
- **自适应位置搜索** — 粗搜 + 精细搜索，自动定位水印区域，不依赖固定位置
- **多尺寸模板匹配** — 支持 48/64/72/80/96 等多种水印尺寸
- **空间 + 梯度双通道检测** — NCC 空间相关性 + Sobel 梯度相关性综合评分
- **Alpha Gain 自动校准** — 自动尝试多种增益值（0.8-2.4），选择最优去除效果
- 支持手动调节强度和重新去水印
- 导入图片时默认自动去除水印

**水印原理与资源同步**
Gemini 水印可近似为白色 Logo 的 Alpha 叠加，满足 `watermarked = α * logo + (1 - α) * original`。去除过程先用背景捕获图计算 alpha map，再通过 NCC + Sobel 在右下区域匹配水印位置/尺寸，最后按反向公式解出原图像素，并配合可选的增益校准、模板对齐与子像素精修提升效果。

资源注意事项：
- `index.html` 中的 `BG_48_SRC` / `BG_96_SRC` 必须与 `gemini-watermark-remover` 的 `src/assets/bg_48.png` / `src/assets/bg_96.png` 内容一致，否则 alpha map 偏差会导致检测失败或去不干净。
- 如需更新水印模板，建议直接从源库图片重新生成 base64 并替换，避免手工编辑出错。

### 网格切分
- 自定义目标尺寸（默认 1024×896）和块大小（默认 128px）
- 自动将图片缩放并切分为网格块
- 点击任意块可预览放大效果

### 块编辑
- **橡皮擦** — 擦除为透明（抠图）
- **画笔** — 用选定颜色绘制
- **移动** — 拖拽平移块内内容
- 可调画笔大小和颜色
- 支持撤销、重置、清空操作

### 块内容变换
- 缩放（10%-200%）和平移调整
- **批量应用** — 一键将当前变换参数应用到整行 / 整列 / 全部块

<img width="1920" height="993" alt="image" src="https://github.com/user-attachments/assets/9e793440-8d0a-4075-9f2e-b4140c9e9f38" />


### 画布缩放
- 缩放滑块（5%-100%）
- 自适应 / 1:1 快捷按钮
- Ctrl/Cmd + 滚轮缩放

### 导出
- 将所有编辑后的块合成为完整图片导出
- 自动保留背景色
- 输出 PNG 格式

### 九宫格切图（grid9.html）
- 上传任意比例图片，自动裁切为 1:1 正方形
- 拖拽调整裁切区域位置
- 可选输出尺寸（1080 / 1024 / 自动）
- 实时预览 3×3 九宫格效果
- 点击单块下载，或全部导出 / 打包下载
- 支持 File System Access API 选择保存目录

## 使用方法

1. 直接在浏览器中打开 `index.html`
2. 上传图片（支持 PNG / JPG / WebP，可点击或拖拽）
3. 图片导入时自动去除 Gemini 水印
4. 设置目标尺寸和块大小，点击"应用切分"
5. 点击块进行预览和编辑
6. 使用变换工具调整块内容，可批量应用到整行/整列/全部
7. 点击"导出图片"下载结果

## 技术栈

- 纯 HTML + CSS + JavaScript，无依赖
- HTML5 Canvas API
- 单文件部署，零构建
- UI：[Space Grotesk](https://fonts.google.com/specimen/Space+Grotesk) + [Manrope](https://fonts.google.com/specimen/Manrope) 字体，[Remix Icon](https://remixicon.com/) 图标
- 浅色主题，CSS 变量驱动设计系统，glassmorphism 风格

## License

MIT
