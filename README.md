# CNSocialDepress (CNSD)

**CNSocialDepress** is a Chinese social media dataset for depression risk detection and structured psychological analysis.

The dataset is designed to support research on Chinese NLP, mental health-related language analysis, depression risk detection, structured psychological profiling, interpretable classification, and large language model evaluation or fine-tuning.

This repository provides the dataset file, data format description, annotation information, usage notes, ethical considerations, limitations, disclaimer, and citation instructions.

## Paper

This dataset is associated with the paper:

**CNSocialDepress: A Chinese Social Media Dataset for Depression Risk Detection and Structured Analysis**

arXiv: https://arxiv.org/abs/2510.11233
(This paper, CNSocialDepress, has been accepted by the 6th RaPID@MENTAL.ai Workshop @ LREC 2026.)

## Download

You can download the dataset from the following link: https://drive.google.com/file/d/1QCRZJAJsESg9WB6pmLqnDtbmASNhHaaN/view?usp=sharing 

## Dataset Overview

CNSocialDepress contains user-level Chinese social media data from Sina Weibo. Each user sample includes multiple Weibo posts and a structured psychological analysis.

The dataset provides:

- Binary user-level depression risk labels
- Multi-dimensional structured psychological analysis
- Evidence-based explanations using post indices
- User-level social media text collections

### Dataset Statistics

| Item | Count |
|---|---:|
| Total users | 233 |
| Positive / depression-risk users | 116 |
| Negative / non-depression-risk users | 117 |
| Total posts / texts | 44,178 |
| Depression-related annotated segments | 10,306 |

According to the paper, the dataset includes 20,360 texts from positive users and 23,818 texts from negative users.

## Data Source

The raw data are derived from the **Sina Weibo Depression Dataset (SWDD)**, a user-level Chinese social media dataset collected from Sina Weibo.

We gratefully acknowledge the authors of SWDD for making their dataset available and for providing an important foundation for research on depression detection from Chinese social media.

CNSocialDepress builds upon SWDD by adding structured psychological annotations and user-level analyses. Compared with datasets that only provide binary labels, CNSocialDepress is designed to support more interpretable and fine-grained analysis of depressive signals in Chinese social media text.

## Dataset File

The dataset is provided as a JSON file:

```text
CNSD_dataset.json
```

The file is organized as a user-level dictionary. Each key corresponds to one anonymized user ID, and each value contains the user's social media texts and structured annotation output.

## Data Structure

The top-level JSON structure is:

```json
{
  "p_user_1": {
    "input": "...",
    "output": "..."
  },
  "n_user_1": {
    "input": "...",
    "output": "..."
  }
}
```

### Field Description

| Field | Description |
|---|---|
| `p_user_x` | An anonymized positive user ID. These users are labeled as depression-risk users. |
| `n_user_x` | An anonymized negative user ID. These users are labeled as non-depression-risk users. |
| `inpput` | The full collection of Weibo texts for one user. Each post is indexed as `text_1`, `text_2`, etc. |
| `output` | The user-level binary label and structured six-dimensional psychological analysis. |
| `答案：“是”` | The user is judged to show depression-risk signals. |
| `答案：“否”` | The user is judged not to show clear depression-risk signals. |

**Note:** The current dataset field name is `inpput`. This is kept to match the released JSON file. Users should load the data using this exact key unless the field name is later standardized to `input`.

## Structure of the `output` Field

The `output` field is stored as a Chinese natural-language annotation string. It contains one binary judgment and six structured analytical dimensions.

The general structure is:

```text
答案：“是/否”。总结和分析:
负面情绪（次要判断标准）：...
抑郁心理状态（主要判断标准）：...
抑郁相关的临床症状（主要判断标准）：...
造成抑郁的潜在外因（次要判断标准）：...
抑郁相关的医疗表达（主要判断标准）：...
抑郁相关的语言表达模式（次要判断标准）：...
综合判断：...
```

The six dimensions are:

| Chinese Dimension | English Description | Role |
|---|---|---|
| 负面情绪 | Negative emotions | Secondary criterion |
| 抑郁心理状态 | Depressive psychological state | Primary criterion |
| 抑郁相关的临床症状 | Clinical symptoms related to depression | Primary criterion |
| 造成抑郁的潜在外因 | Potential external causes of depression | Secondary criterion |
| 抑郁相关的医疗表达 | Medical expressions related to depression | Primary criterion |
| 抑郁相关的语言表达模式 | Language use patterns related to depression | Secondary criterion |

