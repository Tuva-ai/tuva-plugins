---
name: write-report
description: Write a short report on a Tuva project's recent work in which every number traces back to a run, artifact or file, and save it through Tuva's save gate so it shows on the project's Reports page. Use when the researcher asks for a weekly report, a progress or status update, or a write-up of what the project tried, found and dropped this week. Only on request; never write a report unasked.
---

# Write a report on a Tuva project

Tuva's memory knows what a project ran, saved, tried and dropped. This skill
turns that memory into a short report where every number cites the run,
artifact or file it came from. Tuva checks every number before it saves the
report, and the saved report shows on the project's Reports page.

Each document type is a profile: its headings, word limit and rubric. The
weekly project report (`weekly_one_page`, the last 7 days, at most
800 words) is the only type today. `get_tuva_report_brief` gives the
current profile, so follow the brief rather than this page if they differ.

Write a report only when the researcher asks for one.

## Rules

- **Name the project on every call.** Pass `workspace_root`, the absolute
  path of the project folder (your working directory). Under Claude Desktop
  it may be omitted when a default project is configured.
- **Every number comes from the ledger.** Never take one from memory, the
  conversation, a file or notebook you read yourself, or a summary
  (narratives, digests, `research-memory.md`). If the ledger lacks a number,
  gather the evidence that holds it, or leave the number out.
- **Cite once per claim, in its paragraph**, as `[@E3]` or `[@E3; @E7]`, at
  the end of the sentence that finishes the claim. Every number must be in an
  entry its paragraph cites. Dates, years, small counts ("3 seeds") and
  section numbers need no citation.
- **Write as the researcher.** The report is their update to their PI and
  lab: "we", about our own work. Never describe the project from outside,
  the state of the workspace (decks, docs, code), or what you could not
  find. Leave out what nothing backs and tell the researcher in chat.
- **Tell how we got there.** Open from where last week's report left off.
  Give each piece of work a paragraph: why we tried it, what we did, what
  we found. One or two numbers per paragraph, the ones that carry the result.
- **No project-only labels.** The PI knows the field, not the files: use
  field terms as they are, but never file, variable or parameter names,
  numbered tanks or days used as names, or the project's shorthand.
- **One ledger per report.** The first `gather_tuva_evidence` call returns a
  `ledger_id`; pass it back on every later call.
- **Ask for evidence by where it is, never by what you hope it says.** A
  request names a file, a run, a history entry or a topic, never a value;
  `gather_tuva_evidence` does not gather a topic that names a number.
- **Save only with `save_tuva_report`.** Never write `report.md`, or anything
  under `tuva-artifacts/reports/`, yourself: a report written there by hand
  skips the checks and does not show on the Reports page.
- **Only the brief's headings.** No methods section, abstract or appendix.

## Steps

### 1. Read the brief

Call `get_tuva_report_brief`. It returns:

- the headings in order, with what each one answers;
- the word limit;
- the main question;
- the rubric;
- the project's tried-and-dropped work.

The brief holds no evidence. Write nothing yet: the ledger comes first,
and every point of the report is planned from it.

### 2. Start the ledger

Call `gather_tuva_evidence` with no `ledger_id`. It first reads the project's
recent Claude Code and Codex sessions, then starts a ledger for the window
(the last 7 days unless the researcher asked for other dates; pass `since`
and `until` then). Each page lists items with ids such as `[@E3]`:

- history entries, with their question, outcome and checked claims;
- runs;
- saved artifacts;
- the files those touched, re-read now;
- conversation turns.

Page through the rest with `ledger_id` and the `offset` from the "More:"
line until there is none.

Items marked "a summary" or "CHANGED SINCE THE RUN" are context only. Never
cite their numbers.

### 3. Outline

Settle the main question first. The brief gives it in one of three forms:

- **The researcher's own:** copy it word for word under `## Main question`.
  If it names a number, cite the ledger entry that holds the number; if none
  does, leave the number out and tell the researcher the checks cannot trace
  it.
- **Inferred for an earlier report:** keep it under `## Main question`,
  unless the material shows the project is after something else.
- **Not known yet:** infer one sentence from the history entries and synced
  documents the brief lists, checked against the ledger. It says what the
  whole project is trying to find out, not this week's task, and has no
  numbers in it. If you cannot infer one, write `(not yet known)` under the
  heading; Tuva does not keep that as the question.

If the researcher tells you the question in the conversation, use their
words under `## Main question`. Tuva takes a question as the researcher's
own only from the saved `report.md`: to make it stick for later reports,
they edit it there (or on the Reports page).

Then, for each heading, write down:

- a few points, with no numbers;
- the source ids the points draw on.

Where a heading has no evidence, call `gather_tuva_evidence` again with the
`ledger_id` and one of `file`, `run`, `episode` or `topic`. New items join the
same ledger.

