# SQL Server at Know History Guide
This style guide is for SQL Server code and database development, and is the default style for T-SQL at Know History. Which naming convention is used, depends on each person and organization. This document aims to clarify naming standards for SQL Server under the Know History umbrella.

## Naming Conventions

### Tables
When naming your database tables, give consideration to other steps in the development process. Keep in mind that you will most likely have to utilize the names you give your tables several times as part of other objects. e.g. procedures, triggers or views. 

You want to keep the name as simple and short as possible. The naming convention for a table name should be as follows:

#### _Singular Names_
Table names should be singular, for example, `Customer` instead of `Customers`. This rule is applicable because tables are patterns for storing an entity as a record – they are analogous to Classes serving up class instances. And if for no other reason than readability, you avoid errors due to the pluralization of English nouns in the process of database development. For instance, activity becomes activities, ox becomes oxen, person becomes people or persons etc. - Intersection tables that handle "many to many" relationships, e.g. Registry and Person tables should be written as `RegistryPerson`.

#### _Prefixes_
Don’t use prefixes unless they are deemed necessary to help you organize your tables into related groups or distinguish them from other unrelated tables. Generally speaking, prefixes will cause you to have to type a lot of unnecessary characters.

#### _Notation_
For all parts of the table name, including prefixes, use Pascal Case. Using this notation will distinguish your table names from SQL keywords. PascalCase also reduces the need for underscores to visually separate words in names.

#### _Special Characters_
For table names, underscores should not be used. The underscore character has a place in other object names but, not for tables. Using PascalCase for your table name allows for the upper-case letter to denote the first letter of a new word or name. Thus there is no need to do so with an underscore character. Do not use numbers in your table names either. This usually points to a poorly-designed data model or irregularly-partitioned tables. Do not use spaces in your table names either.

#### _Abbreviations_
Avoid using abbreviations if possible. Use `Accounts` instead of `Accts` and `Hours` instead of `Hrs`. Not everyone will always agree with you on what your abbreviations stand for and this makes it simple to read and understand for both developers and non-developers. Avoid using acronyms as well. If exceptions to this rule are deemed necessary, ensure that the same convention is followed by all project members and that this is noted in the project's Readme file.

#### _Examples_
```
Person
PersonDetail
Registry
RegistryPerson
```

### Columns (incl. Primary and Foreign Keys)
When naming your columns, keep in mind that they are members of the table, so they do not need the any mention of the table name in the name. When writing a query against the table, you should be prefixing the  field name with the table name or an alias anyway. Just like with naming tables, avoid using abbreviations, acronyms or special characters. All column names should use PascalCase to distinguish them from SQL keywords.

