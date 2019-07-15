# Consistency models

___

**Q: What consistency models in distributed systems do you know about?**

Consistency models regulate how a change to data is observed by a consumer in the context of replication.

Strong consistency - order of read and write operations is preserved globally. Reads are always guaranteed to observe fully converged data.

Sequential consistency - order of operations is only preserved per session. Reads are not guaranteed to observe fully converged data, but the observed data is guaranteed to be consistent with the per-session order of operations. Changes made within current session are guaranteed to be observed instantly.

Causal consistency - order is only preserved for operations in causal relationships. Reads are not guaranteed to observe fully converged data, but the observed data is guaranteed to be consistent with the causal order of operations.

Eventual consistency - order is not preserved. Reads are not guaranteed to observe fully converged data, and there's also no guarantee that the observed data is consistent with the per-session or causal order of operations.

___

**Q: Can you give an example area of applicability for each consistency model?**

Strong consistency is used in contexts where data always has to be up to date. Example: processing financial transactions.

Sequential consistency can be used when changes made by one user need to be instantly reflected for that user but not necessarily other users. Example: adding an item to cart.

Causal consistency can be used when changes don't have to be instantly reflected, but if they do, they have to be in same causal order. Example: replying to someone's comment.

Eventual consistency can be used when changes don't have to be instantly reflected and they don't have to preserve any order. Example: voting in an election.