Read what comes back against the plan. If the new evidence contradicts a
point, change the point. Never leave out the evidence.

Put work dropped in the report's window under the dropped heading. The brief
lists all of the project's dropped work, whenever it ended: never present any
of it as a live line of work.

### 4. Draft section by section

Write one section at a time, from that section's own entries only.

- **Read the source before you cite it.** Call `read_tuva_ledger` with
  `source_ids` to see the full text of the entries you will cite.
- **Copy each number as the source gives it.** You may round it or give it as
  a percent (0.873 as 87.3%), but keep the source's unit and never add
  precision it does not have.
- **A worked-out number passes** when both numbers it comes from are cited in
  the same sentence, such as the gain in "Accuracy rose from 84.1% [@E3] to
  87.3% [@E5], a 3.2-point gain."
- **Stay inside the word limit.** Share it across the sections. Citations,
  code and markdown do not count.
- **Tell one story.** Decide the week's story first (what we set out to
  learn, what we learned, what it changes) and write every section to it.
  - Lead with what matters most for the main question, and say what each
    result means before the next.
  - Two or three numbers per sentence at most.
  - Short paragraphs, one point each; bullets only for parallel items.
  - No preamble and no closing summary.
  - State the limits the evidence shows, such as one seed or an unfinished run.
  - Write symbols and equations in TeX between dollar signs: `$ED_{50}$`.

Lay the report out as the brief does:

```markdown
# Weekly project report, 2026-11-06 to 2026-11-13

## Main question

Does a lower learning rate improve accuracy on the held-out split?

## What we tried and found

The learning-rate sweep reached 87.3% accuracy [@E3], against 84.1% for the baseline [@E3].
```

### 5. Check

Call `check_tuva_report` with the `ledger_id` and the whole markdown. It runs
the same checks as the save. Fix each failure, then check again:

| Code | What to do |
|---|---|
| `uncited_number` | Cite the entry the number appears in, or take the number out. |
| `value_not_in_source` | Read the cited entry with `read_tuva_ledger` and correct the number, or cite the entry that does hold it. |
| `value_not_in_statement` | The sentence must state the cited value, rounded or as given, never more precisely. |
| `unknown_source`, `foreign_source` | Cite only ids from this report's ledger. |
| `summary_only_source` | Find the run, artifact or file behind the summary, gather it, and cite that. |
| `source_changed_since_run` | The file changed after the run that produced the number, so the number may no longer hold. Leave it out, and say the result needs checking against the file as it is now. |
| `unparsable_value` | Write the number plainly, such as 0.873, 87.3% or 12 ms. |
| `over_length` | Cut, starting with the longest section. |
| `ai_phrase` | Rewrite the sentence in plain words. |

Stop after three rounds of fixes, or as soon as a fix brings back a failure
you already had. Then tell the researcher which numbers you could not trace.
Do not save, and do not loop.

### 6. Review against the rubric

Before saving, read the draft against the brief's rubric and its
tried-and-dropped list. Flag any dropped work the draft presents as live,
naming the history entry id the brief lists for it. This review is advice,
not a check. Fix real problems, then check again.

If you can start a subagent, ask it for a fresh review instead. Give it only
three things: the draft, the ledger (`read_tuva_ledger`) and the brief's
rubric.

### 7. Save

Call `save_tuva_report` with the `ledger_id`, the markdown and a short title.
It runs every check again and refuses a report that fails any. Nothing is
saved then: fix the failures it lists and save again.

A report that passes is saved in the project's `tuva-artifacts/reports/`.
The ledger it was checked against and its check results sit beside it.

Tell the researcher:

- where the report is;
- its `report_id` and version;
- how many numbers traced;
- that it is on the Reports page;
- that they can edit the report on the Reports page or in `report.md`, and
  that the next report uses their main question once they edit it;
- anything the report leaves out because no evidence backs it.

To revise a saved report, call `save_tuva_report` with its `report_id` and say
what changed in `change`. That saves its next version.

## When Tuva refuses

A refusal reads `Refused (<code>): ...`.

| Code | Meaning |
|---|---|
| `project.not_registered` | The folder is not a Tuva project. `find_tuva_project` lists the projects; or the researcher adds it in Tuva. |
| `writing.ledger_not_found` | That ledger belongs to another project, or the id is wrong. Start a new ledger. |
| `writing.source_outside_project` | The file, run or history entry is not this project's. |
| `writing.bad_window` | The start is after the end, or the range is over 92 days. |
| `writing.report_failed_checks` | The report failed a check. Fix each listed failure. |
| `writing.report_not_found` | That `report_id` is not one of this project's reports. |
| `writing.report_not_saved` | The report is empty, far too long, or names an unknown document type. |

The researcher can also have Tuva write the weekly report itself:
**Write weekly report** on the desktop Reports page, or `tuva reports write`
in the project folder.
