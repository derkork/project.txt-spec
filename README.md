# The Project.txt format

## What is it?
Project.txt is an open-source lightweight human-writable and human-consumable format to write up a series of connected tasks that form a project. It is inspired by todo.txt which is a similar format for tracking personal todo items. 

The format is plain text with minimal markup. It allows to connect decentralized teams on a project plan that everyone can see and edit with a simple text editor. Synchronization can be done by any means, be it a git repository, a cloud storage share, some shared folder on a network drive or whatever is available in your setup.

This format is intended as a toolkit for setting up your projects. You may not need all the features this offers and this is totally fine. Use what you need and ignore the rest.

## How does it look like?
Project.txt is a very simple text based format. All you need is a text editor. A very simple project could look like this:

```project.txt
# Who is working on the project:

+ Sandy :#sandy
  Wants to paint a door.
+ Anna  :#anna
  Sandy's daughter who will help her.

# The tasks 

# Anna is responsible for buying stuff and she already completed her tasks.
[x] Buy paint 
    :>#anna :@buying-phase
[x] Buy brushes 
    :>#anna :@buying-phase

# Sandy is responsible for painting
# Painting the door is currently in progress. It depended on the materials
# being bought:
[>] Paint the door 
    :>#sandy :<@buying-phase :#door  

# Sandy and Anna will clean up together. They can however only do it after the door
# is painted. They estimate it will take them an hour to do so.
[ ] Clean up 
    :>#sandy :>#anna :<#door :~1h
```

## How do I express... ?
### Comments
You can start a comment almost everywhere with `#`. Comments help you to structure the file to your needs. The comment always runs to the end of the line.
```project.txt
# This line is a comment.

[ ] This is a task #with some comment
```
### People
A project has people working on it. You can define people by starting a line with a `+`. You can add additional information to people, like what their responsibilities in the project are, contact data, etc.
```project.txt
+ John Smith 
    This is John. He does our web development.
+ Jane Muller
    This is Jane. She is our project manager.
+ Andrea Viticetti
    This is Andrea. She is our tester.
```

### Tasks
A project has tasks that need to be done. You can define tasks by starting a line with `[ ]`:

```project.txt
[ ] Gather requirements
[ ] Write code
[ ] Test implementation
```

As with people you can add additional information to tasks, like a link to your issue tracker, notes about progress, etc.

```project.txt
[ ] Implement phase shield feature
    https://tracker.example.com/issues/STARSHIP-2931
    Needs to be done quickly because client wants to test
    the feature soon.
```

#### Task states
A task can have several states, indicated by a character between the `[ ]`.

```project.txt
[ ] This task is open. It still needs to be done.

[>] This task is in progress. 
    Somebody is currently working at it but it's not yet 
    finished.

[!] This task is on-hold.
    There is something blocking this task, e.g. you need
    to wait for an external thing to happen.

[x] This task is done.

[M] This task is a milestone.
    It will automatically be considered as reached when all
    tasks coming before it are done.
```


### Instructions
You can add additional information to people, e.g. what their email address is, to which team they belong etc. It is also possible to add additional information to tasks like when a task is due, an estimation of how long a task will probably take, task dependencies and a few more. All of this is done using _instructions_. An instruction always starts with a `:`.

#### Generally available instructions
Some instructions are available for both tasks and people. 
##### Tags
A tag allows you to put a mark on a task or a person which you can later use to filter out people or tasks. A tag is an instruction, so it starts with `:` as all instructions do. The `:` is followed by an `@`:
```project.txt
# This marks the 'Gather requirements' task as a high priority task.
[ ] Gather requirements :@high-priority
``` 
There is no set pre-defined tags, so you can use any tag name that fits your use case and your organization. E.g. if you have multiple people you may want to group them in teams. You could use tags for this:

```project.txt
+ Jeff :@epsilon-team
+ Anna :@gamma-team
+ Angela :@gamma-team
```

Our you could mark tasks that are your minimum viable product:

```project.txt
[ ] Implement rocket boosters :@mvp
```

You can have as many tags as you want on each person or task:
```project.txt
# Two tags marking Jeff as a member of epsilon team and a project manager.
+ Jeff :@epsilon-team :@project-managers

# Two tags marking the rocket boosters as both a task belonging to the 
# minimum viable product and a high-priority task.
[ ] Rocket boosters :@mvp :@high-priority
```
##### Labels
A label is very similar to a tag. While a tag is a single name a label is a pair of a name and a value. There is some functional overlap between tags and labels, as they both are intended to mark entries for later filtering. Choose whatever works best for you. As the tag the label is an instruction so it starts with `:`, then followed by `@`, a name then a `:` and the value:

```project.txt
+ Jeff: :@team:epsilon
[ ] Implement rocket boosters :@phase:mvp :@priority:high
```

