# Transaction isolation level

___

**Q: What is transaction isolation level in SQL?**

Transaction isolation level defines what data can be observed by read statements inside a running SQL transaction.

___

**Q: What isolation levels are available?**

`READ UNCOMMITTED` - a transaction running at this isolation level can read uncommitted data made by other transactions.

`READ COMMITTED` - a transaction can only read committed data made by other transactions. This is the default option.

`REPEATABLE READ` - a transaction can only read committed data made by other transactions and also places locks on them so that other transactions can't modify that data until current transaction completes.

`SNAPSHOT` - a transaction can only read data that was available at the start of the current transaction along with any uncommitted changes made in the current transaction.

`SERIALIZABLE` - transactions that interact with the same data are run sequentially rather than concurrently.
