# Index types

___

**Q: What types of indexes are there in SQL?**

In order to make lookups faster, SQL databases employ indexes.

Indexes contain keys built from one or more columns. They can either be clustered or non-clustered.

Clustered index alters the table by sorting rows based on their key values. There can only be one clustered index per table because the data can be sorted only in one way at a time.

Non-clustered index resides outside of the table and represents a mapping of an index key to a row. There can be multiple non-clustered indexes per table because they don't affect the table structure.

Both type of indexes can be configured as unique, in which case rows are not allowed to have duplicate key values.

Indexes are automatically maintained when the table is modified.
