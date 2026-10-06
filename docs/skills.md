# 本仓库的学术书籍编辑技能

配置日期：2026-10-04。技能存放在本仓库 `.agents/skills/`，未安装到 `~/.agents/skills`、`~/.codex/skills` 或其他项目，也未添加全局链接。

## 选定技能

| 仓库内名称 | 来源与市场 | 用途 | 许可证及免费核心 |
| --- | --- | --- | --- |
| `hbe-scientific-writing` | [K-Dense 市场条目](https://skills.sh/k-dense-ai/scientific-agent-skills/scientific-writing) | 章节起草、科学表述、证据与正文对应 | MIT；正文指导无依赖，可选本地检查脚本需要 Python 3.11+ 标准库 |
| `hbe-citation-management` | [K-Dense 市场条目](https://skills.sh/k-dense-ai/scientific-agent-skills/citation-management) | DOI、书籍和书章、BibTeX、引文一致性 | MIT；可手工核验公开来源；可选脚本使用免费 `requests`，本地格式化不需要第三方包 |
| `hbe-scientific-critical-thinking` | [K-Dense 市场条目](https://skills.sh/k-dense-ai/scientific-agent-skills/scientific-critical-thinking) | 主张与证据、因果与相关、反例和替代解释 | MIT；基本审查离线，无付费 API |
| `hbe-academic-tone-guide` | [Wentor AI 市场条目](https://skills.sh/wentorai/research-plugins/academic-tone-guide) | 学术语体、谨慎措辞、作者声音 | MIT；纯 Markdown，无脚本或账号要求 |
| `hbe-academic-book-editing` | 本仓库补充 | 全书主线、章节职责、跨章术语与分层编辑 | 本地说明型技能，无外部服务依赖 |

免费指这些外部技能的许可与所选核心流程；没有启用付费 API、订阅、额外模型服务或账号集成。Python 包也未在此次配置中安装。引用检索可使用当前宿主已有的搜索功能或公开出版社、图书馆、Crossref 记录。

## 固定来源与版权

三个 K-Dense 技能来自 [K-Dense-AI/scientific-agent-skills](https://github.com/K-Dense-AI/scientific-agent-skills)，固定提交 `154988403bb5a18e9d3c0ce4e6d5e2e4b184a298`，原目录分别为：

- `skills/scientific-writing`
- `skills/citation-management`
- `skills/scientific-critical-thinking`

每个本地技能目录都保留该提交的 [MIT 许可原文](https://github.com/K-Dense-AI/scientific-agent-skills/blob/154988403bb5a18e9d3c0ce4e6d5e2e4b184a298/LICENSE.md)，版权为 2025 K-Dense Inc.；`references/`、`scripts/` 和 `assets/` 随所选目录保留。旧仓库名的市场入口可能重定向到此新名。

语体技能来自 [wentorai/research-plugins](https://github.com/wentorai/research-plugins)，固定提交 `bf44b3cd617fa94c8a1b254c5d1987142ca3d631`，原目录为 `skills/writing/polish/academic-tone-guide`。技能目录内保留 [MIT 许可原文](https://github.com/wentorai/research-plugins/blob/bf44b3cd617fa94c8a1b254c5d1987142ca3d631/LICENSE)，版权为 2026 Wentor AI。

这里只保留外部技能自身的许可证，不据此为书稿或整个仓库指定许可证。上游工具论文的引用建议不属于 MIT 的强制许可条款；如作者需要方法或工具致谢，再核实并决定如何记录。

## 本地适配

- 四个外部技能名称加 `hbe-` 前缀，描述和正文加入仓库作用域检查，避免与同名全局技能混淆。
- `scientific-writing` 入口按学术专著改写，保留证据溯源与科学表述原则；论文审计表、主张哈希和作者声明流程只作为按需的出版或实证审计工具。
- `scientific-critical-thinking` 去除依赖 OpenRouter 的图示段落；理论与人文章节按概念与论证审查，不套临床证据评分。
- `citation-management` 去除额外图示和模板技能依赖，把引用资料中的专用搜索 CLI 换成公开来源查询；Zotero 仅接受作者提供的本地导出，不连接账号或回写库。
- `academic-tone-guide` 补充中文书籍语体，修正情态动词固定强弱序、相关性因果化、已发表文献无需谨慎措辞等粗略建议。示例不能作为添加新事实的依据。
- 原有脚本未改变算法。Google Scholar 脚本及代理模式不启用；如后续确需脚本，先检查代码，解析输入输出的真实路径，并在仓库本地环境中运行。

## 隔离与加载

[OpenAI 官方文档](https://learn.chatgpt.com/docs/build-skills) 规定，Codex 从工作目录到仓库根目录扫描 `.agents/skills`。因此本配置采用仓库内实体文件，不放到父目录或用户级技能目录；`AGENTS.md` 与每个技能的入口进一步要求目标文件位于本仓库中。

这些是项目发现范围和调用约束，不是操作系统级安全沙箱。尤其上游 Python 脚本接受路径参数；不能把它们当成可以任意执行而自然限制读写范围的程序。普通写作与审稿只需说明型技能，不需要运行这些脚本。

在 VS Code 中以 `/Users/rongzhi.nie/Human-Bots-Ecology` 打开项目，并在此项目里新建 Codex 会话。新技能可在后续回合发现；若列表未刷新，重启 Codex 或重载窗口。仓库搬迁时同步更新技能入口中的本机路径。

## 使用示例

- `$hbe-academic-book-editing 检查现有目录的论证顺序、章节职责和重复内容。`
- `$hbe-academic-tone-guide 按本仓库规范润色这一节，保留作者立场和论断强度。`
- `$hbe-citation-management 核实这些书籍与书章的版本、页码和参考文献条目。`
- `$hbe-scientific-critical-thinking 检查这一章的因果推理、证据缺口和替代解释。`
- `$hbe-scientific-writing 根据已核实的材料起草这一节，缺证据处明确标记。`

没有安装以规避 AI 检测为目标的 humanizer，也没有采用含模拟研究材料的论文演示流程。全书编辑与中文规范是本地补充，不能称为市场原生的学术专著技能。
