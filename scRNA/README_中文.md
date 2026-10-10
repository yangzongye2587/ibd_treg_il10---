# UC BATF TNFR2项目代码整理说明

## 这套目录是什么

这是从原始 `code` 文件夹中提取的投稿整理副本。原始脚本、数据对象和结果文件没有被移动或修改。大型 `.h5ad`、`.loom`、PDF和PNG没有重复复制；它们已记录在 `full_source_inventory.csv` 中。Jupyter notebook已删除嵌入输出和执行计数，以减小投稿代码体积，但代码单元保持原样。

## 推荐运行顺序

1. `01_data_integration`：读取GSE214695、GSE231993和GSE282122，整理metadata、整合对象并完成大类群注释。
2. `02_fig1_composition`：生成Fig. 1的大类群marker、归一化组成及样本级炎症相关分析。
3. `03_fig2_trajectory_wgcna_ml`：完成CD4轨迹、CellRank/scFates、Treg hdWGCNA、BATF相关性以及bulk机器学习。
4. `04_fig3_treg_substates`：完成Treg细分、TNF-associated Treg相关性、marker、GO和亚群模块分析。
5. `05_fig4_regulatory_chip`：完成CellOracle虚拟敲除及小鼠BATF ChIP-seq峰图。
6. `06_supporting_optional`：非主线或转换工具，确认需要后再纳入共享仓库。

## 当前没有在code文件夹中找到的关键分析代码

- Fig. 4的scATAC-seq完整处理、质控、标签转移/反卷积、TNFRSF1B开放度、chromVAR和footprint代码。
- SCENIC从表达矩阵到adjacency、regulon和AUCell结果的完整执行脚本；现有目录主要是 `.loom/.csv/.tsv` 结果。
- Fig. 6 Il10缺失DSS小鼠bulk RNA-seq的原始质控、比对/定量、差异表达、四类基因程序和GO富集代码。
- Fig. 5动物体重、结肠长度、组织学评分和免疫印迹的统计及作图代码。
- 从原始三个人UC数据集到最终Seurat对象的完整、可从空环境一键运行流程仍不连贯。

这些缺失内容必须补齐，或者在Methods中明确由何人、用何软件和参数生成。Fig. 4和Fig. 6若没有可复现脚本，不建议立即投稿。

## 已发现的主要复现风险

- 多个R脚本包含本机绝对路径和 `setwd()`，换电脑后无法直接运行。
- 多个脚本依赖交互式工作区中已经存在的Seurat对象，没有显式读取输入文件。
- `wgcna.R`、`TREG_SUB.R`、`rna_机器学习.R` 等混有探索、作图和重复执行代码，尚未形成单一确定流程。
- 部分脚本重复加载包；个别行疑似存在拼接或语法问题，运行前需逐段验证。
- 多数机器学习和轨迹代码未明显记录随机种子；结果可能不能逐数值复现。
- notebook存在多份`copy`版本，其中只有两个空的`TFANALYZE.IPYNB`哈希完全相同；其他同名副本并非完全一致，不能直接删除。
- `tcell_draw.R`含明显的NEC项目路径和分析内容，未纳入本项目投稿副本。

## 投稿前建议改造

1. 新建统一配置文件，集中设置项目根目录、输入目录、输出目录和随机种子。
2. 所有脚本从磁盘显式读取输入，不依赖RStudio全局环境。
3. 将每个脚本整理为“一种分析、一个确定入口、固定输入和固定输出”。
4. 使用患者/生物学样本作为统计单位，并保存每张图对应的source-data表。
5. 记录R、Python、包和数据库版本；R可保存`sessionInfo()`，Python可导出环境文件。
6. 机器学习的特征筛选放入交叉验证内部，严格保持GSE87473训练、GSE75214外部验证。
7. 为所有随机分析设置并记录seed。
8. 将新生成的小鼠bulk RNA-seq脚本和数据accession加入仓库。

## 两个清单文件

- `code_manifest.csv`：本次选出的投稿相关代码、用途、依赖包、硬编码路径和随机种子审计。
- `full_source_inventory.csv`：原始code文件夹内全部文件的类型、大小和SHA256，可用于确认文件完整性。
