## Basic task management

To create a new task, run:

```
[~] task add This is a test task.
Created task 100.
```

To mark that same task as done, run:

```
[~] task done 100
Completed task 100 'test'.
Completed 1 task.
You have more urgent tasks.
```

If you created a task by mistake, you can also delete the task altogether:

```
[~] task del 100
Delete task 100 'test'? (yes/no) yes
Deleting task 100 'test'.
Deleted 1 task.
```

If you really need to remove a task from the data files altogether, you can also purge the deleted task:

```
[~] task purge 0fc062bd
Permanently remove task 0fc062bd 'test'? (yes/no) yes
Purged 1 task.
```

You may want to add a task that has been completed straight away. For example, you may have completed a task before you had the chance to record it in taskwarrior, but still want to add it there for record keeping. You can use `task log` to do that.

```
[~] task log test
[~] task all test

ID St UUID     Age  Done Tags Description
 - C  dd05bd43 4s   4s        test

1 task
```

In the event you run a command you did not mean to and need to correct your mistake, you can undo your last command.

```
[~] task add test
Created task 100.
[~] task undo

The last modification was made 2023-06-05

             Prior Values  Current Values
description                test
[redacted for brevity]

The undo command is not reversible.  Are you sure you want to revert to the previous state? (yes/no) yes
Task removed.
```

## Viewing tasks

After creating one or more tasks, you may want to look at them later. Just maybe. Taskwarrior has *very* many ways for you to do that. You can view those using `task reports`, but I'll go over the most useful ones.

**Describe description filtering**

The simplest way to view a task is to run `task list`, which will output most relevant information on tasks. `task long` is similar, but will output exact dates rather than an approximation. If you want a briefer view of your tasks, use `task minimal`. This will only output the task ID **link to id section**, description, and number of annotations **link to annotation section**. `task ls` is one step up from that, also outputting tags **put link to tags section here**.

Just running the `task` command on a task will result in different behavior depending on the context. If your filter matches more than one task, it will behave the same as `task list`. If your filter matches exactly one task, it will output all the info associated with a task. This will include its UUID and full edit history.

If you need to look for a task that has already been completed, use `task all`. This will output all tasks, including completed ones, along with their **UUIDs**.

### Graphs
task burndown.daily/monthly/yearly
task summary

### Reports
task reports of all sorts
task calendar

### Statistics
task count
task stats
task timesheet

## Organizing tasks

There are two ways to organize tasks: by project, or by tag.

+ and -
proj:
task tags
task projects

## Due dates

### Calculator

### Scheduling tasks
task wait:
task scheduled:

### Recurring tasks

Taskwarrior has a pretty advanced system for interpreting the dates you give it, so you can use `task calc` to figure out specifically how `task` will interpret a given date.

## Modifying tasks

task mod
task append
task prepend
task edit

## Annotations

## Dependencies

dep:
task blocked/blocking

## Task IDs

You will notice that each task has an ID.

```
[~] task add This is a test task.
Created task 100.
```

`This is a test task.` is now a task with the ID `100`. You can now use that ID to refer to that task.

```
[~] task long 100

ID  Created    Mod  Description
100 2023-06-05 3min test

1 task
```

However, the ID is only for your convenience. It is not the primary means for `task` to index tasks, and is assigned only to open/incomplete tasks. Once the task is marked as complete, its ID is freed up for use by another task.

In order to refer to completed tasks, you may use the task's UUID, which you can find using `task all`.

```
[~] task all 100

ID  St UUID     Age  Description
100 P  0fc062bd 6min test

1 task
```

## Starting/stopping tasks

## Hooks

## Contexts
