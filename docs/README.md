# Flix-Rime

`flix-rime` 是基于 `雾凇拼音(rime-ice)` 的 Rime 用户目录配置，英文翻译器、部件拆字反查、Emoji、简繁转换、候选置顶等功能都是雾凇拼音中配置好的，个人针对 Windows 平台 —— `小狼毫(weasel)` 进行个性化配置和优化（其他平台后续会考虑），个人使用所以会长期更新维护。

**效果展示：**

<details>
<summary><b>MacOS 主题配色</b></summary>

- MacOS Light

![MacOS Light](preview/macos_light.png)

- MacOS Dark

![MacOS Dark](preview/macos_dark.png)
</details>

<details>
<summary><b>Windows 11 主题配色</b></summary>

- Windows11 Light

![Windows11 Light](preview/windows11_light.png)

- Windows11 Dark

![Windows11 Dark](preview/windows11_dark.png)
</details>


## 雾凇拼音(rime-ice)
雾凇拼音是由 `@iDvel` 大佬配置的一套开箱即用，并且适配各个平台的方案。详细配置可以去 [雾凇拼音(rime-ice)](https://github.com/iDvel/rime-ice) 或者 [Rime 配置：雾凇拼音](https://dvel.me/posts/rime-ice/) 查看，这里就不多做赘述。

## 安装 Rime
官网 👉 [RIME | 中州韻輸入法引擎](https://rime.im/download/) 提供各个平台的下载以安装教程。

## 如何自定义
### 配置说明
> 基本不需要修改源文件，只需要在 `xxx.custom.yaml` 文件中进行修改/打补丁即可。

- 默认配置：`default.yaml` 主要是基础方案设置还有按键绑定，需要修改的话在 `default.custom.yaml` 文件中修改。
- 样式配置：`weasel.yaml` 用于定义全局样式与预设配色，需要做个性化配置则在`weasel.custom.yaml` 文件中修改/打补丁即可。
- 方案配置：`rime_ice.schema.yaml` 是雾凇拼音的主要配置，他各个功能都是在这里配置，例如：翻译器、Emoji等的行为，可以通过 `rime_ice.custom.yaml` 文件来修改这些功能，甚至可以自己写个脚本来自定义插件。

### 配置主题
本项目将主题配置文件统一存放至 `/themes/*` 目录下，方便统一管理。

**引入主题**
- 在 `weasel.custom.yaml` 中使用 `__include: themes/[theme_name].yaml:/` 引入相应主题配置，并将样式中的 `color_scheme` 和 `color_scheme_dark` 修改为主题对应的配色名称即可。
- 不喜欢当前主题配置可以直接在 `weasel.custom.yaml` 覆盖样式。

> 每个主题对应的配色名称都在 `/themes/[theme_name].yaml` 文件中一一对应

```yaml
# weasel.custom.yaml
patch:
  # 引入主题配色文件
  __include: themes/MacOS.yaml:/
  # 颜色风格方案
  "style/color_scheme": macos_light
  # 深色模式下颜色风格方案
  "style/color_scheme_dark": macos_dark

  # 在下方自定义样式
  "style/inline_preedit": false # 修改为在输入框编码，默认行内编码
  # ...
  # ⚠️ 注意：这里最好是用 + 的形式插入配置，这样如果有重复的配置项，下面的会覆盖上面的，否则会
  # 将整个配色配置全部覆盖，导致配色不生效，除非你就是想要重新写个配色（重写推荐新建文件写，而不是在原来的配置上修改）。
  "preset_color_schemes/macos_light/+":
    text_color: 0xFF0000 # 将 macos_light 配色中的文本颜色改成红色
  # ...
```

### 更新词库
不定期更新雾凇拼音的词库，或者可以到雾凇拼音仓库自行下载词库更新。
为方便管理，以及后续扩展词库，本项目将词库全部存放至 `/dicts/*` 目录下，雾凇拼音的词库为 `ice_cn/*` 和 `ice_en/*` 目录，将下载的词库文件替换即可。

### 扩展词库
在 `/dicts/` 目录下创建存放词库的文件夹，例如：`/dicts/baishuang_cn/` 这个就是用来存放白霜词库的文件夹，后续只需要在 `rime-ice.dict.yaml` 文件中修改：
```yaml
# rime-ice.dict.yaml
name: rime_ice
version: "2024-11-27"
import_tables:
  - dicts/ice_cn/8105     # 字表
  - dicts/ice_cn/41448    # 大字表（按需启用）（启用时和 8105 同时启用并放在 8105 下面）
  - dicts/ice_cn/base     # 基础词库
  - dicts/ice_cn/ext      # 扩展词库
  - dicts/ice_cn/tencent  # 腾讯词向量（大词库，部署时间较长）
  - dicts/ice_cn/others   # 一些杂项

  # 上面为雾凇拼音内置的词库，将扩展词库放在下方
  - dicts/baishuagn_cn/base
  # ...
```
> TODO：扩展词库，后续慢慢弄吧...

### 语法大模型
本项目使用的是 `万象语法模型` 长词优化，更高更精准。前往 GitHub 仓库 👉 [万象语法模型](https://github.com/amzxyz/RIME-LMDG) 下载语法模型文件。

> [!NOTE]
>
> 我按照万象介绍的使用方法来配置，没能成功加载语法模型
> 
> 最终按照 `薄荷输入法` 中的配置方法才成功的
>
> 可以去 👉 [oh-my-rime 配置教程](https://www.mintimate.cc/zh/) 看看，也是很不错的一套方案

```yaml
# rime_ice.custom.yaml
patch:
  "grammar/language": wanxiang-lts-zh-hans
  "grammar/collocation_max_length": 8         #命中的最长词组
  "grammar/collocation_min_length": 2         #命中的最短词组，搭配词频健全的词库时候应当最小值设为3避开2字高频词
  "grammar/collocation_penalty": -10          #默认-12 对常见搭配词组施加的惩罚值。较高的负值会降低这些搭配被选中的概率，防止过于频繁地出现某些固定搭配。
  "grammar/non_collocation_penalty": -17      #默认-12 对非搭配词组施加的惩罚值。较高的负值会降低非搭配词组被选中的概率，避免不合逻辑或不常见的词组组合。
  "grammar/weak_collocation_penalty": -24     #默认-24 对弱搭配词组施加的惩罚值。保持默认值通常是为了有效过滤掉不太常见但仍然合理的词组组合。
  "grammar/rear_penalty": -18                 #默认-18 对词组中后续词语的位置施加的惩罚值。较高的负值会降低某些词语在句子后部出现的概率，防止句子结构不自然。
  # translator 内加载
  translator/contextual_suggestions: true
  translator/max_homophones: 5
  translator/max_homographs: 5
```
❗ 一定要配置在当前在使用的方案的配置文件中，不然是不会生效的。

---

## 参考/致谢
1. [Rime-RimeWithSchemata](https://github.com/rime/home/wiki/RimeWithSchemata)
2. [Rime-Weasel](https://github.com/rime/weasel/wiki)
3. [雾凇拼音 | 长期维护的简体词库](https://github.com/iDvel/rime-ice)
4. [万象词库 | Rime输入法语法模型全流程构建教程，全局带声调词库，最全声调标注工具链](https://github.com/amzxyz/RIME-LMDG)
5. [oh-my-rime配置教程 | 跨平台的开源输入法Rime定制指南，打造强大的个性化输入法](https://www.mintimate.cc/zh/guide/)
