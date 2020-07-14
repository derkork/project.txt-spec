# The Project.txt format specification
Revision: 2.0.0 - 2020-07-14

## What is it?
Project.txt is an open-source lightweight human-writable and human-consumable format to write up a series of connected tasks that form a project. It is inspired by todo.txt which is a similar format for tracking personal todo items. 

The format is plain text with minimal markup. It allows to connect decentralized teams on a project plan that everyone can see and edit with a simple text editor. Synchronization can be done by any means, be it a git repository, a cloud storage share, some shared folder on a network drive or whatever is available in your setup.

This format is intended as a toolkit for setting up your projects. You may not need all the features this offers and this is totally fine. Use what you need and ignore the rest.

## How does it look like?
Project.txt is a very simple text based format. All you need is a text editor. A basic example could look like this:

```project.txt
# -----------------------------------------
# Who is working on the project
# -----------------------------------------

^/ Sandy ^#sandy
  Wants to paint a door.
^/ Anna  :#anna
  Sandy's daughter who will help her.

# -----------------------------------------
# The tasks 
# -----------------------------------------

# Anna is responsible for buying stuff and she already completed her tasks.
^[x] Buy paint 
    ^>#anna ^@buying-phase
^[x] Buy brushes 
    ^>#anna ^@buying-phase

# Sandy is responsible for painting.  Painting the door is currently in progress. 
# It depended on the materials being bought:
^[>] Paint the door 
    ^>#sandy ^<@buying-phase ^#door  

# Sandy and Anna will clean up together. They can however only do it after the door
# is painted. They estimate it will take them an hour to do so.
^[ ] Clean up 
    ^>#sandy ^>#anna ^<#door ^~1h
```

Everything except commands is treated as plain text. All commands are issued by writing a caret (`^`) followed by the command. You can already see a few commands, e.g. for creating tasks, assigning people to them, making estimations, etc. The following sections will describe all the commands.

## How do I express... ?

### Tasks
A project has tasks that need to be done. You can create a task by writing `^[ ]`:

```project.txt
^[ ] Gather requirements
^[ ] Write code
^[ ] Test implementation
```

You can add additional information to tasks, like a link to your issue tracker, notes about progress, meeting minutes, etc. This way your project plan can become living documentation of the progress and the history of the project. Everything you write behind a task start marker (`^[ ]`) becomes part of the task description. 

```project.txt
^[ ] Implement phase shield feature
     https://tracker.example.com/issues/STARSHIP-2931
     Needs to be done quickly because client wants to test
     the feature soon.
```

#### Task states
A task can have several states, indicated by a character between the `[ ]`.

```project.txt
^[ ] This task is open. It still needs to be done.

^[>] This task is in progress. 
     Somebody is currently working at it but it's not yet 
     finished. The > mimics a play button sign, you can use
     this analogy to remember this.

^[!] This task is on-hold.
     There is something blocking this task, e.g. you need
     to wait for an external thing to happen. The exclamation mark
     indicates some problem.

^[x] This task is done. It is crossed out with an x.

^[M] This task is a milestone.
     It will automatically be considered as reached when all
     tasks coming before it are done. M stands for "milestone".
```

### People
A project has people working on it. You can define people with the `^/` command. Think of someone waving, to remember this. You can add additional information to people, like what their responsibilities in the project are, contact data, etc. Remember, the format is free-form so you can add as much or as little information as you need.

```project.txt
^/ John Smith 
   This is John. He does our web development.
^/ Jane Muller
   This is Jane. She is our project manager.
^/ Andrea Viticetti
    This is Andrea. She is our tester.
```


### Comments
You can start a comment at the beginning of a line `#`. The whole line will be considered as a comment. Comments help you to structure the file to your needs. They also allow you to quickly comment out a task which you are not sure about. 
```project.txt
# ---------------------------------------------
# This is some section marker
# ---------------------------------------------

# [ ] This is a task that I commented out.
```