### Conceptual Parsed Format

Although the released `output` field is stored as a single text string, it can be conceptually parsed into the following structure:

```json
{
  "answer": "是",
  "analysis": {
    "negative_emotions": "...",
    "depressive_psychological_state": "...",
    "clinical_symptoms": "...",
    "potential_external_causes": "...",
    "medical_expressions": "...",
    "language_use_patterns": "..."
  },
  "final_judgment": "..."
}
```

This structure makes the dataset suitable for both binary classification and structured generation tasks.

## Example

The following is a simplified and anonymized Chinese example. It is provided only to illustrate the data format and annotation style. It is not a real user sample.

```json
{
  "p_user_example": {
    "inpput": "\ntext_1: 最近总是睡不好，醒来之后还是很累。\ntext_2: 情绪一直很低落，不太想和别人说话。\ntext_3: 有时候觉得自己很没用，但还是希望以后能慢慢好起来。\ntext_4: 最近压力很大，很多事情都不知道该怎么处理。\ntext_5: 昨天又去看医生了，医生让我继续吃药。",
    "output": "答案：“是”。总结和分析: 负面情绪（次要判断标准）：用户多次表达低落、疲惫和压力较大的负面情绪。text_1[疲惫]，text_2[情绪低落]，text_4[压力很大]。抑郁心理状态（主要判断标准）：用户出现自我否定和社交退缩倾向。text_2[不想和别人说话]，text_3[觉得自己很没用]。抑郁相关的临床症状（主要判断标准）：用户提到睡眠问题和持续疲惫。text_1[睡眠不好、醒来后仍然疲惫]。造成抑郁的潜在外因（次要判断标准）：用户提到近期压力较大，可能构成心理状态恶化的外部诱因。text_4[压力很大，事情难以处理]。抑郁相关的医疗表达（主要判断标准）：用户明确提到看医生和继续服药。text_5[看医生、继续吃药]。抑郁相关的语言表达模式（次要判断标准）：整体语言呈现消极、自我否定和退缩倾向。综合判断，该用户可能存在抑郁风险。"
  },
  "n_user_example": {
    "inpput": "\ntext_1: 今天看了一部很好看的电影，心情不错。\ntext_2: 周末准备和朋友出去吃饭。\ntext_3: 最近有点累，但总体还好。\ntext_4: 明天想早点睡，然后继续准备考试。",
    "output": "答案：“否”。总结和分析: 负面情绪（次要判断标准）：用户偶尔提到疲惫，但整体表达较为日常和积极。text_3[有点累]。抑郁心理状态（主要判断标准）：未发现持续低落、自我否定、无望感或明显社交退缩等表达。抑郁相关的临床症状（主要判断标准）：未发现明显的睡眠障碍、食欲变化、持续疲劳或功能受损相关描述。造成抑郁的潜在外因（次要判断标准）：文本中未发现明确的外部压力事件或创伤性经历。抑郁相关的医疗表达（主要判断标准）：未出现就医、服药、诊断或治疗相关表达。抑郁相关的语言表达模式（次要判断标准）：整体语言以日常生活分享为主，未呈现明显消极、自我否定或绝望表达。综合判断，该用户未呈现明显抑郁风险。"
  }
}
```

## Annotation Scheme

CNSocialDepress provides structured psychological analysis at the user level. Each user is analyzed along six depression-related dimensions.

### 1. Depressive Psychological State

This dimension captures expressions related to core depressive psychological states, such as:

- Hopelessness
- Self-negation
- Guilt
- Worthlessness
- Emotional numbness
- Self-harm ideation
- Suicidal ideation
- Persistent psychological pain

This is treated as a **primary criterion**.

### 2. Medical Expressions Related to Depression

This dimension captures explicit medical or treatment-related expressions, such as:

- Diagnosis
- Medication
- Hospital visits
- Doctors
- Therapy
- Side effects
- Treatment experience
- Depression-related medical terms

This is treated as a **primary criterion**.

### 3. Clinical Symptoms Related to Depression

This dimension captures symptoms that may be associated with depression, such as:

