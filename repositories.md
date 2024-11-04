# Repositories with MAUI

I'll be using the models from [E.F. Core](https://github.com/raphaelfrei/maui-guides/blob/main/ef-core.md)

For folders, you should create this:

````
- Repositories
  > Interfaces
````

## 1. Create Interface for each models:

Name should be something like ````IModelRepository.cs````, so in my case ````ITaskModelRepository.cs```` and ````ISubTaskModelRepository.cs````.

## 1.1. Create everything you want to pass to SQL:

In this example, we'll have 5 operations for ````ITaskModelRepository.cs````:

1. Get all tasks
2. Get task by ID
3. Add task
4. Update task
5. Delete task

The operations implemented:

````csharp
namespace AppTaskTODO.Repositories.Interfaces {

    public interface ITaskModelRepository {

        IList<TaskModel> Tasks { get; }

        TaskModel GetTaskByID(int id);

        void AddTask(TaskModel task);

        void UpdateTask(TaskModel task);

        void DeleteTask(TaskModel task);

    }
}
````

## 2. Implementing the Interface:

Now, let's create an file called ````TaskModelRepository.cs```` inside Repositories folder...

Set the class to public and implements the Interface:

````csharp
using AppTaskTODO.Repositories.Interfaces;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace AppTaskTODO.Repositories {

    public class TaskModelRepository : ITaskModelRepository {

    }

}
````

To get rid of "...Repository" does not implement member, you'll have to create all operations from the interface.

````csharp
using AppTaskTODO.Models;
using AppTaskTODO.Repositories.Interfaces;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace AppTaskTODO.Repositories {

    public class TaskModelRepository : ITaskModelRepository {

        public IList<TaskModel> Tasks { get; }

        public TaskModel GetTaskByID(int id);

        public void AddTask(TaskModel task) {

        }

        public void UpdateTask(TaskModel task) {

        }

        public void DeleteTask(TaskModel task) {

        }

    }

}
````

Now, let's implement the AppDbContext that you created on the constructor:

````csharp
private readonly AppDbContext _dbContext;

public TaskModelRepository(AppDbContext dbContext) {
    _dbContext = dbContext;
    
}
````

Now, let's implement the methods:

````csharp
using AppTaskTODO.Database;
using AppTaskTODO.Models;
using AppTaskTODO.Repositories.Interfaces;
using Microsoft.EntityFrameworkCore;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace AppTaskTODO.Repositories {

    public class TaskModelRepository(AppDbContext dbContext) : ITaskModelRepository {

        private readonly AppDbContext _dbContext = dbContext;

        public IList<TaskModel> Tasks => _dbContext.Tasks.ToList();

        public TaskModel GetTaskByID(int id) => _dbContext.Tasks.Include(sb => sb.SubTasks).FirstOrDefault(ts => ts.TaskID == id);

        public void AddTask(TaskModel task) {
            _dbContext.Tasks.Add(task);
            _dbContext.SaveChanges();
        }

        public void UpdateTask(TaskModel task) {
            _dbContext.Tasks.Update(task);
            _dbContext.SaveChanges();
        }

        public void DeleteTask(TaskModel task) {
            _dbContext.Tasks.Remove(task);
            _dbContext.SaveChanges();
        }

    }

}
````
