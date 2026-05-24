# Application 3 Documentation  
# ToDo Application using Blazor (Binding, Events & Parameters)

---

# Project Overview

This application is a **Blazor-based ToDo system** that demonstrates core frontend and component-based concepts:

- One-way data binding  
- Two-way data binding  
- Event handling  
- Component communication (Parent → Child, Child → Parent)  
- State management  
- Dynamic UI rendering  

The application follows a **modular architecture** where logic is divided into components.

---

# Project Structure

| Layer | File |
|------|------|
| Pages | Home.razor |
| Pages | OneWay.razor |
| Pages | TwoWay.razor |
| Pages | ToDoPage.razor |
| Pages | ToDoComponent.razor |
| Classes | TaskItem.cs |

---
# Application 3 Documentation

# ToDo Application using Blazor

---

## TASK MODEL (Class Layer)

## Code

```csharp
namespace BindingEventsParameters.Classes
{
    public class TaskItem
    {
        public string TaskDescription { get; set; }
        public bool IsComplete { get; set; }
    }
}
```

---

## Explanation

### Purpose of this class

This class represents a **single task entity** in the application. Every task that user creates in UI is stored as an object of this class.

---

### How it works in application flow

1. User enters task in input field
2. String is stored temporarily
3. New `TaskItem` object is created
4. Object is added into `List<TaskItem>`
5. UI renders list dynamically

---

### Key idea

This class is acting as a **data model layer**, separating structure from UI logic.

---

### Variables explanation

- `TaskDescription` → stores task text written by user  
- `IsComplete` → stores completion state (true/false)

---

## HOME PAGE

## Code

```razor
@page "/"

<PageTitle>Home</PageTitle>

<h1>Hello, world!</h1>

Welcome to your new app.
```

---

## Explanation

### Purpose

This is the **entry point page** of the application.

---

### Functionality

- Only displays static content  
- No logic or state management  
- Used to confirm application is running  

---

### Flow

Application start → Router loads `/` → Home page displayed  

---
### Output (Screenshot)


<img width="1882" height="901" alt="image" src="https://github.com/user-attachments/assets/0fa6c257-f204-4503-8249-7a3d1ca1c060" />

---

## ONE WAY BINDING PAGE

## Code

```razor
@page "/oneway"

<h3>@Title</h3>

@code {
    string Title = "The name of this page is: One Way Binding";
}
```

---

## Code Breakdown

### Part 1: Variable declaration

```csharp
string Title = "The name of this page is: One Way Binding";
```

### Explanation

This variable is created in backend logic and holds static text.

---

### Part 2: UI binding

```razor
<h3>@Title</h3>
```

### Explanation

- `@Title` is used to display backend value in UI  
- This is **one-way binding**

---

## Full Concept Explanation

### What is happening here?

Data is flowing only in one direction:

Backend (C#) → UI (Razor)

UI cannot change the value of `Title`.

---

### Real behavior in application

1. Page loads  
2. Variable `Title` is initialized  
3. Blazor renders UI  
4. Value appears on screen  

---

### Key concept

This is used when we only want to **display data**, not edit it.

---

### Output (Screenshot)


## One Way Binding
<img width="1872" height="908" alt="image" src="https://github.com/user-attachments/assets/02fef7d7-d532-472f-965c-eae42b737142" />

---

## TWO WAY BINDING PAGE

## Code

```razor
@page "/twoway"

<h3>Two Way Binding</h3>

<p>Slider Value: <b>@SliderValue</b></p>

<input type="range"
       @bind-value="SliderValue"
       @bind-value:event="oninput" />

@code {
    int SliderValue = 0;
}
```

---

## Code Breakdown

### Part 1: Variable

```csharp
int SliderValue = 0;
```

### Explanation

This variable stores numeric value of slider.

---

### Part 2: Display

```razor
<p>Slider Value: <b>@SliderValue</b></p>
```

### Explanation

This shows current value of slider in UI.

---

### Part 3: Binding logic

```razor
<input type="range"
       @bind-value="SliderValue"
       @bind-value:event="oninput" />
```

### Explanation

This is **two-way binding implementation**.

- When user moves slider → value updates variable  
- When variable changes → UI updates automatically  

---

## Full Concept Explanation

### What is Two-Way Binding?

It means:

UI ↔ Backend synchronization

---

### Application Flow

1. Page loads  
2. Slider initialized at 0  
3. User moves slider  
4. Value updates `SliderValue`  
5. UI text updates instantly  

---

### Key idea

No manual event handling is needed — Blazor handles synchronization automatically.

---
### Output (Screenshot)

## Two Way Binding
<img width="1892" height="868" alt="image" src="https://github.com/user-attachments/assets/ecbc27e9-f734-4553-a4f3-669c215b8a69" />


---

## TO DO PAGE (MAIN COMPONENT - PARENT)

## Code

```razor
@page "/ToDoPage"
@using BindingEventsParameters.Classes
@rendermode InteractiveServer

<h3>ToDo Dashboard</h3>

<select @bind="SelectedColor">
    <option value="green">Green</option>
    <option value="red">Red</option>
    <option value="blue">Blue</option>
</select>

<button @onclick='() => SetFilter("All")'>All</button>
<button @onclick='() => SetFilter("Pending")'>Pending</button>
<button @onclick='() => SetFilter("Completed")'>Completed</button>

<TodoComponent Tasks="Tasks"
               RemoveTaskChanged="RemoveTask" />

<input @bind="newTaskDescription" />
<button @onclick="AddTask">Add</button>

<button @onclick="ClearAll">Clear All</button>

@code {

    private List<TaskItem> Tasks = new();
    private string newTaskDescription;
    private string SelectedColor = "green";

    protected override void OnInitialized()
    {
        Tasks.Add(new TaskItem { TaskDescription = "Learn Blazor", IsComplete = false });
        Tasks.Add(new TaskItem { TaskDescription = "Build ToDo App", IsComplete = true });
    }

    private void AddTask()
    {
        if (!string.IsNullOrWhiteSpace(newTaskDescription))
        {
            Tasks.Add(new TaskItem
            {
                TaskDescription = newTaskDescription,
                IsComplete = false
            });

            newTaskDescription = "";
        }
    }

    private void RemoveTask(TaskItem task)
    {
        Tasks.Remove(task);
    }

    private void ClearAll()
    {
        Tasks.Clear();
    }

    private void SetFilter(string filter)
    {
    }
}
```

---

# TO DO PAGE — DEEP EXPLANATION (IMPORTANT PARTS)

---

# PART 1: STATE VARIABLES

## Code

```csharp
private List<TaskItem> Tasks = new();
private string newTaskDescription;
private string SelectedColor = "green";
```

---

## Explanation

### Core Idea

These are the **core state variables** of the application.

---

### Variables Meaning

- **Tasks** → main data storage (all tasks)  
- **newTaskDescription** → input field binding  
- **SelectedColor** → UI styling state  

---

### Logic Role

This page works as a **state manager (similar to backend controller)** for the entire ToDo system.

---

# PART 2: INITIAL DATA

## Code

```csharp
protected override void OnInitialized()
{
    Tasks.Add(new TaskItem { TaskDescription = "Learn Blazor", IsComplete = false });
    Tasks.Add(new TaskItem { TaskDescription = "Build ToDo App", IsComplete = true });
}
```

---

## Explanation

### Purpose

This method runs automatically when the page loads.

---

### Functionality

- Adds sample tasks on startup  
- Simulates database-like initial data  

---

### Flow

Page Load → Method Executes → Tasks Appear in UI

---

# PART 3: ADD TASK LOGIC

## Code

```csharp
private void AddTask()
{
    if (!string.IsNullOrWhiteSpace(newTaskDescription))
    {
        Tasks.Add(new TaskItem
        {
            TaskDescription = newTaskDescription,
            IsComplete = false
        });

        newTaskDescription = "";
    }
}
```

---

## Explanation

### Purpose

This method adds a new task to the list.

---

### Step-by-Step Flow

1. User types task in input field  
2. Clicks **Add** button  
3. System checks if input is empty  
4. New `TaskItem` object is created  
5. Object is added to `Tasks` list  
6. Input field is cleared  
7. UI updates automatically  

---

### Result

New task appears dynamically in the ToDo list.

---

# PART 4: DELETE TASK

## Code

```csharp
private void RemoveTask(TaskItem task)
{
    Tasks.Remove(task);
}
```

---

## Explanation

### Purpose

Removes a selected task from the list.

---

### Flow

User clicks Delete → Task passed to method → Task removed from list → UI refreshes

---

### Result

Selected task disappears from UI immediately.

---

# PART 5: CLEAR ALL TASKS

## Code

```csharp
private void ClearAll()
{
    Tasks.Clear();
}
```

---

## Explanation

### Purpose

Removes all tasks at once from the system.

---

### Flow

Button Click → ClearAll Method Executes → List Emptied → UI Becomes Empty

---

### Result

Entire ToDo list resets to initial empty state. 

---

### Variables

| Variable | Purpose |
|----------|--------|
| Tasks | Stores all tasks |
| newTaskDescription | Input binding |
| SelectedColor | UI theme |

---

## TO DO COMPONENT (CHILD COMPONENT)

## Code

```razor
@using BindingEventsParameters.Classes

<ul>

@foreach (var task in Tasks)
{
    <li>

        <input type="checkbox"
               @bind="task.IsComplete" />

        @task.TaskDescription

        <button @onclick="() => Remove(task)">
            Delete
        </button>

    </li>
}

</ul>

@code {

    [Parameter]
    public List<TaskItem> Tasks { get; set; }

    [Parameter]
    public EventCallback<TaskItem> RemoveTaskChanged { get; set; }

    private async Task Remove(TaskItem task)
    {
        await RemoveTaskChanged.InvokeAsync(task);
    }
}
```

---

## Explanation

### Purpose

This component is responsible for displaying tasks and handling user interaction.

---

### Communication Flow

- Parent → Child = Parameter  
- Child → Parent = EventCallback  

---

### Logic Flow

User Action → Child Event → Parent Method → State Update → UI Re-render  

---

### Output (Screenshot)

## ToDo Page - Add Task

<img width="1886" height="871" alt="image" src="https://github.com/user-attachments/assets/24a6b474-595a-4111-a82e-eccabccbe8ce" />

---
### Output (Screenshot)

## ToDo Page - Delete Task
<img width="1882" height="868" alt="image" src="https://github.com/user-attachments/assets/da7647b3-5f06-4110-9a20-8740967178ed" />

---
### Output (Screenshot)

## ToDo Page - Clear All Tasks
<img width="1893" height="878" alt="image" src="https://github.com/user-attachments/assets/1a704d69-0b75-4852-ad93-012cff2069a4" />

---
### Output (Screenshot)

## ToDo Page - Task Complete Ticked(Green line shows completed or pending)
<img width="1891" height="886" alt="image" src="https://github.com/user-attachments/assets/7b0bb34a-0a39-4b8e-b86f-9e6393eff43a" />

---
## FINAL ARCHITECTURE FLOW

```
User Action
   ↓
Child Component
   ↓ (EventCallback)
Parent Component
   ↓
State Update
   ↓
UI Re-render
```

---

## FINAL CONCLUSION

This application demonstrates:

- Real Blazor component architecture  
- Clean separation of concerns  
- Proper state management  
- Real-world event-driven design  
- Data binding techniques  

It behaves like a **mini production-level frontend system**.