- Sleep disturbance
- Insomnia
- Fatigue
- Appetite changes
- Weight changes
- Physical discomfort
- Nightmares
- Loss of energy
- Functional impairment

This is treated as a **primary criterion**.

### 4. Negative Emotions

This dimension captures emotional expressions such as:

- Sadness
- Anxiety
- Loneliness
- Fear
- Irritability
- Anger
- Despair
- Grief
- Emotional instability

This is treated as a **secondary criterion**.

### 5. Potential External Causes of Depression

This dimension captures possible external triggers or contextual factors, such as:

- Relationship problems
- Family conflict
- School pressure
- Work stress
- Social isolation
- Interpersonal conflict
- Major life events
- Long-term stressors

This is treated as a **secondary criterion**.

### 6. Language Use Patterns Related to Depression

This dimension captures linguistic patterns that may be associated with depressive expression, such as:

- Repeated negative wording
- Self-deprecating language
- Expressions of helplessness
- Rhetorical questions
- Negation
- Extreme or absolute phrasing
- Withdrawal-oriented expressions
- Repeated references to pain or exhaustion

This is treated as a **secondary criterion**.

## Annotation Process

The annotation process follows an expert-in-the-loop design.

The dataset includes two levels of annotation:

### 1. User-level binary risk label

Each user is assigned a binary depression risk label:

- `是`: depression-risk user
- `否`: non-depression-risk user

### 2. Six-dimensional structured analysis

For each user, the annotation output provides a structured analysis across the six depression-related dimensions. When evidence is found, the annotation cites the corresponding post index, such as `text_12` or `text_35`, together with a short explanation.

The annotation was conducted by four senior researchers with expertise in psychology and depression-related scales. Annotators were blinded to the original binary labels in SWDD during re-annotation. They periodically cross-checked overlapping samples and revised the guidelines through discussion to improve consistency and annotation quality.

## Intended Use

CNSocialDepress is intended for academic research purposes, including but not limited to:

- Chinese depression risk detection
- Mental health-related NLP
- User-level social media analysis
- Structured psychological profiling
- Interpretable depression-risk classification
- Evidence-based mental health text analysis
- Large language model evaluation
- Large language model fine-tuning for structured psychological analysis
- Summarization and explanation generation for mental health-related text

## Not Intended Use

This dataset must **not** be used for:

- Clinical diagnosis
- Medical decision-making
- Psychological counseling replacement
- Treatment recommendation
- Automated triage
- Individual surveillance
- Commercial user profiling
- Insurance, employment, education, or credit-related decision-making
- Any automated decision that affects real individuals

The labels and analyses in this dataset are research-oriented annotations based on social media text. They must not be interpreted as clinical diagnoses.

## Ethical Considerations

CNSocialDepress contains sensitive mental health-related language data. Users of this dataset must handle it responsibly and follow privacy-preserving research practices.

When using this dataset, users should follow these principles:

- Do not attempt to identify, trace, contact, or profile any individual in the dataset.
- Do not use the dataset for surveillance, discrimination, commercial targeting, or real-world individual assessment.
- Do not interpret model outputs as clinical diagnoses.
- Do not build systems that automatically decide whether a real person has depression.
- Clearly report the dataset source, task boundary, ethical risks, and model limitations in any related publication.
- Any real-world application must require additional ethical review, clinical validation, regulatory compliance, and human expert supervision.

During re-annotation, the data were further de-identified by removing or masking personal information, geographic locations, and other potentially identifiable information.

## Disclaimer

CNSocialDepress is released for research purposes only.

The dataset is not a medical device, clinical tool, diagnostic instrument, or psychological counseling system. The labels and structured analyses reflect research annotations based on social media language and should not be considered equivalent to a diagnosis by a psychiatrist, clinical psychologist, physician, or licensed mental health professional.

Any model trained or evaluated on this dataset can only provide research-level predictions or references. It must not be used as the sole basis for assessing an individual's mental health condition.

If a system built using this dataset identifies high-risk language, the appropriate response is to encourage the user to seek professional help or emergency support when necessary. No algorithmic system can replace in-person psychiatric evaluation or professional mental health care.

## Limitations

Although CNSocialDepress provides a structured resource for Chinese depression risk detection, it has several limitations.

### 1. Platform-specific bias