### Commands
You can add additional information to people, e.g. what their email address is, to which team they belong etc. It is also possible to add additional information to tasks like when a task is due, an estimation of how long a task will probably take, task dependencies and a few more. All of this is again done with commands.

#### Generally available commands
Some commands are available for both tasks and people. 

##### Tags
A tag allows you to put a mark on a task or a person which you can later use to filter out people or tasks. You write a tag like this: `^@my-tag`.
```project.txt
# This marks the 'Gather requirements' task as a high priority task.
^[ ] Gather requirements ^@high-priority
``` 

There is no set pre-defined tags, so you can use any tag name that fits your use case and your organization. E.g. if you have multiple people you may want to group them in teams. You could use tags for this:

```project.txt
^/ Jeff ^@epsilon-team
^/ Anna ^@gamma-team
^/ Angela ^@gamma-team
```

Our you could mark tasks that are your minimum viable product:

```project.txt
^[ ] Implement rocket boosters ^@mvp
```

You can have as many tags as you want on each person or task:

```project.txt
# Two tags marking Jeff as a member of epsilon team and a project manager.
^/ Jeff ^@epsilon-team ^@project-managers

# Two tags marking the rocket boosters as both a task belonging to the 
# minimum viable product and a high-priority task.
^[ ] Rocket boosters ^@mvp ^@high-priority
```

##### Labels
A label is very similar to a tag. While a tag is a single name, a label is a pair of a name and a value. There is some functional overlap between tags and labels, as they both are intended to mark entries for later filtering. The same rules as for tags apply - you can have as many as you would like per entry and there are no predefined labels. Choose whatever works best for you. To add a label you write `^@my-label:my-value`.

```project.txt
# This is how you could express that Jeff belongs to team epsilon.

^/ Jeff: ^@team:epsilon

# Using labels to mark the rocket boosters as an MVP task with a high priority.
^[ ] Implement rocket boosters ^@phase:mvp ^@priority:high
```

##### IDs
IDs allow you to cross reference entries with each other. They are used for example if you would like to assign tasks to people or make tasks dependent on each other. Again there is some functional overlap with labels and tags. An ID is supposed to be assigned to a single thing only while the same set of labels and tags can appear on many persons or tasks. To add an ID you write: `^#my-id`.

```project.txt
# This will assign the ID `#jeff` to Jeff
^/ Jeff Smith ^#jeff

# This will assign the ID `#TK1272` to the task
^[ ] Implement rocket boosters ^#TK1272
```

For IDs you can use any word or character sequence as long as it is unique. 

#### Commands for people
The following commands are only applicable to people. They have no meaning on tasks.

##### Email address
For people you almost always want to note down their email address. This information is also useful when processing the project file with tooling to (e.g. for rendering Gravatars or sending notifications to people.). You can define an email address for a person with `^=>email@example.com`. You can remember this by thinking of the stylised arrow as "how can I contact this person".

```project.txt
+ Jeff Smith :=>jeff.smith@example.com 
  This is Jeff. He does things.
