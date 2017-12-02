# IDatabase.BeginTransaction method

Creates a new [`ITransaction`](../ITransaction.md). Must execute [`CommitAsync`](../ITransaction/CommitAsync.md) to save the transaction changes. Calling Dispose() on the transaction without committing rolls back the changes./&gt;

```csharp
public Task<ITransaction> BeginTransaction()
```

## Return Value

An [`ITransaction`](../ITransaction.md) instance (can then call InsertAsync(), UpdateAsync(), or DeleteAsync() on the ITransaction instance to make changes on the transaction)/&gt;

## See Also

* interface [ITransaction](../ITransaction.md)
* interface [IDatabase](../IDatabase.md)
* namespace [Butterfly.Database](../../Butterfly.Database.md)

<!-- DO NOT EDIT: generated by xmldocmd for Butterfly.Database.dll -->