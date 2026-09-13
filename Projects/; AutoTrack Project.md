---
tags: project
alias:
  - AutoTrack Project
status: Waiting
subtitle: For demo.
---
%%
Goal:: [[2022-07-25 - AutoTracking test|AutoTracking test]]
Bar:: `$= dv.view('total-progress-bar', {file: "; AutoTrack Project"})`
%%

`$= dv.view('total-progress-bar', {file: "; AutoTrack Project", alignLeft: true})`
# AutoTrack Project

## Project Info

## Thoughts 

## Resources

## Review questions

## Tasks
- [x] 1
- [ ] 2
- [ ] 3


```dataview
list
WHERE any(map(file.tasks, (task) => !task.completed))
```