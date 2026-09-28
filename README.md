# 面向统计学的 Python 程序设计（64 学时标准版）

本仓库是面向应用统计学大一学生的课程发布版，包含 32 学时理论材料和 32 学时实验材料。

课程以统计数据处理与分析为主线，内容依次覆盖 Python 基础、程序控制与数据结构、NumPy、Pandas、数据可视化与探索性分析，以及两组比较和简单线性回归。

启用 GitHub Pages 后，仓库根目录的 `index.html` 提供完整课程导航。网页也可以直接在本地浏览器中打开。

## 目录

```text
lecture/      T01–T16 课堂演示、可执行课堂 Notebook 和离线 HTML
lab/          E01–E16 学生实验 Notebook、预习卡和课程数据
_extensions/  Quarto 交互式课件扩展
```

实验按“章内连续、章末综合、章节间软依赖”组织。学生可以继续使用前一实验生成的成果；缺少个人成果时，可以从 `lab/shared/` 中的官方检查点重新开始。

## 环境安装

建议安装 Miniforge 或其他提供 Conda/Mamba 的发行版，然后在仓库根目录执行：

```powershell
mamba env create -f environment.yml
mamba activate pythonBasic
```

如果没有安装 Mamba，也可以使用：

```powershell
conda env create -f environment.yml
conda activate pythonBasic
```

## 使用材料

- 浏览器课堂演示：打开 `lecture/chapter*/` 中对应的 HTML。
- 本地课堂练习：打开 `lecture/chapter*/` 中以 `_inclass.ipynb` 结尾的 Notebook。
- 实验课：打开 `lab/applied-stat-64/chapter*/` 中对应的 `_inclass.ipynb`。
- 实验前：阅读同目录下相应的 `_prelab_card.md`。

从仓库根目录启动 JupyterLab，可以让实验 Notebook 的路径判断和成果衔接保持一致：

```powershell
jupyter lab
```

## 重新渲染课件

安装 Quarto 后，可在仓库根目录运行：

```powershell
quarto render
```

仓库已经包含预渲染 HTML；不需要重新渲染即可使用课堂演示。

## 数据说明

课程数据完全模拟，仅用于教学，不包含真实学生信息。字段定义、加工阶段和官方检查点见 `lab/shared/README.md` 与 `lab/shared/data_dictionary.md`。

## 发布来源

本仓库由课程私有源仓库通过白名单脚本生成。`RELEASE-INVENTORY.json` 记录导出时的源提交和全部发布文件哈希。
