---
name: save-results
description: "Only in a Tuva project, one that find_tuva_project lists. When a step ends with a finished result worth keeping, save it with save_tuva_artifact so it goes into the project's decks, such as a figure, a table, a headline number, a derivation, a protocol or an algorithm. Not for scripts, data or notebooks. Never for the parts of a report: a report is saved whole with save_tuva_report, through the write-report skill."
---

Only in a Tuva project, one that find_tuva_project lists. When a step ends with a finished result worth keeping, save it with save_tuva_artifact so it goes into the project's decks, such as a figure, a table, a headline number, a derivation, a protocol or an algorithm. Not for scripts, data or notebooks. Never for the parts of a report: a report is saved whole with save_tuva_report, through the write-report skill.

Write the file first: a figure as PDF (PNG for a raster image), a table as a LaTeX tabular in a .tex file with its CSV as data_path, a headline number or a derivation as a .tex fragment. Then call save_tuva_artifact (Tuva's MCP server) with its path, kind (one of the kinds the tool lists), title, takeaway, question and steps, and workspace_root set to the project folder's absolute path (find_tuva_project lists it if you are unsure). Mention the save in one short line.