#### _Primary Keys_
For fields that are the primary key for a table and uniquely identify each record in the table, the name should simply be `[tableName] + "Id"` (e.g. in an Ancestor table, the primary key field would be `AncestorId`. Though `AncestorId` conveys no more information about the field than `Ancestor.Id` and is a far wordier implementation, it is still preferable to having to type brackets around `Id`.

#### _Foreign Keys_
Foreign key fields should have the exact same name as they do in the parent table where the field is the primary. For example, in the Forebearer table the primary key field might be `ForebearerId`. In a Person table where the ForebearerId may be kept, it would also be `ForebearerId`. There is one exception to this rule, which is when you have more than one foreign key field per table referencing the same primary key field in another table. In this situation, it might be helpful to add a descriptor before the field name. An example of this is if you had an Address table. You might have another table with foreign key fields like `HomeAddressId`, `WorkAddressId`, `MailingAddressId`, or `ShippingAddressId`.

#### _Prefixes_
Do not prefix your fields with `fld_` or `Col_` as it should be obvious in SQL statements which items are columns (before or after the FROM clause). Do not use a data type prefix for the field either, for example, `IntCustomerId` for a numeric type or `VcName` for a varchar type. These "clog up" our naming and add little value.

#### _Data Type Specific Naming_
Bit fields should be given affirmative boolean names like `IsDeleted`, `HasPermission`, or `IsValid` so that the meaning of the data in the field is not ambiguous. If the field holds date and/or time information, the word "Date" or "Time" should appear somewhere in the field name. It is sometimes appropriate to add the unit of time to the field name also, especially if the field holds data like whole numbers ("3" or "20"). Those fields should be named like `RuntimeHours` or `ScheduledMinutes`.

#### _Field Name Length_
Field names should be no longer than 50 characters and all should strive for less lengthy names if possible. You should, however, not sacrifice readability for brevity and avoid using abbreviations unless it is absolutely necessary.

#### _Special Characters_
Field names should contain only letters and numbers. No special characters,  underscores or spaces should be used.

### Indexes
Indexes will remain named as the SQL Server default, unless the index created is for a special purpose. All primary key fields and foreign key fields will be indexed and named in the SQL Server default. Any other index will be given a name indicating it’s purpose.

#### _Naming Convention_
Follow this structure in the case of special-purpose indexes, where `U/N` is for unique or non-unique and `IX_` matches the default prefix that SQL Server assigns indexes:
  
```
{U/N}IX_{TableName}{SpecialPurpose}
```

### Constraints
Constraints are at the field/column level so the name of the field the constraint is on should be used in the name. The type of constraint should be noted also. Constraints are also unique to a particular table and field combination, so you should include the table
name also to ensure unique constraint names across your set of database tables.

#### _Naming Convention_
Constraints should be named using the following structure:

```
{ConstraintAbbreviation}{TableName}_{FieldName}
```

#### _Prefixes_
A two letter prefix gets applied to the constraint name depending on the type:

```
Primary Key: Pk
Foreign Key: Fk
Check      : Ck
Unique     : Uq
```

#### _Examples_

```
PkPerson_PersonId
FkAncestor_PersonId
CkPerson_AncestorId
UqAncestor_KnowHistoryId
```

### Views
Views follow many of the same rules that apply to naming tables. There are only two differences. If your view combines entities with a join condition or where clause, be sure to combine the names of the entities that are joined in the name of your view.

#### _Prefixes_
While it is pointless to prefix tables, it can be helpful for views. Prefixing your views with `vw` is a helpful reminder that you're dealing with a view, and not a table. Whatever type of prefix you choose to apply, use at least 2 letters and not just `v` because a prefix should use more than one letter or its meaning can be ambiguous. 

#### _View Types_
Some views are simply tabular representations of one or more tables with a filter applied or because of security procedures (users given permissions on views instead of the underlying table(s) in some cases). Some views are used to generate report data with more specific values in the WHERE clause. Naming your views should be different depending on the type or purpose of the view. For simple views that just join one or more tables with no selection criteria, combine the names of the tables joined. 

#### _Examples_
Joining the `Person` and `HarvestingArea` table to create a view of people and their respective harvesting areas should be given a name like `vwPersonHarvestingArea`. Views created expressly for a report should have an additional prefix of Report applied to them, e.g. `vwReportPersonHarvestingArea`.

### Stored Procedures
Unlike a lot of the other database objects discussed here, stored procedures are not logically tied to any table or column. Typically though, stored procedures perform one or more common database activities (Read, Insert, Update, and/or Delete) on a table, or another action of some kind. Since stored procedures always perform some type of operation, it makes sense to use a name that describes the operation they perform. Use a verb to describe the type of operation, followed by the table(s) the operations occur on.

#### _Prefixes_
A 3 letter prefix `usp` (user-created stored procedure) is applied to the beginning of the stored procedure name. Never use `sp_`, `xp_` or `dt_` as this will cause SQL Server to check the Master database for this procedure first, causing a performance hit.

#### _Naming Conventions_
Standard stored procedures should be named using the following structure:

```
usp{TableName(s)}_{DatabaseActivity}
```

There could be occasion where your procedure returns a scalar value, or performs an activity like validations, and in this case you should use the verb and noun combination:

```
usp{Verb}{Noun}
```

#### _Examples_

```
Standard
-----------------
uspPerson_Read
uspAncestor_Create
uspPersonAncestor_Update

Non-Standard
-----------------
uspValidateLogin
uspCheckStatus
```

### Functions
Functions follow many of the same rules that apply to naming stored procedures. There are only two differences that exist so the user of the function knows they are dealing with a function and not a stored procedure.

*__Prefixes__*
A 2 letter prefix `fn` is applied to the beginning of the function name. 

*__Function Purpose__*
Functions should always be named as a verb, because they will always return a value.

*__Examples__*

```
fnGetOpenDate
fnFormatExcel
```

### Triggers
Triggers have many things in common with stored procedures. However, triggers are different than stored procedures in two important ways. First, triggers don't exist on their own. They are dependent upon a table. So it is wise to include the name of this table in the trigger name. Second, triggers can only execute when an Insert, Update, or Delete happens on one or more of the records in the table. So it also makes sense to include the type of action that will cause the trigger to execute.

#### _Prefixes_
A 2 letter prefix `tr` is applied to the beginning of the trigger name.

#### _Multiple Operations_
If a trigger handles more than one operation (both INSERT and UPDATE for example) then include both operation abbreviations in your name. 

#### _Examples_

```
trPersonInsertUpdate
trAncestorMove
```

### Variables
In addition to the general naming standards regarding no special characters, no spaces, and limited use of abbreviations and acronyms, common sense should prevail in naming variables - variable names should be meaningful and natural.

#### _Name length limit_
Variable names should describe its purpose and not exceed 50 characters in length.

#### _Prefix_
All variables must begin with the `@` symbol. Do __NOT__ use `@@` to prefix a variable as this signifies a SQL Server system global variable and will affect performance.

#### _Case_
All variables should be written in camelCase.

#### _Special Characters_
Variable names should contain only letters and numbers. No special characters or spaces should be used.

#### _Examples_

```sql
@person
@personId
```

# Coding Conventions

## Basics

- All SQL Server operators (SELECT, FROM, WHERE, UPDATE etc.) should be written in uppercase.
- Use trailing commas

## SQL Statements

### SELECT
Use the more readable ANSI-Standard Join clauses (SQL-92 syntax).

```sql
SELECT
    p.PersonId,
    a.AncestorId
FROM Registry.Person p
INNER JOIN Registry.Ancestor a ON
    a.AncestorId = p.AncestorId
WHERE
    p.KnowHistoryId = '123'
```

Never use the `SELECT *` convention when gathering results from a permanent table, instead specify the field names and bring back only those fields you need; this optimizes query performance and eliminates the possibility of unexpected results when fields are added to a table. 

Prefix all table names with the table owner. This results in a performance gain as the optimizer does not have to perform a lookup on execution as well as minimizing ambiguities in your SQL.

```sql
SELECT 
    PersonId,
    Name,
    DateOfBirth
FROM Registry.Person
```

Use aliases for your table names in most SQL statements; a useful convention is to make the alias out of the first or first two letters of each capitalized table  name, e.g. `Ancestor` becomes `a` and `AncestorType` becomes `at`.

### INSERT
Always use a column list in your INSERT statements. This helps in avoiding problems when the table structure changes (like adding or dropping a column).

```sql
INSERT INTO Registry.Person (Name, DateOfBirth)
VALUES ('Example', '2022-05-30')
```

### Formatting
SQL statements should be arranged in an easy-to-read manner. When statements are written all to one line or not broken into smaller easy-to-read chunks, more complicated statements are very hard to decipher. By doing this and aliasing table names when possible, you will make column additions and maintenance of queries much easier.

## NOCOUNT
Use SET NOCOUNT ON at the beginning of your SQL batches, stored procedures for report output and triggers in production environments, as this suppresses messages like '(1 row(s) affected)' after executing INSERT, UPDATE, DELETE and SELECT statements. This improves the performance of stored procedures by reducing network traffic.

## Code Commenting
Important code blocks within stored procedures and user defined functions should be commented. Brief functionality descriptions should be included where important or complicated processing is taking place.

Stored procedures and functions should include at a minimum a header comment with a brief overview of the batches functionality.

```
-- single line comment use 2 dashes (--)

/*
block comments use ( /* ) to begin and ( */ ) to close
*/
```

## Data Access and Transactions
Always access tables in the same order in all your stored procedures and triggers consistently. This helps to avoid deadlocks. Other things to keep in mind to avoid deadlocks are: Keep your transactions as short as possible. Touch as little data as possible during a transaction. 

Never, ever wait for user input in the middle of a transaction. Do not use higher level locking hints or restrictive isolation levels unless they are absolutely needed. Make your front-end applications  deadlock-intelligent, that is, these applications should be able to resubmit the transaction in case the previous transaction fails with error 1205. In your applications, process all the results returned by SQL Server immediately so that the locks on the processed rows are released, hence no blocking.

Always check the global variable @@ERROR immediately after executing a data manipulation statement (like INSERT/UPDATE/DELETE), so that you can rollback the transaction in case of an error (@@ERROR will be greater than 0 in case of an error). This is important, because, by default, SQL Server will not rollback all the previous changes within a transaction if a particular statement fails. Executing SET XACT_ABORT ON can change this behavior. The @@ROWCOUNT variable also plays an important role in determining how many rows were affected by a previous data manipulation (also, retrieval) statement, and based on that you could choose to commit or rollback a particular transaction.

```sql
DECLARE @sqlError INT
SET @sqlError = 0

BEGIN TRANSACTION

    UPDATE Registry.Person
    SET
        Name = source.Name,
        DateOfBirth = source.DateOfBirth,
        IsLiving = source.IsLiving
    FROM Registry.Person AS target
    INNER JOIN #oldPerson AS source ON
        target.PersonId = source.PersonId

    SELECT @SqlError = @@error

    IF @SqlError = 0
    BEGIN
        COMMIT TRANSACTION
        DROP TABLE #oldPerson
    END
    ELSE
    BEGIN
        ROLLBACK TRANSACTION
    END

END
```

## Error Handling
Although some procedural processes may not require additional error handling, as shown above in the Data Access and Transactions section there may be instances within your stored procedures you wish to handle errors more gracefully. You can use error handling to rollback entire transactions if one piece fails, report a single failed process within a procedure back to the calling application…etc. There are some examples below of proper implementation of error handling within a SQL Server stored procedure.

```sql
CREATE PROCEDURE Registry.uspPerson_Delete
    @personId INT
AS
SET NOCOUNT ON

BEGIN TRY
    BEGIN TRANSACTION
        DELETE FROM Registry.Person
        WHERE PersonId = @personId
    COMMIT TRANSACTION
END TRY

BEGIN CATCH
    SELECT
        ERROR_NUMBER() as ErrorNumber,
        ERROR_MESSAGE() as ErrorMessage

    IF (XACT_STATE()) = -1
    BEGIN
        PRINT N'The transaction is in an uncommittable state. Rolling back transaction.'

        ROLLBACK TRANSACTION
    END

    IF (XACT_STATE()) = 1
    BEGIN
        PRINT N'The transaction is committable. Committing transaction.'

        COMMIT TRANSACTION
    END
END CATCH
```

## Foreign Key and Check Constraints
Unique foreign key constraints should ALWAYS be enforced across alternate keys whenever possible. Not enforcing these constraints will compromise data integrity.

Perform all your referential integrity checks and data validations using constraints (foreign key and check constraints) instead of triggers, as they are faster. Limit the use of triggers to auditing, custom tasks
and validations that cannot be performed using constraints.

Constraints save you time as well, as you don't have to write code for these validations, allowing the RDBMS to do all the work for you!

## Cursor Usage
Try to avoid server side cursors as much as possible. Always stick to a 'set-based approach' instead of a 'procedural approach' for accessing and manipulating data. Cursors can often be avoided by using SELECT
statements instead, or at worst, temporary tables and/or table variables.

Should the need arise where cursors are the only option, avoid using dynamic and update cursors. Make sure you define your cursors as local, forward only to increase performance and decrease overhead.

```sql
DECLARE @personId int
DECLARE personProcessCursor CURSOR LOCAL FAST_FORWARD FOR

SELECT PersonId
FROM Registry.Person
WHERE KnowHistoryId = 1939

OPEN personProcessCursor
FETCH NEXT FROM personProcessCursor INTO @personId
WHILE (@@fetch_status <> -1)
BEGIN
    IF (@@fetch_status <> -2)
    BEGIN
        DELETE FROM Registry.Ancestor
        WHERE personId = @personId
    END
    FETCH NEXT FROM personProcessCursor INTO @personId
END
CLOSE personProcessCursor
DEALLOCATE personProcessCursor
```

## Temporary Tables and Table Variables
Use temporary tables (e.g. #oldPerson) only when absolutely necessary. When temporary storage is needed within a SQL statement or procedure, it’s recommended that you use local table variables (e.g. @oldPersonId) instead if the amount of data stored is relatively small. This eliminates unnecessary locks on system tables
and reduces the number of recompiles on stored procedures. This also increases performance as table variables are created in RAM, which is significantly faster than disk. 

## Database Design Best Practices
Using SQL Servers RDBMS platform to its fullest potential is encouraged. Below are a few tips to aide in maximizing the built in features of relational database structures.

- Utilize enforced relationships between tables to protect data integrity when possible. Always remember...garbage in....garbage out. The data will only ever be as good as the integrity used to store it.

- Take care to normalize data as much as possible while still achieving peak performance and maintainability. Overnormalization can also be an issue as it can have a severe effect on performance and data maintenance and cause un-needed overhead. Entities and their proposed uses should be studied to formulate a data model that can balance integrity, scalability and performance.

- Make use of auto-numbered surrogate keys on all tables. This can reduce data storage where compound primary and foreign keys are prevalent and simplify relationships between data when normalizing data structures and writing queries.

- __Always__ use primary keys on tables! Even if you are enforcing integrity through other means (i.e. business layer, unique table index) always put a primary key on a table. Tables with no primary key are a problem waiting to happen.

- Utilize indexes and fill factors correctly. Nothing can speed up data throughput more than correct utilization of clustered indexes, regular indexes and fill factors. This can be tricky business but SQL Server has a small host of tools that can assist. Nothing better than experience when it comes to optimizing queries and insert/update performance.

- Database security is always a concern. Close attention must be paid when developing front end applications and stored procedures to protect against SQL injection attacks.