The data are derived from Sina Weibo. Therefore, the dataset may reflect platform-specific language styles, user demographics, cultural practices, and posting behaviors. Models trained on this dataset may not generalize well to other platforms or offline contexts.

### 2. Limited sample size

The dataset contains 233 user-level samples. Although the annotations are detailed and expert-informed, the number of users is still limited for large-scale deep learning or broad population-level generalization.

### 3. Limited linguistic coverage

Chinese social media language contains dialects, metaphors, slang, emojis, indirect expressions, and rapidly changing online phrases. The dataset may not fully cover all regional, demographic, or community-specific language variations.

### 4. Annotation subjectivity

Depression risk analysis involves complex psychological and linguistic interpretation. Even with expert annotation and cross-checking, some judgments may still be affected by context, interpretation, and subjective criteria.

### 5. Focus on depression-related signals

The current structured analysis mainly focuses on depression-related symptoms and expressions. It does not comprehensively cover other mental health conditions such as anxiety disorders, bipolar disorder, PTSD, eating disorders, or personality disorders.

### 6. Social media text is not clinical evidence

Social media posts only reflect what users express online. They do not provide complete information about a person's clinical history, diagnosis, living environment, interpersonal relationships, or real psychological state.

### 7. Risk of misuse

Because the dataset concerns mental health, it may be misused for profiling or surveillance if proper restrictions are not followed. Users must respect the intended academic research scope and avoid any individual-level decision-making use.

## Data Loading Example

```python
import json

with open("CNSD_dataset.json", "r", encoding="utf-8") as f:
    data = json.load(f)

print("Number of users:", len(data))

for user_id, item in data.items():
    user_texts = item["inpput"]
    annotation = item["output"]

    print("User ID:", user_id)
    print("Input preview:", user_texts[:200])
    print("Output preview:", annotation[:200])
    break
```

## Recommended Preprocessing

Depending on the research task, users may consider the following preprocessing steps:

1. Split `inpput` into individual posts using the `text_i:` markers.
2. Extract the binary label from `output` by detecting `答案：“是”` or `答案：“否”`.
3. Parse the six annotation dimensions from the structured `output` field.
4. Extract evidence indices such as `text_12`, `text_35`, etc.
5. Keep user-level grouping when splitting train/dev/test sets to avoid data leakage.
6. Remove or mask any remaining sensitive information before model training or redistribution.

## Possible Tasks

CNSocialDepress can be used for several research tasks:

### 1. Binary Depression Risk Detection

Input:

```text
User-level Weibo posts
```

Output:

```text
是 / 否
```

### 2. Structured Psychological Analysis Generation

Input:

```text
User-level Weibo posts
```

Output:

```text
Six-dimensional structured psychological analysis
```

### 3. Evidence-based Explanation Generation

Input:

```text
User-level Weibo posts
```

Output:

```text
Risk judgment + cited text indices + explanation
```

### 4. Multi-dimensional Mental Health-related Text Analysis

Input:

```text
User-level Weibo posts
```

Output:

```text
Negative emotions / depressive psychological state / clinical symptoms / external causes / medical expressions / language patterns
```

## Citation

If you use CNSocialDepress, please cite our paper:

```bibtex
@inproceedings{xu2026cnsocialdepress,
  title     = {{CNSocialDepress}: A Chinese Social Media Dataset for Depression Risk Detection and Structured Analysis},
  author    = {Xu, Jinyuan and Lan, Tian and Yu, Xinyi and He, Xiangyu and Zhang, Hongbo and Wang, Yuxuan and Magistry, Pierre and Valette, Mathieu and Li, Lihua},
  booktitle = {Proceedings of the 6th RaPID@MENTAL.ai Workshop on Resources and ProcessIng of linguistic, para-linguistic and extra-linguistic Data from people with various forms of cognitive/psychiatric/developmental impairments @ LREC 2026},
  year      = {2026}
}
```

Please also cite the original SWDD dataset:

```bibtex
@article{cai2023depression,
  title = {Depression Detection on Online Social Network with Multivariate Time Series Feature of User Depressive Symptoms},
  author = {Cai, Yicheng and Wang, Haizhou and Ye, Huali and Jin, Yanwen and Gao, Wei},
  journal = {Expert Systems with Applications},
  volume = {217},
  pages = {119538},
  year = {2023},
  doi = {10.1016/j.eswa.2023.119538}
}
```
