# NEA Planning

## The analysis phase

### My plan for the next 6 weeks

<!-- spell-checker:words proto -->

| Task                  | Description                           | Deadline   | Completed |
| --------------------- | ------------------------------------- | ---------- | --------- |
| Contact client        | Contact PineappleFan about objectives | 2022-10-20 |           |
| Consider goals        | Write down some initial goals         | 2022-10-27 |           |
| Research TUI library  | TUI-rs and Cursive are options        | 2022-11-10 |           |
| Research SQL library  | It's probably SQLx                    | 2022-11-10 |           |
| Make a basic proto    | It'd be good to see that stuff works  | 2022-11-20 |           |
| Show client the proto | See if they like proto, confirm goals | 2022-11-30 |           |

### Planned SQL tables

#### Tasks

| Column      | Description       | Type   | Extra  |
|-------------|-------------------|--------|--------|
| ID          |                   | int    | PK, AI |
| Description | What is the task? | string |        |

#### Repeats

| Column | Description                   | Type | Extra |
|--------|-------------------------------|------|-------|
| Task   | The task that we're repeating | FK   | CPK   |
| Type   | The type of repeat            | ENUM | CPK   |
| Start  | The date the repeat starts    | DATE | CPK   |

##### Repeats.Type

| ENUM    |
|---------|
| Daily   |
| Weekly  |
| Monthly |
| Yearly  |
