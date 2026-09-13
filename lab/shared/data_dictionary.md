# 共享实验数据字典

## 数据范围

- 观测对象：160 名学习 Python 的模拟大学生。
- 学生标识：`student_id`，格式为 `S001` 至 `S160`。
- 重复观测：每名学生原则上有 4 轮学习记录。
- 数据性质：完全模拟，仅用于教学。
- 随机种子：`202609`。
- 字符约定：学生读取的数据文件只包含 ASCII 字符并统一写为 UTF-8；中文变量含义仅用于说明，不作为文件中的分类取值。

## 学生调查主表

原始文件为 `raw/student_survey_raw.txt`，官方清洗结果为 `checkpoints/e10_students_clean.csv`。

E02–E04 不直接读取完整原始文件。课程准备代码从 `samples/chapter2_records.json` 载入同一批模拟学生中的 8 条规范记录；该读取步骤不作为学生学习内容。样本固定覆盖三档学习时长、两种支持方案、三类专业和关注条件，三个实验依次练习数据组织、程序控制、函数和异常处理。E05 在学完字符串与文件读取后，首次正式读取完整原始文本并生成 JSON；E09–E10 再使用 Pandas 处理同一原始文件。

| 字段 | 类型 | 含义 | 清洗说明 |
| --- | --- | --- | --- |
| `student_id` | 字符串 | 模拟学生唯一标识 | 去除首尾空白；按此字段删除重复记录 |
| `alias` | 字符串 | 按 `gender` 从英文常用名池中固定种子抽取的模拟昵称，如 `David` 或 `Alice` | 去除首尾空白；全体不重复，但不是记录主键 |
| `gender` | 分类 | `female`、`male` | 将 `F/M` 等别名统一 |
| `major` | 分类 | `data_science`、`applied_statistics`、`statistical_computing` | 将空格分隔的别名统一为下划线代码 |
| `prior_programming` | 两组分类 | 入课前是否有编程经历：`yes`、`no` | 将 `Y/N` 等别名统一 |
| `support_plan` | 两组分类 | `regular`（常规练习）、`feedback`（即时反馈） | 将 `standard/instant_feedback` 等别名统一 |
| `reported_study_hours` | 数值 | 学生自报每周学习时长，单位为小时 | `NA`、`unknown` 转为缺失值 |
| `reported_attendance_rate` | 数值 | 学生自报出勤率，单位为百分比 | 去除百分号；`-` 转为缺失值 |

### 研究设计说明

`support_plan` 来自既有教学安排，并非随机分配。学生分析表中每名学生只出现一次，不涉及同一学生的重复测量；第六章在分析中将不同学生视为相互独立的观测。随机分配和独立性属于研究设计信息，不能只根据数据表中的数值判断。因此，两组比较可以描述组间差异和统计证据，但不能单独证明支持方案造成了成绩差异。

## 四轮学习记录

原始文件为 `raw/learning_records_raw.csv`，官方清洗结果为 `checkpoints/e11_learning_records_clean.csv`。

| 字段 | 类型 | 含义 |
| --- | --- | --- |
| `student_id` | 字符串 | 模拟学生唯一标识 |
| `wave` | 整数 | 学习记录轮次，取值 1–4 |
| `study_hours` | 数值 | 本轮每周平均学习时长，单位为小时 |
| `attendance_rate` | 数值 | 本轮出勤率，单位为百分比 |
| `quiz_score` | 数值 | 本轮阶段测验成绩，范围 0–100 |
| `task_completion_rate` | 数值 | 本轮任务完成率，单位为百分比 |

学生与轮次共同构成记录键，即 `student_id + wave`。原始文件含少量重复键、无效值和缺失值，清洗后每名学生保留 4 轮记录。

## 项目成绩表

原始文件为 `raw/project_scores_raw.csv`，官方清洗结果为 `checkpoints/e12_project_scores_clean.csv`。

| 字段 | 类型 | 含义 |
| --- | --- | --- |
| `student_id` | 字符串 | 模拟学生唯一标识 |
| `wave` | 整数 | 项目所属轮次 |
| `project_name` | 分类 | `notebook_basics`、`text_cleaning`、`random_simulation`、`pandas_integration` |
| `project_score` | 数值 | 项目成绩，范围 0–100 |

项目表故意缺少部分 `student_id + wave` 组合，且部分已提交项目的成绩为空。合并后应区分“没有匹配的项目记录”和“存在项目记录但成绩缺失”。

## 课程结果表

文件为 `raw/course_outcomes_raw.csv`，官方检查点为 `checkpoints/e12_course_outcomes_clean.csv`。

| 字段 | 类型 | 含义 |
| --- | --- | --- |
| `student_id` | 字符串 | 模拟学生唯一标识 |
| `baseline_score` | 数值 | 入课基础测验成绩 |
| `final_score` | 数值 | 期末 Python 成绩 |
| `data_literacy_score` | 数值 | 课程末数据素养任务成绩 |

## 综合分析表

`checkpoints/e12_analysis_ready.csv` 是一人一行的分析就绪表，由学生主表、四轮学习记录汇总、项目成绩汇总和课程结果表合并得到。

| 字段 | 类型 | 含义 |
| --- | --- | --- |
| `student_id` | 字符串 | 模拟学生唯一标识 |
| `alias` | 字符串 | 英文模拟昵称 |
| `gender` | 分类 | `female`、`male` |
| `major` | 分类 | 三个英文专业代码 |
| `prior_programming` | 两组分类 | `yes`、`no` |
| `support_plan` | 两组分类 | `regular`、`feedback` |
| `study_hours` | 数值 | 四轮平均每周学习时长 |
| `attendance_rate` | 数值 | 四轮平均出勤率 |
| `task_completion_rate` | 数值 | 四轮平均任务完成率 |
| `first_quiz_score` | 数值 | 第一轮阶段测验成绩；原记录缺失时继续保留缺失 |
| `last_quiz_score` | 数值 | 第四轮阶段测验成绩；原记录缺失时继续保留缺失 |
| `submitted_projects` | 整数 | 有有效成绩的项目数 |
| `mean_project_score` | 数值 | 已评分项目的平均成绩 |
| `baseline_score` | 数值 | 入课基础测验成绩 |
| `final_score` | 数值 | 期末 Python 成绩 |
| `data_literacy_score` | 数值 | 数据素养任务成绩 |

## 第五、六章分析视图

分析视图只选择并重命名课程后半段需要的字段，不产生新的学生或新的随机结果。

| 文件 | 主要字段 | 教学用途 |
| --- | --- | --- |
| `analysis/student_eda.csv` | `gender`、`major`、`study_hours`、`attendance_rate`、`python_score`、`data_literacy_score` | 分布、频数、分组比较、散点图和相关热力图 |
| `analysis/learning_wave.csv` | `student_id`、`major`、`wave`、`study_hours`、`python_score` | 分组汇总和轮次变化折线图 |
| `analysis/student_analysis.csv` | `support_plan`、`baseline_score`、`study_hours`、`attendance_rate`、`final_score` | Welch t 检验、简单 OLS、残差和解释边界 |
| `analysis/e15_group_comparison.csv` | 两组名称、样本量、均值、均值差、95% 置信区间、t 统计量和 p 值 | E16 在缺少个人 E15 成果时使用的官方结果检查点；差异方向固定为 `feedback - regular` |

其中 `student_eda.csv` 中的 `python_score` 来自综合表的 `final_score`；`learning_wave.csv` 中的 `python_score` 来自各轮 `quiz_score`。两者名称相同但观测层级不同，使用时必须结合文件和一行所代表的观测单位解释。
