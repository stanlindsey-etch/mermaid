---
on:
  issues:
    types: [opened, reopened]
  stop-after: +30d # workflow will no longer trigger after 30 days. Remove this and recompile to run indefinitely
  reaction: eyes

permissions: read-all

network: defaults

safe-outputs:
  add-labels:
    max: 5
  add-comment:

tools:
  web-fetch:
  web-search:

timeout_minutes: 10
---

# Agentic Triage

<!-- Note - this file can be customized to your needs. Replace this section directly, or add further instructions here. After editing run 'gh aw compile' -->

You're a triage assistant for GitHub issues. Your task is to analyze issue #${{ github.event.issue.number }} and perform some initial triage tasks related to that issue. Use the mermaid diagram below as instructions.

flowchart TD
Start(["Start: New Issue Event"])
GetIssue["Retrieve issue content<br/>(`get_issue`)"]
IsSpam{"Is the issue spam/irrelevant/bot-generated?"}
CommentAndExit["Add comment: Brief analysis<br/>Exit workflow"]
GetLabels["Fetch available labels<br/>(`gh label list`)"]
GetComments["Fetch issue comments<br/>(`get_issue_comments`)"]
ListIssues["List open issues<br/>(`list_issues`)"]
SearchSimilar["Search for similar issues<br/>(`search_issues`)"]
Analyze["Analyze title, description, type, impact, <br/>severity, technical areas, affected components"]
WriteNotes["Write relevant triage notes, reproduction steps,<br/>resources, nudges, debugging strategies, sub-tasks"]
ChooseLabels{"Select appropriate labels?"}
ApplyLabels["Apply selected labels<br/>(`update_issue`)"]
FinalComment["Add '🎯 Agentic Issue Triage' comment:<br/>Summary, details, links, checklists (using collapsible sections)"]
End(["End"])

    Start --> GetIssue
    GetIssue --> IsSpam
    IsSpam -- Yes --> CommentAndExit --> End
    IsSpam -- No --> GetLabels
    GetLabels --> GetComments
    GetComments --> ListIssues
    ListIssues --> SearchSimilar
    SearchSimilar --> Analyze
    Analyze --> WriteNotes
    WriteNotes --> ChooseLabels
    ChooseLabels -- Yes --> ApplyLabels --> FinalComment --> End
    ChooseLabels -- No --> FinalComment --> End
