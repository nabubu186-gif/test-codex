# 面向科研PPT和化学机理图的 skill 筛选

## 本次范围

只在线读取公开仓库说明、许可证和部分关键脚本；没有克隆候选仓库、下载技能包、安装候选技能、执行候选代码或调用候选技能的付费服务。本文的静态审阅不等于完整安全审计、依赖审计或杀毒扫描。未发现明显恶意行为的部分，不代表整个项目无风险。

此次机理圈直接用现有环境的PowerPoint矢量线和文字完成，不需要新装skill。圆环表示候选光敏循环，不伪造已确认的再生步骤；PVC氧化断链列为独立支路。

## 1. RDKit skill：优先考虑

来源：[K-Dense Scientific Agent Skills / rdkit](https://github.com/K-Dense-AI/scientific-agent-skills/tree/main/skills/rdkit)

- **用途**：根据SMILES、MOL、SDF生成和检查分子结构，控制键型、价态、立体化学，输出规范结构图。适合避免PPT中的分子结构画错。
- **对本课题的帮助**：核对2-氯戊烷、二氯甲烷、二氯乙烷及已确认EAQ异构体；辅助绘制确定结构。它不能从未知机理自动证明中间体，也不是ChemDraw式完整机理排版器。
- **网络与密钥**：说明声明安装后本地运行；审阅的三个入口脚本和共享读取器中未见联网或读取API密钥的逻辑。安装RDKit依赖本身仍需受信任的软件源。
- **文件行为**：读取指定分子文件，按参数输出CSV/SDF/SMILES等；指定输出路径仍可能写入已有文件，因此应使用独立输出目录。
- **已看代码**：`molecular_properties.py`、`similarity_search.py`、`substructure_filter.py`、`_common.py`，以及SKILL.md；未审查RDKit二进制发行包及全部依赖。
- **许可**：该技能标明BSD-3-Clause；安装时应一并核对具体目录及依赖的实际许可。
- **结论**：最符合准确化学结构的需求；你筛选后可考虑单独安装此技能，不必安装整套177个技能。

## 2. Scientific Visualization：按需考虑

来源：[K-Dense / scientific-visualization](https://github.com/K-Dense-AI/scientific-agent-skills/tree/main/skills/scientific-visualization)

- **用途**：实验图表、配色、图像元数据、导出与图表真实性检查。适合有原始数据的FTIR、动力学、GPC或产物定量图表；不是专门的化学机理圈工具。
- **安全观察**：主说明与所看入口强调本地运行、保留原始数据、输出需显式覆盖。审阅的`figure_export.py`、`image_metadata.py`、`palette_audit.py`中未见联网或读取密钥逻辑。
- **文件行为**：可写导出文件、临时文件和检查报告；部分流程允许显式覆盖。不应把未知Python脚本当作纯数据输入，动态导入及其他依赖仍需单独检查。
- **依赖**：Python与Matplotlib/Pillow/pypdf等，具体功能按需使用；Plotly静态导出可能需要Chrome/Chromium。
- **许可**：MIT。
- **结论**：适合后续实验数据制图；对当前单页机理圈不是必需。

## 3. Scientific Schematics：本次不优先推荐

来源：[K-Dense / scientific-schematics](https://github.com/K-Dense-AI/scientific-agent-skills/tree/main/skills/scientific-schematics)

- **用途**：用图像模型生成概念示意草图，再由模型给出视觉审阅意见。
- **关键限制**：输出PNG，非可逐项编辑的矢量化学机理图；模型可能画错结构、标注和箭头。自动评分不等于科学正确性。
- **数据与费用**：需要OpenRouter API Key；提示词发送给OpenRouter及底层模型服务，生成图还会用于远程审阅，可能产生费用。
- **密钥读取**：入口会从显式参数、环境变量或当前及上级目录的`.env`寻找`OPENROUTER_API_KEY`，随后通过环境传给子脚本。没有理由把未发表实验材料或无关密钥交给此流程。
- **已查入口**：`generate_schematic.py`；对`generate_schematic_ai.py`检查了请求、密钥、读写和子进程相关代码，并非逐行完整审计。
- **许可**：MIT。
- **结论**：不是认定有病毒；其外发、收费和不可编辑输出与当前任务不匹配，所以不优先。

## 4. Scientific Slides：有条件考虑

来源：[K-Dense / scientific-slides](https://github.com/K-Dense-AI/scientific-agent-skills/tree/main/skills/scientific-slides)

- **用途**：科研叙事、答辩结构、少字图多、演讲节奏及视觉检查，与你的整体汇报要求相关。
- **条件**：含OpenRouter图像生成/审阅路径，也支持原生PowerPoint。应明确只使用本地可编辑排版路径；生成式功能另行审批与审查。
- **审查程度**：本次主要看主说明，未审阅全部脚本，所以安全结论弱于上面两个本地工具。
- **许可**：MIT。
- **结论**：可列入候选，但为这次机制页无需额外安装。

## 5. Anthropic PPTX skill：暂不推荐直接安装到Codex

来源：[Anthropic / pptx](https://github.com/anthropics/skills/tree/main/skills/pptx)

- **功能**：原生PPT、图表、模板编辑和文件验证，技术上符合可编辑PPT需求。
- **文件行为**：含清理脚本，会删除解包目录中未引用的幻灯片和资源，应只作用于工作副本。主说明中也有覆盖、删除和重新打包命令。
- **许可问题**：[当前LICENSE.txt](https://github.com/anthropics/skills/blob/main/skills/pptx/LICENSE.txt)为专有许可，并写有在其服务外保留副本、复制、修改、分发等限制，不能因为仓库公开就当作通用开源安装包。需要先确认具体使用许可。
- **已查内容**：SKILL.md、LICENSE.txt、`scripts/clean.py`；未完整审计所有工具。
- **结论**：暂不作为Codex直接安装推荐，原因是许可和使用边界，不是发现病毒。

## 版本与复查

审阅期间只读查看到的仓库HEAD：

- K-Dense Scientific Agent Skills：`a1043ef544e3a100c28e70a736b6cf32554d0677`
- Anthropic Skills：`dbd4588f9e1033efb41dad4bef2f7947c8993d44`

公开main分支可能更新。你选定之后，安装前应按实际选中的版本重新核对入口、依赖、网络目标、覆盖/删除行为和许可；本次不自动授权后续下载、安装或启用付费功能。