```

#### Commands for tasks
Controlling tasks can become quite involved therefore this format has quite a few commands available for this. Note that you don't necessarily need to use all of them. Use what you need on your project, just because a command is available, it doesn't mean it makes sense to use it in all projects.

##### Due dates
Tasks may have due dates. In many cases it makes sense to put a due date on a milestone to see if you can reach a milestone in time - but you can add a due date to normal tasks as well. You can write a due date like this `^!2020-10-10`. The exclamation mark can serve as a reminder that this is a due date. The date must be given in `year-month-day` order and four digits must be used for the year.

```project.txt
# This task is due on October 21st 2020.
^[ ] Buy rocket parts ^!2020-10-21
```

##### Do dates
You may want to plan ahead which task should be performed at which day. For this you can use do dates. You can write a do date very similar to a due date, but instead of an exclamation mark you use a plus sign. Think of the plus sign as "adding this task to the work log of the day": `^+2020-10-10`

```project.txt
# This task is planned to be done on October 13th 2020.
^[ ] Buy rocket parts ^+2020-10-13
```

##### Start dates
Sometimes you know in advance that a task cannot start before a certain date. You can use the start date command to note this down. Noting down earliest start dates helps in getting a more accurate prediction on how long a project will take. You can set an earliest start date similar to a due or do date, but this time you use a `*`. Think of this as the sun rising, marking the start of the day: `^*2020-10-27`.

```project.txt
# This task cannot start before October 27th 2020.
^[ ] Buy paint ^*2020-10-27
```

##### Remaining effort
To find out how long your project is going to take it is a good idea to put some estimation of remaining effort on your tasks. You can write an effort estimation like this: `^~4d3h2m`. The `~` is used to convey an estimation (it us used in mathematics as an approximation indicator which nicely applies to estimations as well). Supported durations are days (3 days = `3d`) hours (2 hours = `2h`) and minutes (25 minutes = `25m`). Durations can appear in any order, so `20m2h` is the same as `2h20m`. You can leave out any of the days, hours and minutes parts (but not all of them). If you have tasks that span more than a few days, it may be a good idea to break them down into smaller pieces to get a better idea of what the task actually entails.

```project.txt
# Building a rocket will take 4 days.
^[ ] Build a rocket ^~4d

# Buying paint will take 3 hours 30 minutes.
^[ ] Buy paint ^~3h30m
```

##### Task dependencies
In any non-trivial project you will have dependencies between tasks. You can express dependencies by writing `^<#some-task`. The `<` indicates that some other tasks need to be finished before this task can be done. In order to refer to some other task, this task needs to have an ID, a label or a tag. This also allows you to quickly specify a dependency between groups of tasks. For example, if you write `^<@my-label` then this task will depend on all tasks that have the `@my-label` tag. Likewise, if you write `^<@phase:mvp` then this task will depend on all tasks that labeled as being in the `mvp` phase. 

```project.txt
^[ ] Buy paint ^#buy-paint

# I need to buy paint before I can paint the door.
^[ ] Paint door ^<#buy-paint
```

Using tags or labels as reference in dependencies is especially useful if you want to track milestones.

```project.txt
^[ ] Buy paint ^@buying-phase
^[ ] Buy brushes ^@buying-phase

^[ ] Paint the door ^@painting-phase
^[ ] Paint the room ^@painting-phase

^[ ] Clean everything ^@cleanup-phase
^[ ] Put in furniture ^@cleanup-phase

# This milestone depends on all tasks tagged with @buying-phase
^[M] Buying phase finished ^<@buying-phase   ^@intermediate-milestone

# This milestone depends on all tasks tagged with @painting-phase
^[M] Painting phase finished  ^<@painting-phase  ^@intermediate-milestone

# This milestone depends on all tasks tagged with @cleanup-phase
^[M] Cleanup phase finished ^<@cleanup-phase ^@intermediate-milestone

# This milestone depends on all intermediate milestones
^[M] Everything finished ^<@intermediate-milestone
```

There is also a shortcut available which allows you to add a dependency on  the task that is immediately before the task where you add the dependency. This is useful if you have a list of tasks you know you need to work on sequentially and you don't want to define IDs or labels for all of them. To make a task dependent on the task before it, simply type `^<<`.

```project.txt
^[ ] This needs to be done first
^[ ] This needs to be done second ^<<
^[ ] This needs to be done third ^<<
```
This shortcut also makes it easy to re-order tasks by just reordering them in the document.

##### Task assignments
Task assignments allow you to assign tasks to people. They work very similar to task dependencies. You can assign a task to a person by writing `^>#person`. The arrow pointing to the person indicates that this person is responsible for the task. As with task dependencies you can also refer to people with a tag or with a label instead of an ID.

```project.txt
^[ ] This task is assigned to Jeff ^>#jeff
^[ ] This task is assigned to gamma team ^>@gamma-team
```

