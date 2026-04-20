# academic-writing-skills 原始输出摘录

## quick-audit 输出摘要

```text
[audit] File: quick_audit_fixture.tex | Format: .tex | Language: en | Mode: quick-audit
Found 28 issues (2 critical). Overall score: 2.1/6.0 (Reject).

Submission Blockers:
- Conclusion overreaches the reported evidence.
- Undefined reference: \ref{fig:missing} — no matching label found
```

关键高信号问题：

- Performance claim lacks an explicit baseline or comparator.
- Performance claim is not tied to a concrete metric or numeric result.
- Conclusion lacks limitations or future work discussion.

---

## deep-review 输出摘要

```text
Deep review found 1 major, 4 moderate, 1 minor issues.
Committee Score: 4.5/10
Editor Verdict: Pass to Review
Reviewer Recommendation: Major Revision
Issue Bundle: 1 major / 4 moderate / 1 minor
```

生成的主要文件：

- `review_report.md`
- `peer_review_report.md`
- `final_issues.json`
- `revision_roadmap.md`

Revision Roadmap 摘录：

```markdown
## Priority 1
- [ ] Abstract and conclusion claims need explicit evidence traceability

## Priority 2
- [ ] Headline claim needs tighter evidence calibration
- [ ] Novelty claim should be grounded against the closest prior work
- [ ] Comparison protocol should make fairness assumptions explicit
- [ ] Method assumptions should be justified explicitly
```

---

## latex-paper-en 多模块输出摘要

```text
% GRAMMAR: No rule-based issues detected in selected scope.
% LONG SENTENCE: No sentences exceeded configured thresholds.
% LOGIC/METHODOLOGY: No rule-based coherence issues detected.
% EXPERIMENT (Line 11) [Severity: Major] [Priority: P1]: Comparison claim names only generic baselines; cite or name the exact comparator.
% EXPERIMENT (Line 10) [Severity: Minor] [Priority: P2]: No ablation or component-level evidence is mentioned.
% EXPERIMENT (Line 10) [Severity: Minor] [Priority: P2]: No statistical significance, variance, or confidence information is mentioned.
% EXPERIMENT (Line 10) [Severity: Minor] [Priority: P2]: No efficiency comparison is mentioned.
```

---

## typst-paper 尝试输出

```text
[ERROR] File not found: academic-writing-skills/typst-paper/examples/sample.typ
```

结论：

- `typst-paper` 的能力定义存在。
- 但当前仓库没有可直接运行的 `.typ` 示例输入，因此无法仅凭自带样例完成演示。
