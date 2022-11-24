# NEA Planning

## The analysis phase

### My plan for the next 6 weeks

<!-- spell-checker:words proto,SpyHoodle -->

| Task                  | Description                           | Deadline   | Completed |
| --------------------- | ------------------------------------- | ---------- | --------- |
| Contact client        | Contact SpyHoodle about objectives    | 2022-10-20 |           |
| Consider goals        | Write down some initial goals         | 2022-10-27 |           |
| Research TUI library  | TUI-rs and Cursive are options        | 2022-11-10 |           |
| Research SQL library  | It's probably SQLx                    | 2022-11-10 |           |
| Make a basic proto    | It'd be good to see that stuff works  | 2022-11-20 |           |
| Show client the proto | See if they like proto, confirm goals | 2022-11-30 |           |

### Planned SQL tables

#### Tasks

| Column      | Description       | Type   | Extra  |
| ----------- | ----------------- | ------ | ------ |
| ID          |                   | int    | PK, AI |
| Description | What is the task? | string |        |

#### Repeats

| Column | Description                   | Type | Extra |
| ------ | ----------------------------- | ---- | ----- |
| Task   | The task that we're repeating | FK   | CPK   |
| Type   | The type of repeat            | ENUM | CPK   |
| Start  | The date the repeat starts    | DATE | CPK   |

##### Repeats.Type

| ENUM    |
| ------- |
| Daily   |
| Weekly  |
| Monthly |
| Yearly  |

### Questions to ask my client

- What features do you think are essential in a task-list program?
- Here are some features I'm considering adding: rate them in order of priority:
  - Repeated events
  - Plugin support
  - Notification support
  - iCal (a common calendar format used by Google, MS etc. to export) import
    support
  - iCal support with network subscription
  - Task priority ranking
  - Multiple task sort orders
  - Multiple user support
  - Online syncing
- Is there anything else you'd like added?
- Is there anything I definitely *shouldn't* include?

### Planned features

- Repeat tasks
- Single-task mode (avoid overwhelming)


