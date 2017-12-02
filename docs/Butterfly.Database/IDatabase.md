# IDatabase interface

Allows executing INSERT, UPDATE, and DELETE statements; creating dynamic views; and receiving data change events both on tables and dynamic views.

Adding records and echoing all data change events to the console...

```csharp
// Create database instance (will also read the schema from the database)
var database = new SomeDatabase();

// Listen for all database data events
var databaseListener = database.OnNewCommittedTransaction(dataEventTransaction => {
    console.WriteLine($"Low Level DataEventTransaction={dataEventTransaction}");
});

// INSERT a couple of records (this will cause a single data even transaction with
// two INSERT data events to be written to the console above)
using (var transaction = database.BeginTransaction()) {
    await database.InsertAndCommitAsync("employee", values: {
        department_id: 1,
        name: "SpongeBob"
    });
    await database.InsertAndCommitAsync("employee", values: {
        department_id: 1,
        name: "Squidward"
    });
    await database.CommitAsync();
);
```

Creating a DynamicView and echoing data change events on the DynamicView to the console...

```csharp
// Create database instance (will also read the schema from the database)
var database = new SomeDatabase();

// Create a DynamicViewSet that print any data events to the console
// (this will immediately echo an INITIAL data event for each existing matching record)
var dynamicViewSet = database.CreateAndStartDynamicViewSet(
    "SELECT * FROM employee WHERE department_id=@departmentId", 
    new {
        departmentId = 1
    },
    dataEventTransaction => {
        Console.WriteLine(dataEventTransaction);
    }
);

// This will cause the above DynamicViewSet to echo an INSERT data event
await database.InsertAndCommitAsync("employee", values: {
    department_id: 1
    name: "Mr Crabs"
});

// This will NOT cause the above DynamicViewSet to echo an INSERT data event
// (because the department_id doesn't match)
await database.InsertAndCommitAsync("employee", values: {
    department_id: 2
    name: "Patrick Star"
});
```

```csharp
public interface IDatabase
```

## Members

| name | description |
| --- | --- |
| [Tables](IDatabase/Tables.md) { get; } | Dictionary of tables keyed by name |
| [BeginTransaction](IDatabase/BeginTransaction.md)() | Creates a new [`ITransaction`](ITransaction.md). Must execute [`CommitAsync`](ITransaction/CommitAsync.md) to save the transaction changes. Calling Dispose() on the transaction without committing rolls back the changes./&gt; |
| [CreateAndStartDynamicView](IDatabase/CreateAndStartDynamicView.md)(…) | Convenience method which creates a [`DynamicViewSet`](../Butterfly.Database.Dynamic/DynamicViewSet.md), adds a single [`DynamicView`](../Butterfly.Database.Dynamic/DynamicView.md) to the [`DynamicViewSet`](../Butterfly.Database.Dynamic/DynamicViewSet.md), and starts the [`DynamicViewSet`](../Butterfly.Database.Dynamic/DynamicViewSet.md). (2 methods) |
| [CreateDynamicViewSet](IDatabase/CreateDynamicViewSet.md)(…) | Allows creating a set of [`DynamicView`](../Butterfly.Database.Dynamic/DynamicView.md) instances that publish a single DataEventTransaction instance with initial data and new DataEventTransaction instances when data changes. The DataEventTransaction instances are published to the lambda passed as the *listener*. (2 methods) |
| [CreateFromResourceFileAsync](IDatabase/CreateFromResourceFileAsync.md)(…) | Creates database tables from an embedded resource file by internally calling CreateFromTextAsync with the contents of the embedded resource file ([`CreateFromTextAsync`](IDatabase/CreateFromTextAsync.md). |
| [CreateFromTextAsync](IDatabase/CreateFromTextAsync.md)(…) | Creates database tables from a string containing a semicolon delimited series of CREATE statements in MySQL format (will be converted to native database format as appropriate). |
| [DeleteAndCommitAsync](IDatabase/DeleteAndCommitAsync.md)(…) | Executes the DELETE statement as a single transaction (the DELETE statement may contain vars like @name specified in *vars*) |
| [GetInitialDataEventsAsync](IDatabase/GetInitialDataEventsAsync.md)(…) | Executes the SELECT statement of the DynamicQuery and returns a sequence of DataChange events starting an InitialBegin event, then an Insert event for each row, and then an InitialEnd event. |
| [GetInitialDataEventTransactionAsync](IDatabase/GetInitialDataEventTransactionAsync.md)(…) | Execute the SELECT statement and return the data in a DataEventTransaction |
| [InsertAndCommitAsync](IDatabase/InsertAndCommitAsync.md)(…) | Executes the INSERT statement as a single transaction (the INSERT statement may contain vars like @name specified in *vars*) |
| [OnNewCommittedTransaction](IDatabase/OnNewCommittedTransaction.md)(…) | Adds a listener that is invoked when there is a new committed transaction |
| [OnNewUncommittedTransaction](IDatabase/OnNewUncommittedTransaction.md)(…) | Adds a listener that is invoked when there is a new uncommitted transaction |
| [SelectRowAsync](IDatabase/SelectRowAsync.md)(…) | Executes the SELECT statement and return the first row (the SELECT statement may contain vars like @name specified in *vars*) |
| [SelectRowsAsync](IDatabase/SelectRowsAsync.md)(…) | Executes the SELECT statement and return the rows (the SELECT statement may contain vars like @name specified in *vars*) |
| [SelectValue&lt;T&gt;](IDatabase/SelectValue.md)(…) | Executes the SELECT statement and return the value of the first column of the first row (the SELECT statement may contain vars like @name specified in *vars*) |
| [SetInsertDefaultValue](IDatabase/SetInsertDefaultValue.md)(…) | Allows specifying a lambda that creates a default value for a field when executing an INSERT. If the tableName parameter is left null, the getDefaultValue lambda will be applied to all tables. |
| [UpdateAndCommitAsync](IDatabase/UpdateAndCommitAsync.md)(…) | Executes the UPDATE statement as a single transaction (the UPDATE statement may contain vars like @name specified in *vars*) |

## See Also

* namespace [Butterfly.Database](../Butterfly.Database.md)

<!-- DO NOT EDIT: generated by xmldocmd for Butterfly.Database.dll -->