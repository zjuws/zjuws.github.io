# 王术 · 渲染技术实验室

网站：https://zjuws.github.io/

## 修改内容

内容使用 Markdown，布局和样式独立维护。

| 内容 | 修改文件 |
|---|---|
| 首页介绍 | `_includes/content/home.md` |
| 项目与研究 | `_includes/content/projects.md` |
| 技术笔记列表 | `_includes/content/notes.md` |
| 关于我 | `_includes/content/about.md` |
| 完整简历 | `_includes/content/resume.md` |
| 插帧图文笔记全文 | `notes/frame-interpolation.md` |

在 GitHub 打开对应文件，点击铅笔修改并提交到 master。GitHub Pages 会自动重新生成网页；通常几分钟后刷新网站即可看到。可在 Actions 查看发布进度。

Markdown 中的 `#` 是标题，普通文字是段落，`**文字**` 是加粗，`[文字](地址)` 是链接。代码放在三反引号中。

为保留原页面样式，部分内容文件包含 `<div>` 等布局标签、`{: .类名 }` 样式标记及 SVG 图示，编辑文字时保留这些标记。无需直接修改生成后的 HTML。

`index.html` 与 `_layouts/technical-note.html` 管布局；`style.css` 和 `app.js` 管样式与交互。旧 render-lab 地址保留跳转。
