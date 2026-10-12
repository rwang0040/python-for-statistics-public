# 共享实验数据

本目录保存标准版与辅修版共同使用的模拟教学数据。数据主题为“大学生 Python 学习与课程适应调查”，所有人物和记录均由固定随机种子生成，不对应真实学生。

## 设计原则

- 同一批 160 名模拟学生贯穿文本处理、NumPy、Pandas、可视化和基础统计分析。
- 文件格式随教学阶段变化，但 `student_id` 和变量含义保持稳定。
- E02–E04 由课程准备代码从小型 JSON 文件载入同源样本；学生只需运行该单元，不把文件读取作为学习内容。
- E05 在完成字符串与文件教学后首次读取完整原始文本；E09–E10 再用 Pandas 读取同一文件。
- 原始数据包含受控的空白、类别别名、无效数值、重复记录和缺失记录，用于清洗与核验练习。
- 学生读取的原始数据、检查点和分析视图只使用 ASCII 字符；中文含义保留在数据字典和教学说明中，避免把编码排障引入初学阶段。
- 官方检查点由同一生成器产生，学生可以使用自己的前序成果，也可以从检查点继续。
- 第五章分析视图支持分布、频数、分组比较、散点图、折线图和相关热力图。
- 第六章分析视图支持两组 Welch t 检验和简单 OLS；关系被设计为清楚但不完美，不能据此作因果解释。

## 目录结构

| 路径 | 用途 |
| --- | --- |
| `raw/student_survey_raw.txt` | E05 首次读取、E09–E10 使用 Pandas 再次读取的原始调查文本 |
| `raw/learning_records_raw.csv` | 四轮学习记录 |
| `raw/project_scores_raw.csv` | 不完整的项目成绩记录 |
| `raw/course_outcomes_raw.csv` | 基础成绩、期末成绩和数据素养成绩 |
| `samples/chapter2_records.json` | E02–E04 由课程准备代码载入的 8 条同源规范记录；固定覆盖三档学习时长、两种支持方案和三类专业 |
| `checkpoints/e05_cleaned_records.json` | 文本清洗后的记录列表，供 NumPy 阶段读取 |
| `checkpoints/e10_students_clean.csv` | 清洗后的学生调查主表 |
| `checkpoints/e11_learning_records_clean.csv` | E11 清洗后的四轮学习记录，供 E12 继续使用 |
| `checkpoints/e12_*` | E12 使用的项目成绩、课程结果和综合分析表 |
| `analysis/student_eda.csv` | E13–E14 使用的学生横截面分析视图 |
| `analysis/learning_wave.csv` | E14 使用的四轮变化分析视图 |
| `analysis/student_analysis.csv` | E15–E16 使用的统计分析视图 |
| `analysis/e15_group_comparison.csv` | E16 缺少个人 E15 成果时使用的一行官方比较结果 |
| `validation_summary.json` | 行数、受控问题和第五、六章关键分析结果 |

各文件字段见 `data_dictionary.md`。运行 `generate_data.py` 会使用固定种子重新生成全部数据文件，并在关键教学统计量超出预设范围时失败。

## 数据衔接

| 实验 | 输入 | 产出与去向 |
| --- | --- | --- |
| E02–E04：基础处理 | 由课程准备代码载入 `chapter2_records.json` | 依次练习列表与字典、程序控制、函数与异常处理；处理思路在 E05 中复用 |
| E05：文本清洗 | `student_survey_raw.txt`，首次读取完整文本 | `e05_cleaned_records.json` → E06–E07 |
| E06–E07：NumPy 分析 | 优先使用学生 E05 输出，缺少时使用官方检查点 | 数组化、筛选、缺失处理与二维汇总；不作为后续 Pandas 实验的文件输入 |
| E09–E10：Pandas 审计与清洗 | 再次读取 `student_survey_raw.txt` | E09 形成数据审计，E10 产出 `e10_students_clean.csv` → E12 |
| E11：纵向记录整理 | `learning_records_raw.csv` | 清洗、分组汇总、频数、交叉表与宽长表转换；产出 `e11_learning_records_clean.csv` → E12、E14 |
| E12：多表汇合 | E10 学生主表、E11 学习记录，以及 `project_scores_raw.csv`、`course_outcomes_raw.csv` | 合并、核验与导出 `e12_analysis_ready.csv`，形成第五、六章的分析入口 |
| E13–E14：探索性分析 | E12 学生分析表；E14 另读 E11 轮次记录（均提供官方分析视图兜底） | 分布、分组比较、变量关系与轮次变化，为第六章的分析与解释作准备 |
| E15：两组比较 | 学生分析表；缺少个人成果时使用官方分析视图 | Welch t 检验；产出 `e15_group_comparison.csv` → E16 |
| E16：回归与最终报告 | 学生分析表，以及 E15 比较结果（均提供官方兜底） | 简单 OLS、拟合与残差图，汇合两组比较结果形成最终报告 |

**E08 是独立的随机模拟支线**：固定种子 → 随机试验 → 事件判定 → Monte Carlo 比例估计。不修改共享数据，也不产生下游实验依赖。

原始文件、官方检查点和学生输出应分开保存。后续实验材料不得要求学生覆盖本目录中的任何文件。
