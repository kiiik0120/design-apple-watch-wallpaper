# design-apple-watch-wallpaper

一个用于制作 **Apple Watch 最新款 Series 照片表盘壁纸** 的 Codex Skill。

> 最后更新：2026-08-11

## 作用

该 Skill 会根据 watchOS 时间显示位置调整图片构图，避开时间、日期、复杂功能和圆角裁切区域，尽量保证人物、面部、手部、Logo 等主体细节不与表盘界面重叠。

主要能力：

- 仅适配最新款 Apple Watch Series **42mm（374×446 px）** 和 **46mm（416×496 px）**
- 支持左侧时间、右侧时间和顶部时间三种布局
- 每次根据用户选择只生成一张干净壁纸
- 可自动分析并协调主体与背景颜色
- 支持 `preserve`、`harmonize` 和 `contrast` 三种颜色处理方式
- 可针对浅色或深色时钟优化时间区域的可读性
- 保留原图人物身份、画风、线稿、纹理和关键颜色

## 版本选择

- **Codex Skill：** 使用仓库根目录的 `SKILL.md`，适合 Codex，包含完整判断、编辑和验证流程。
- **通用模型兼容版：** 使用 `prompts/universal-model-prompt.md`，可复制到豆包、通义、DeepSeek 等支持图片输入或图片编辑的模型。该版本采用更短、更明确的步骤，减少模型遗漏约束的概率。

不同模型的图片编辑和精确尺寸导出能力不同。通用提示词可以统一工作流程，但不能保证所有模型都能直接输出精确像素文件。

## 不支持

不适用于 Apple Watch Ultra、Apple Watch SE、旧款 40/41/44/45mm Series、其他品牌手表或自定义 `.watchface` 表盘包。

## Codex 使用示例

安装后，在 Codex 中提供原图，并指定尺寸及时间布局，例如：

> 使用 $design-apple-watch-wallpaper 制作 46mm 壁纸，左侧时间、主体靠右，使用浅色时钟。

Skill 会优先参考用户提供的 Watch App 表盘预览；若未指定布局或时钟颜色，会先请求用户选择，再生成最终单张壁纸。

## 通用模型使用方法

打开 [`prompts/universal-model-prompt.md`](prompts/universal-model-prompt.md)，复制其中的完整提示词，填写手表尺寸、时间位置、时钟颜色和色彩模式，然后随原图一起发送给模型。