Apart from that, the same rules as for tags apply - you can have as many as you would like per entry and there are no predefined labels. Use what makes sense in your project and team setup.

##### IDs
IDs allow you to cross reference entries with each other. They are used for example if you would like to assign tasks to people or make tasks dependent on each other. An ID is an instruction so it again starts with `:` then followed by a `#` and some unique identifier. 

```project.txt
# This will assign the ID `#jeff` to Jeff
+ Jeff Smith :#jeff

# This will assign the ID `#TK1272` to the task
[ ] Implement rocket boosters :#TK1272
```

For IDs you can use any word or character sequence as long as it is unique. 

#### Instructions for people
##### Email address
For people there exists an instructions to note down their email address. As all instructions it starts with a `:` followed by `=>` and the actual email address:

```project.txt
+ Jeff Smith :=>jeff.smith@example.com 
  This is Jeff. He does things.
```

#### Instructions for tasks
For tasks you have quite a few instructions available. Note that you don't necessarily need to use all of them. Use what you need on your project, just because an instruction is available, it doesn't mean it makes sense to use it in all projects.

##### Due dates
Tasks may have due dates. This makes sense for tasks that need to hit a certain deadline. In many cases it makes sense to put a due date on a milestone to see if you can reach a milestone in time - but you can add a due date to normal tasks as well. A due date instruction starts with `:` - like all instructions, followed by `!` and the date in `year-month-day` format:

```project.txt
# This task is due on October 21st 2020.
[ ] Buy rocket parts :!2020-10-21
```

##### Do dates
You may want to plan ahead which task should be performed at which day. For this you can use do dates. A do date instruction starts with `:` followed by a `+` and the date in `year-month-day` format:

```project.txt
# This task is planned to be done on October 13th 2020.
[ ] Buy rocket parts :+2020-10-13
```

##### Start dates
Sometimes you know in advance that a task cannot start before a certain date. You can use the start date instruction to note this down. Noting down earliest start dates helps in getting a more accurate prediction on how long a project will take. The start date instruction again starts with a `:` followed by a `*` and the date in `year-month-day` format:

```project.txt
# This task cannot start before October 27th 2020.
[ ] Buy paint :*2020-10-27
```



##### Remaining effort
To find out how long your project is going to take it is a good idea to put some estimation of remaining effort on your tasks. An effort instruction starts with a `:` followed by a `~` and a duration:

```project.txt
# Building a rocket will take 4 days.
[ ] Build a rocket :~4d

# Buying paint will take 3 hours 30 minutes.
[ ] Buy paint :~3h30m
```

Supported durations are days (3 days = `3d`) hours (2 hours = `2h`) and minutes (25 minutes = `25m`). Durations can appear in any order, so `20m2h` is the same as `2h20m`.

##### Task dependencies
In any non-trivial project you will have dependencies between tasks. You can express them with the dependency instruction. For this the task which is the prerequisite needs to have an ID, tag or label so it can be referenced. A dependency instruction like all instructions starts with `:` followed by a `<` and the ID, tag or label of the task(s) that is/are the prerequisite:

```project.txt
[ ] Buy paint :#TSK001

# I need to buy paint before I can paint the door.
[ ] Paint door :<#TSK001
```

Using tags or labels as reference in dependencies is especially useful if you want to track milestones.

```project.txt
[ ] Buy paint :@buying-phase
[ ] Buy brushes :@buying-phase

[ ] Paint the door :@painting-phase
[ ] Paint the room :@painting-phase

[ ] Clean everything :@cleanup-phase
[ ] Put in furniture :@cleanup-phase

# This milestone depends on all tasks tagged with @buying-phase
[M] Buying phase finished :<@buying-phase   :@intermediate-milestone

# This milestone depends on all tasks tagged with @painting-phase
[M] Painting phase finished :<@painting-phase  :@intermediate-milestone

# This milestone depends on all tasks tagged with @cleanup-phase
[M] Cleanup phase finished :<@cleanup-phase :@intermediate-milestone

# This milestone depends on all intermediate milestones
[M] Everything finished :<@intermediate-milestone
```

There is also a shortcut available which allows you to add a dependency on  the task that is immediately before the task where you add the dependency. This is useful if you have a list of tasks you know you need to work on sequentially and you don't want to define IDs or labels for all of them. To make a task dependent on the task before it, simply type `:<<`

```project.txt
[ ] This needs to be done first
[ ] This needs to be done second :<<
[ ] This needs to be done third :<<
```
This shortcut also makes it easy to re-order tasks by just reordering them in the document.

##### Task assignments
Task assignments allow you to assign tasks to people. They work very similar to task dependencies. To assign a task you use the task assignment instruction. It starts with a `:` followed by `>` and then the ID, label or tag of a person.

```project.txt
[ ] This task is assigned to Jeff :>#jeff
[ ] This task is assigned to gamma team :>@gamma-team
```

