# SQLite Implementation for .NET MAUI

Guide for using SQLite with .NET MAUI without using [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/).
*I actually don't know why someone would even think about using this, that's just a pain to implement...*

Example project - [financial-manager](https://github.com/raphaelfrei/financial-manager/).

## 01. The basics:

### A. Required addon:
The only required is [Microsoft.Data.Sqlite](https://www.nuget.org/packages/Microsoft.Data.Sqlite/).

### B. Script class:
I like to do the entire SQL communication into a single script.<br> 
So, everything that uses the DB, must call this file.

````csharp
﻿using System;
using Microsoft.Data.Sqlite;
using Microsoft.Maui.Controls;

public static class SQLConn {

    // By using AppDataDirectory, the app will keep the information stored inside the DB between compilation and new versions
    private static string DBName = Path.Combine(FileSystem.AppDataDirectory, "mycardb.db"); // Or the name that you like
    private static string ConnectionString = $"Data Source={DBName};";

}
````

## 02. Database implementation:
| User_ID | User_Name    | User_Email             | User_Password | User_Birthday |
| :---:   | :---:        | :---:                  | :---:         | :---:         |
| 1       | Raphael Frei | raphaelrfrei@gmail.com | A91kAmczx01ks | 14/02/2002    |
| 2       | John Smith   | john.s@gmail.com       | kmAnxm08123As | 01/12/1983    |

**User_ID:** Integer; Unique; Auto Increment; PK.<br>
**User_Name:** Text; Not Null.<br>
**User_Email:** Text; Not Null; Unique.<br>
**User_Password:** Text; Not Null.<br>
**User_Birthday:** Text; Not Null.<br>

Obs.: The *minus* is not a valid table or db name - Must use underline.<br>

## 03. Database CREATE and DROP:
The database creation and drop must be made by code - It's not possible to implement the .DB file into your project.

````csharp
﻿using System;
using Microsoft.Data.Sqlite;
using Microsoft.Maui.Controls;

public static class SQLConn {

    private static string DBName = "dbase.db"; // Or the name that you like
    private static string ConnectionString = $"Data Source={DBName};";

    private static void CreateDatabaseIfNotExists(){
        using (var conn = new SqliteConnection(ConnectionString)) {
            conn.Open();

            string createUserTable = "CREATE TABLE IF NOT EXISTS \"user_mgr\" (\"user_id\" INTEGER UNIQUE, \"user_name\" TEXT NOT NULL, \"user_email\" TEXT NOT NULL UNIQUE, \"user_password\" TEXT NOT NULL, \"user_birthday\" TEXT NOT NULL, PRIMARY KEY(\"user_id\" AUTOINCREMENT))";

            using (var command = new SqliteCommand(createUserTable, conn)) {
                command.ExecuteNonQuery();
            }

            conn.Close();
        }
    }

    public static void DropAllTables(){
        using (var conn = new SqliteConnection(ConnectionString)) {
            conn.Open();

            var command = new SqliteCommand($"DROP TABLE IF EXISTS user_mgr", conn);
            command.ExecuteNonQuery();

            conn.Close();
        }
    }
}
````

## 04. Run SQL Commands:
There are two ways to add a new register - One that's possible to receive an SQL injection and one where's not possible.

````csharp
﻿using System;
using Microsoft.Data.Sqlite;
using Microsoft.Maui.Controls;

public static class SQLConn {

    private static string DBName = "dbase.db"; // Or the name that you like
    private static string ConnectionString = $"Data Source={DBName};";

    /* SQL Injection */
    public static (bool, string) LoginUser(string email, string password){

         CreateDatabaseIfNotExists();

        // Status 0 - Login OK
        // Status 1 - Error

        string selectCommand = $"SELECT user_password FROM user_mgr WHERE user_email='{email}'";
        string countCommand = $"SELECT COUNT(*) FROM user_mgr WHERE user_email='{email}'";

        try {

            using (var conn = new SqliteConnection(ConnectionString)) {
                conn.Open();
    
                using (var cmd = new SqliteCommand(countCommand, conn)) {
    
                    int count = Convert.ToInt32(cmd.ExecuteScalar());
    
                    if (count == 0)
                        return (1, "This email does not exists in the system.");
    
                    using (var cmdd = new SqliteCommand(selectCommand, conn)) {
                        using (var reader = cmdd.ExecuteReader()) {
                            if (reader.Read()) {
                                string storedPassword = reader.GetString(reader.GetOrdinal("user_password"));
    
                                if (storedPassword == ConvertToMD5.ConvertStringToMD5(password))
                                    return (0, "");
                                else
                                    return (1, "The password does not match the saved.");
                            }
                        }
                    }
    
                }
    
                conn.Close();
            }

        } catch (Exception ex) {
            return (1, ex.ToString());
        }
    }

    /* Avoid SQL Injection */
    public static (bool, string) LoginUser(string email, string password){
    
        CreateDatabaseIfNotExists();
    
        // Status 0 - Login OK
        // Status 1 - Error
    
        string selectCommand = "SELECT user_password FROM user_mgr WHERE user_email=@Email";
        string countCommand = "SELECT COUNT(*) FROM user_mgr WHERE user_email=@Email";
    
        try {
            using (var conn = new SqliteConnection(ConnectionString)) {
                conn.Open();
    
                using (var cmd = new SqliteCommand(countCommand, conn)) {
                    cmd.Parameters.AddWithValue("@Email", email);
    
                    int count = Convert.ToInt32(cmd.ExecuteScalar());
    
                    if (count == 0)
                        return (1, "This email does not exists in the system.");
    
                    cmd.CommandText = selectCommand;
                    using (var reader = cmd.ExecuteReader()) {
                        if (reader.Read()) {
                            string storedPassword = reader.GetString(reader.GetOrdinal("user_password"));
    
                            if (storedPassword == ConvertToMD5.ConvertStringToMD5(password))
                                return (0, "");
                            else
                                return (1, "The password does not match the saved.");
                        }
                    }
                }
    
                conn.Close();
            }
        } catch (Exception ex) {
            return (1, ex.ToString());
        }
    }
}
````

## 05. Get Count:

````csharp
﻿using System;
using Microsoft.Data.Sqlite;
using Microsoft.Maui.Controls;

public static class SQLConn {

    private static string DBName = "dbase.db"; // Or the name that you like
    private static string ConnectionString = $"Data Source={DBName};";

    public static int GetEmailCount(string email) {

        int count = 0;

        string selectCommand = $"SELECT COUNT(*) FROM user_mgr WHERE user_email='{email}'";

        try {
            using (var conn = new SqliteConnection(ConnectionString)) {
                conn.Open();

                using (var command = new SqliteCommand(selectCommand, conn)) {

                    count = Convert.ToInt32(command.ExecuteScalar());

                }

                conn.Close();
            }
        } catch (Exception) {

        }

        return count;
    }
}
````
