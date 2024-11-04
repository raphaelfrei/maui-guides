# Entity Framework Core with MAUI

This are my two models, that I'll be using during the steps:

1- TaskModel.cs
````csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace AppTaskTODO.Models {

    [Table("Tasks")]
    public class TaskModel {

        [Key]
        public int TaskID { get; set; }

        [Required(ErrorMessage = "The task name is mandatory")]
        [StringLength(100, ErrorMessage = "The task name must not exceed {1} caracters")]
        public string TaskName { get; set; } = string.Empty;

        public string TaskDescription { get; set; } = string.Empty;

        public DateTime PrevisionDate { get; set; } = DateTime.MinValue;

        public bool IsCompleted { get; set; }   

        public List<SubTaskModel> SubTasks { get; set; } = new List<SubTaskModel>();

    }

}
````

2- SubTaskModel.cs
````csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace AppTaskTODO.Models {

    [Table("SubTasks")]
    public class SubTaskModel {

        [Key]
        public int SubTaskID { get; set; }

        [Required(ErrorMessage = "The subtask name is mandatory")]
        [StringLength(100, ErrorMessage = "The subtask name must not exceed {1} caracters")]
        public string SubTaskName { get; set; } = string.Empty;

        public int TaskID { get; set; }

        public virtual TaskModel TaskModel { get; set; } = new TaskModel();

        public bool IsCompleted { get; set; }

    }

}
````

## 1. Additional Projects *(Optional)*:

### 1.1. Models:

Add an **CLASS LIBRARY** to store models into your project.<br>
*(Suggested name is YOUR_PROJECT.Models)*<br>

### 1.2. Database:

Add an **CLASS LIBRARY** to store Database settings.<br>
*(Suggested name is YOUR_PROJECT.Database)*<br>

### 1.3. Migration:

Add an **CONSOLE APPLICATION** to store Migrations code.<br>
*(Suggested name is YOUR_PROJECT.Database.Migration.Runtime)*<br>

### 1.4. Main Project:

On the main project, only add references to ````MY_PROJECT.Database```` and ````MY_PROJECT.Models````

## 2. Required packages:

Install these packages inside ````MY_PROJECT.Database```` project:

> Microsoft.EntityFrameworkCore<br>
> Microsoft.EntityFrameworkCore.DATABASE *(I'm using SQLite)*<br>

And this packages inside ````MY_PROJECT.Database.Migration.Runtime````:

> Microsoft.EntityFrameworkCore.Tools<br>

## 3. Create Classes:

We'll use ````MY_PROJECT.Database```` - Don't forget to add *Project Dependency* to ````MY_PROJECT.Models````

### 3.1. AppDbContext.cs:

This file is responsable for connecting the models with E.F. Core

````csharp
using Microsoft.EntityFrameworkCore;

namespace AppTaskTODO.Database {

    public class AppDbContext : DbContext {

    }

}
````

### 3.2. Setting Tables:

This step is required for every model that you would like in your project.

Inside the *AppDbContext.cs*, create the ````public DbSet<MODEL> TABLE_NAME { get; set; }````

In my case:

````csharp
using AppTaskTODO.Models;
using Microsoft.EntityFrameworkCore;

namespace AppTaskTODO.Database {

    public class AppDbContext : DbContext {

        // TASKS will be my table name - The name can be overridable by using DataAnnotations
        public DbSet<TaskModel> Tasks { get; set; }
        public DbSet<SubTaskModel> SubTasks { get; set; }

    }

}
````

### 3.3. Configure E.F. Code to SQLite:

Modify the **OnConfiguring** method inside the *AppDbContext.cs* to start using SQLite:

````csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder) {
    base.OnConfiguring(optionsBuilder);

    var dbPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "apptask.db");
    var connString = $"Filename={dbPath}";

    optionsBuilder.UseSqlite(connString);
    
}
````

### 3.4. Ensure versions are correct:

By setting up this constructor, we'll make sure that the DB version on the app is always updated.

````csharp
public AppDbContext() {
    Database.Migrate();
}
````

### 3.5. Full *AppDbContext.cs*:

````csharp
using AppTaskTODO.Models;
using Microsoft.EntityFrameworkCore;

namespace AppTaskTODO.Database {

    public class AppDbContext : DbContext {

        public DbSet<TaskModel> Tasks { get; set; }
        public DbSet<SubTaskModel> SubTasks { get; set; }

        public AppDbContext() {
            Database.Migrate();
        }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder) {
            base.OnConfiguring(optionsBuilder);
            //ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

            var dbPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "apptask.db");
            var connString = $"Filename={dbPath}";

            optionsBuilder.UseSqlite(connString);
            
        }   

    }

}
````

## 4. Migrations:

Inside **Migrations.Runtime**, add *Project Reference* to ````MY_PROJECT.Database```` and ````MY_PROJECT.Models````.

I'll be doing by using *Package Manager Console*, but you can use *NET CLI* as well.

**EXECUTE THIS WITH ````MY_APP.Database```` PROJECT**

The comands are:

1. Create Migration:

> add-migration NAME [options]

2. Apply Migration:

> update-database [options]

3. Remove Migration:

> remove-migration

So, for the first database commit:

> add-migration InitialCommit<br>
> update-database

````
For NET CLI:

Add Migration:
    dotnet ef migrations add NAME

Update on DB:
    dotnet ef database update

Remove last Migration:
    dotnet ef migrations remove
````

You can see what changes this will cause inside the *Migrations* folder
