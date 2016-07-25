# DbFaker - A .NET database populator for testing purposes
===

Travis: [![Travis](https://travis-ci.org/stone-pagamentos/DbFaker.svg?branch=master)](https://travis-ci.org/stone-pagamentos/DbFaker)

## Project Description ##
DbFaker is a tool designed to automatically fills your database with random data to make your unit/integration tests faster.  
The generator will auto maps all foreign keys and generates records to related tables.

## Getting Started ##
1. Install DbFaker with NuGet Package Manager:
> Install-Package DbFaker

2. Install your database dialect, example:
> Install-Package DbFaker.SqlServer

3. Configure the required connection strings.

To start generating your test data, create a new DataFactory object:
```csharp
using DbFaker;
using DbFaker.SqlServer;
...
var dataFactory = new DataFactory<SqlServer>("MyConnectionString");
```

## Usage ##

Consider the example schema:

#### User Table  
| Column | Data Type | PK | FK |
| -- | -- | -- | -- |
| Id | INT | True | False |
| Name | VARCHAR(20) | False | False |
| Email | VARCHAR(50) | False | False |
| Age | INT | False | False |
| GroupId| INT| False| Group(Id) |

#### Group Table  
| Column | Data Type | PK | FK |
| -- | -- | -- | -- |
| Id | INT | True | False |
| Name | VARCHAR(20) | False | False |

### 1. Creates a single record:
```csharp
...
IDicionary<string, object> record = dataFactory.CreateOne("dbo.User");
```
The method will creates an record in the group table, and associates it to the created user. The dictionary will contains:
```json
{
  "Id": "Generated User's Id",
  "Name": "Random string",
  "Email": "Random string",
  "Age": "Random integer number",
  "GroupId": "Id from generated group record"
}
```
So you can specify your custom values. Do following:
```csharp
User user = dataFactory.CreateOne("dbo.User", t => {
    t.WithValue("Name", "Will");
});
```

The dictionary will contains:
```json
{
  "Id": "Generated User's Id",
  "Name": "Will",
  "Email": "Random string",
  "Age": "Random integer number",
  "GroupId": "Id from generated group record"
}
```

### 2. Delete generated records:
To delete all generated records, just run:
```csharp
...
dataFactory.TearDown();
...
```

## Contributors ##
- @anckizes
- @pedrohfernandes