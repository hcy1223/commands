---
description: Display the completion progress of tasks from a file.
argument-hint: [path/to/task.md]
---
Review and display the progress of tasks from a Markdown task file.

**Execution Steps:**

1.  **Identify the Target File:**
    - If the user provides a file path in `$ARGUMENTS`, use that file.
    - If `$ARGUMENTS` is empty, find the most recently created Markdown file in the `tasks/` directory.
    - If no file is found, stop and inform the user that no task file was found, suggesting they run `/create-task` or specify a file path.

2.  **Analyze the File:**
    - Read the contents of the target file.
    - Count the number of completed tasks (lines with `- [x]`).
    - Count the number of incomplete tasks (lines with `- [ ]`).
    - Calculate the total number of tasks.

3.  **Generate the Progress Report:**
    - If the total number of tasks is zero, report that no tasks were found in the file.
    - Otherwise, calculate the completion percentage.
    - Display the status in the following format, including a 50-character progress bar (`■` for completed, `□` for incomplete).

**Output Format:**

```
📊 Status for: [path/to/the/task/file.md]

   [■■■■■■■■■■■■■■■■■■■■■■■■■□□□□□□□□□□□□□□□□□□□□□□□□□] 50% (10/20 tasks completed)

```
Print the file path, the visual progress bar, the percentage, and the fraction of completed tasks, exactly as shown above.
