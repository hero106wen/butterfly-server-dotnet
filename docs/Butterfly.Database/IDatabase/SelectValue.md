# IDatabase.SelectValue&lt;T&gt; method

Executes the SELECT statement and return the value of the first column of the first row (the SELECT statement may contain vars like @name specified in *vars*)

```csharp
public Task<T> SelectValue<T>(string selectStatement, object vars, T defaultValue)
```

| parameter | description |
| --- | --- |
| T | The return type of the single value returned |
| selectStatement | The SELECT statement to execute (may contain vars like @name specified in *vars*) |
| vars | Either an anonymous type or Dictionary with the vars used in the SELECT statement |
| defaultValue | The value to return if no rows were returned or the value of the first column of the first row is null |

## Return Value

The value of the first column of the first row

## See Also

* interface [IDatabase](../IDatabase.md)
* namespace [Butterfly.Database](../../Butterfly.Database.md)

<!-- DO NOT EDIT: generated by xmldocmd for Butterfly.Database.dll -->