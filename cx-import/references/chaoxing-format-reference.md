# Chaoxing Import Format Reference

## General Rules

- Do not add document title.
- Do not add question-type section headings.
- Every question MUST have a type label using Chinese brackets: 【单选题】、【多选题】、【填空题】、【判断题】、【简答题】.
- Type label is placed between question number (with 顿号) and score (with parentheses).
- Format: `题号、【题型】（分值）题目内容`
- Keep question numbers continuous across all types.
- Every question must include `正确答案:` and `解析:`.

## Single Choice Template

```text
1、【单选题】（2.0分）题目内容?
A. 选项A    B. 选项B    C. 选项C    D. 选项D
正确答案: B
解析: 解析内容。
```

## Multiple Choice Template

```text
21、【多选题】（3.0分）题目内容?
A. 选项A    B. 选项B    C. 选项C    D. 选项D
正确答案: ABD
解析: 解析内容。
```

Notes:
- Multi-choice answer is multi-letter combination, for example `ABCD`, `ABD`.

## Fill-in-the-Blank Template

Single blank:

```text
31、【填空题】（1.0分）题目内容____。
正确答案: 内容
解析: 解析内容。
```

Multiple blanks:

```text
32、【填空题】（2.0分）题目____、____、____。
正确答案:
空1: 内容1
空2: 内容2
空3: 内容3
解析: 解析内容。
```

Notes:
- Blank token must be four underscores: `____`.

## True/False Template

```text
41、【判断题】（1.0分）陈述句题目内容。
正确答案: 正确
解析: 解析内容。
```

Notes:
- Allowed answers: `正确` or `错误` only.

## Short Answer Template

```text
51、【简答题】（10.0分）简答题题目内容。
正确答案:
第一点...
第二点...
解析: 解析内容。
```

Notes:
- Put answer body on new lines after `正确答案:`.

## Score Suggestions

- Single choice: 2 points per question.
- Multiple choice: 3 points per question.
- Fill blank: 1 point per blank.
- True/false: 1 point per question.
- Short answer: 10 points per question.

## HTML to DOCX

Use HTML first, then convert with pandoc:

```bash
pandoc input.html -o output.docx
```

Keep HTML content structure aligned to the plain-text templates above.
