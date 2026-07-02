---
name: cx-import
description: Format question banks for Chaoxing Learning Pass exam import. Use when users ask to generate, convert, clean, or validate Chaoxing import text/HTML/docx, or mention prompts like "超星学习通", "试卷导入", "题库格式", "正确答案", "解析".
---

# CX Import

Generate import-ready Chaoxing exam content that strictly follows required question format and answer/analysis conventions.

## Workflow

1. Read user source content and normalize all questions into plain text blocks.
2. Remove forbidden content:
   - Document title (for example, "XX试卷")
   - Question-type section headings (for example, "一、单项选择题")
3. Add question type label for every question:
   - Use Chinese brackets: 【单选题】、【多选题】、【填空题】、【判断题】、【简答题】
   - Place between question number (with 顿号) and score (with parentheses)
4. Ensure every question uses this fixed order:
   - First line: question number + 【type label】 + score + question text
   - Then options line when applicable
   - Then `正确答案:` line
   - Then `解析:` line
5. Verify numbering continuity across all question types.
6. Verify every question includes analysis.
7. Return final content only (no extra explanation mixed into deliverable unless user asks).

## Hard Rules

- Keep question number, type label, score, and question text on the same line.
- Format: `题号、【题型】（分值）题目内容` (e.g., `1、【单选题】（2.0分）题目内容`).
- Use `、` (顿号) after question number.
- Type labels must use Chinese brackets: 【单选题】、【多选题】、【填空题】、【判断题】、【简答题】.
- Use full-width score style like `（2.0分）`.
- Keep answer prefix exactly as `正确答案:`.
- Keep analysis prefix exactly as `解析:`.
- Use `____` for blanks.
- For multi-blank questions, place answers on separate lines as `空1: ...`, `空2: ...`.
- For true/false, answers must be `正确` or `错误` only.
- For short answer questions, put detailed answer content on new lines after `正确答案:`.
- Avoid course-specific references like `在XX课程中`; prefer generic context like `在Linux操作系统中`.

## Output Templates

Use templates from `references/chaoxing-format-reference.md`.

When user asks for HTML/docx workflow:
- Generate HTML that mirrors the same question/answer/analysis structure.
- If conversion command is requested, use `pandoc input.html -o output.docx`.

## Final Validation Checklist

- No title or question-type heading exists.
- Question numbering is continuous.
- Every question has a type label in Chinese brackets (【单选题】/【多选题】/【填空题】/【判断题】/【简答题】).
- Type label is placed between question number and score.
- Score format is present on each question.
- Answer line exists and uses exact prefix.
- Analysis line exists for every question.
- Question-specific constraints are satisfied (single/multiple/fill/true-false/short-answer).
