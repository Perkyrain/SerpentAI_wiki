*注意：这个类现阶段还是有点太简单了，beta阶段一定会进行扩展。1.0发行版计划会大规模改善1.0*

_GameFrame_ 类旨在呈现一个在 Serpent.AI 框架中的游戏帧(哇！)。


## GameFrame GameAgent实例里有什么？

### 功能

* 保存游戏图像帧的图像数据
* 跟踪游戏帧的元数据
* 提供传需求调用的缓存的图像帧内变量
* 对其他 GameFrame 实例进行测量

### 概念

#### 帧变量

帧变量是对 GameFrame 实例的图像数据执行的转换，它们将作为属性被提供给用户。出于性能考量，帧变量并不是预计算的，而是在被首次请求属性值时惰性求值的。结果会被自动缓存，所以下次访问此属性时，可以即时获得结果。

### 相关信息

* `self.offset_x`: 如果 GameFrame 实例是更大的 GameFrame 的子集，则该变量是以像素为单位的 X 方向的偏移量，默认为0。
* `self.offset_y`: 如果 GameFrame 实例是更大的 GameFrame 的子集，则该变量是以像素为单位的 Y 方向的偏移量，默认为0。
* `self.resize_order`: 调整图像数据大小时使用[样条插值](https://zh.wikipedia.org/wiki/样条插值)的顺序，默认为1。

### 帧变量

* `self.frame`: 全分辨率图像数据
* `self.half_resolution_frame`: 1/4 分辨率图像数据 (宽度 / 2, 高度 / 2)
* `self.quarter_resolution_frame`: 1/8 分辨率图像数据 (宽度 / 4, 高度 / 4)
* `self.eighth_resolution_frame`: 1/16 分辨率图像数据 (宽度 / 8, 高度 / 8)
* `self.grayscale_frame`: 灰度图像, 全分辨率图像数据
* `self.ssim_frame`: 用于 SSIM 计算的 100x100 灰度图像数据

### 动作

### `self.compare_ssim`

**方法签名** *compare_ssim(self, previous_game_frame)*

返回 2 个 GameFrame 实例 *ssim_frame* 变量之间的结构相似性分数（SSIM）

* **previous_game_frame**: 另一个 GameFrame 游戏图像帧实例

## GameFrame 实例会在哪里被调用？

技术上来说，GameFrame 实例在可以在任何地方使用，但其主要用途还是作为游戏代理的帧处理程序的 *game_frame* 参数被使用。在 Serpent.AI 中，通常最好还是用 GameFrame 实例来取代原始图像数据。
