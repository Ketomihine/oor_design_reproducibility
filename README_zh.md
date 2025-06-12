# 使用健康单细胞参考进行疾病状态识别
[![DOI](https://zenodo.org/badge/503372743.svg)](https://zenodo.org/badge/latestdoi/503372743)

**欢迎新贡献者！** 本项目致力于通过比较疾病样本的单细胞数据与健康参考数据集，以识别在疾病中发生改变或新出现的细胞状态。我们诚邀您参与贡献，共同改进分析方法和工具。

本代码仓库包含相关的Jupyter Notebook和Python脚本，用以复现论文中的基准测试分析（参见[手稿](https://doi.org/10.1101/2022.11.10.515939)）。这些分析的核心在于利用对照组和细胞图谱数据集作为健康参考，来识别与特定疾病相关的细胞状态。

相关的疾病状态识别及“参考外”（Out-of-Reference, OOR）检测与评估的工作流程，已封装为一个[Python软件包](https://github.com/emdann/oor_benchmark)。

## 项目目标与核心方法

在单细胞生物学研究中，准确识别疾病如何影响细胞状态是一个关键挑战。本项目旨在通过构建和利用健康的单细胞参考图谱来应对这一挑战。核心方法步骤如下：

1.  **构建参考图谱：** 整合来自多个健康个体的单细胞数据，创建一个全面的、标准化的健康细胞状态参考。
2.  **整合疾病数据：** 将来自疾病队列的单细胞数据与已构建的健康参考图谱进行计算对齐。
3.  **差异状态分析：** 通过比较疾病样本与健康参考，识别那些在疾病中丰度发生显著改变的细胞状态，或检测疾病特异性的、在健康参考中未曾出现的新细胞状态（即“参考外”细胞状态）。
4.  **基准测试与方法评估：** 我们采用模拟数据及真实的复杂疾病数据集（例如COVID-19、特发性肺纤维化IPF），系统性地评估不同参考设计策略（如：仅使用匹配的对照组 vs. 利用大规模健康细胞图谱）和计算方法在准确识别疾病相关细胞状态方面的性能。

通过这些方法，我们期望为理解复杂疾病中的细胞异质性及其动态变化提供更精确、更稳健的分析策略。

## 快速入门指南

若您是新加入本项目的贡献者，请遵循以下步骤配置您的开发环境并开始运行分析：

1.  **克隆代码仓库：**
    ```bash
    git clone https://github.com/MarioniLab/oor_design_reproducibility.git
    cd oor_design_reproducibility
    ```

2.  **设置Python环境：** 强烈建议使用`conda`或`venv`创建一个独立的Python环境。本项目主要基于Python 3.x。
    ```bash
    # 使用 conda
    conda create -n oor_env python=3.8
    conda activate oor_env

    # 或者使用 venv
    python3 -m venv oor_env
    source oor_env/bin/activate
    ```

3.  **安装依赖包：** 项目所需的依赖包通常在各个分析脚本或Jupyter Notebook中指明，或在根目录的`requirements.txt`文件中统一列出（若项目提供）。核心依赖包括`scanpy`、`scvi-tools`、`pandas`和`numpy`。具体分析目录（如`src/1_PBMC_data_preprocessing/`）可能包含更详细的依赖说明。
    ```bash
    # 示例命令（具体依赖包可能有所不同，请以实际为准）
    pip install scanpy scvi-tools pandas numpy jupyterlab
    ```

4.  **下载数据：** 分析所需的预处理数据集（通常为`.h5ad`格式文件）和预训练模型可从[Figshare](https://doi.org/10.6084/m9.figshare.21456645)下载。请根据您计划运行的特定分析任务，下载相应文件，并将其存放于代码仓库内的指定位置（通常是`data/`目录或特定分析子目录，请依据脚本中的路径设置进行调整，因为本项目结构未显式指定顶层`data`目录）。

5.  **运行分析笔记本或脚本：**
    *   分析流程通常以Jupyter Notebook (`.ipynb`)或Python脚本 (`.py`)的形式提供，位于`src/`目录下的相应子文件夹内。
    *   启动Jupyter Lab或Jupyter Notebook以交互式运行笔记本：
        ```bash
        jupyter lab
        # 或 jupyter notebook
        ```
    *   随后，在Jupyter用户界面中找到并打开相应的笔记本执行分析。对于Python脚本，可直接通过命令行运行。

    例如，若要开始PBMC数据的预处理，您可以查阅`src/1_PBMC_data_preprocessing/`目录下的笔记本或脚本。

## 代码仓库结构

- `diff2atlas` - 实用工具模块，可能包含项目共用的一些辅助函数。
- `metadata` - 存放用于分析的元数据表格（例如，研究样本信息、临床数据等）。
- `src` - 包含所有分析笔记本和脚本的核心目录。
  - `1_PBMC_data_preprocessing/` - PBMC（外周血单核细胞）数据集的预处理与多样本数据协调流程。这通常是整体分析流程的起始步骤。
  - `2_simulation_design/` - 基于模拟数据集进行“参考外”(OOR)细胞状态检测的基准测试。这有助于在可控条件下评估不同方法的性能。
  - `3_simulation_ctrl_atlas_size` - 评估OOR检测性能对于参考图谱和对照组数据集大小变化的鲁棒性。
  - `3b_crosstissue_atlas` - 研究在使用组织匹配的参考图谱与跨组织参考图谱时，OOR检测的鲁棒性。
  - `4_COVID_design` - 在COVID-19患者数据上比较不同参考设计策略（例如，如何选择和整合参考样本）的效果。
  - `5_IPF_HLCA_design` - 在IPF（特发性肺纤维化）肺部数据集上，结合HLCA（人类肺细胞图谱）进行参考设计策略的比较和评估。

## 数据说明

本分析流程中使用的经过预处理的数据集和scVI（单细胞变分推断）模型，均可通过[Figshare](https://doi.org/10.6084/m9.figshare.21456645)公开获取。关于原始数据集的详细来源信息，请参阅[研究元数据表](https://github.com/MarioniLab/oor_design_reproducibility/blob/master/metadata/suppl_table_studies.csv)。

**关键数据格式与模型解释：**

*   `.h5ad` 文件：这是一种基于HDF5的AnnData（Annotated Data）文件格式，广泛用于存储单细胞组学数据，其内部通常包含基因表达矩阵、细胞层面的元数据（如细胞类型、实验批次）以及基因层面的元数据。
*   `scVI model`：指使用`scvi-tools`库训练得到的深度生成模型。这类模型常用于单细胞数据的降维、整合、批次效应校正以及细胞类型注释等任务。

**模拟分析专用数据：**
*   `PBMC_merged.normal.subsample500cells.clean_celltypes.h5ad` - 这是一个经过整合与清洗的健康人PBMC图谱（AnnData对象），包含了来自13个不同研究的健康个体PBMC数据，并进行了细胞类型注释（每项研究抽样500个细胞）。该数据集主要用于通过模拟实验对OOR（参考外）细胞状态的识别能力进行基准测试。
*   `model_PBMC_merged.normal.subsample500cells.zip` - 这是在上述健康PBMC图谱上训练得到的scVI模型压缩包，用于后续的联合注释和数据整合。（采用scvi-tools v0.16.2训练，具体的训练参数详见[此Jupyter Notebook](https://github.com/MarioniLab/oor_design_reproducibility/blob/master/src/1_PBMC_data_preprocessing/20220601_PBMC_scVI.ipynb)）。
*   模拟分析的量化结果以 `.csv` 文件形式提供 (`OOR_simulations_*.csv`)：
    *   `*.nhood_results_all.csv` - 基于Milo分析的细胞邻域（neighborhood）级别的结果，其中包含了OOR状态细胞的比例信息。
    *   `*.TPRFPRFDR_results_all.csv` - 每次模拟实验中，关于OOR状态识别的真阳性率（TPR）、假发现率（FDR）和假阳性率（FPR）。这些指标用于评估分类模型的性能。
    *   `*.AUPRC_results_all.csv` - 每次模拟实验的精确率-召回率曲线下面积（AUPRC）。此指标常用于评估二分类模型的综合性能，尤其适用于处理类别不平衡的数据集。

**COVID-19分析专用数据：**
*   `PBMC_COVID.subsample500cells.atlas.h5ad` - 健康对照图谱数据集（AnnData对象），包含来自12个研究的健康个体PBMC数据（每项研究抽样500个细胞）。
*   `PBMC_COVID.subsample500cells.covid.h5ad`- COVID-19疾病队列数据集（AnnData对象），包含来自[Stephenson et al. 2021](https://www.nature.com/articles/s41591-021-01329-2)研究中COVID-19患者的PBMC数据（每位患者抽样500个细胞）。
*   `PBMC_COVID.subsample500cells.ctrl.h5ad` - 匹配的健康对照数据集（AnnData对象），包含来自[Stephenson et al. 2021](https://www.nature.com/articles/s41591-021-01329-2)研究中健康捐赠者的PBMC数据（每位捐赠者抽样500个细胞）。
*   后续以 `PBMC_COVID.subsample500cells.design.*.post_milo.h5ad` 命名的文件是不同参考设计（如ACR - Atlas-Control-Reference, CR - Control-Reference）策略下，经过Milo差异丰度分析处理后的AnnData对象，包含了Milo的分析结果。对应的 `*.nhood_adata.h5ad` 文件则为邻域级别的AnnData对象。
*   `model_COVID19_reference_atlas_scvi0.16.2.zip` - 专为ACR设计在健康对照图谱数据集上训练的scVI模型。（采用scvi-tools v0.16.2训练，训练参数详见[此脚本](https://github.com/MarioniLab/oor_design_reproducibility/blob/master/src/4_COVID_design/COVID_train_references.py)）。

**IPF（特发性肺纤维化）分析专用数据：**
*   类似地，以 `IPF_HLCA.*_design.post_milo.h5ad` 命名的文件是针对IPF数据集，在不同参考设计（ACR, CR, AR - Atlas-Reference）下，经过Milo分析并存储结果的AnnData对象。其中 `IPF_HLCA.ACR_design.post_milo.h5ad` 文件还包含了对异常基底样细胞状态的注释信息 (`adata.obs['basal_like_annotation']`)。对应的 `*.nhood_adata.h5ad` 文件为邻域级别的AnnData对象。

**跨组织图谱分析专用数据：**
*   `model_TabulaSapiens_scvi0.20.0.zip` - 在Tabula Sapiens（一个大型多组织人类细胞图谱）数据集上训练的scVI模型。（采用scvi-tools v0.20.0训练，训练参数详见[此脚本](https://github.com/MarioniLab/oor_design_reproducibility/blob/revision-1.0/src/3b_crosstissue_atlas/train_atlas.py)）。

## 贡献指南

我们热烈欢迎各种形式的社区贡献，包括但不限于：

*   提交BUG报告 (Bug reports)
*   提出新功能或改进建议
*   完善项目文档
*   提交代码修复或功能增强的拉取请求 (Pull Requests)

在您提交代码贡献之前，请尽量确保：
1.  您的代码遵循项目既有的编码风格和规范。
2.  若适用，请为您的更改添加相应的单元测试或验证步骤。
3.  同步更新相关的项目文档。

## 如何引用

若您在您的研究工作中使用了本项目的代码、数据或分析方法，请按以下方式引用我们的出版物：
> Dann E., Teichmann S.A. and Marioni J.C. Precise identification of cell states altered in disease with healthy single-cell references. biorXiv https://doi.org/10.1101/2022.11.10.515939

## 联系我们

如果您有任何疑问，或希望就潜在的贡献进行讨论，请通过在GitHub代码仓库中提交一个 [Issue](https://github.com/MarioniLab/oor_design_reproducibility/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc) 来联系我们。我们期待与您的交流与合作！
