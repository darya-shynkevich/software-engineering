*==As a general rule the longer a transaction is open the more contention and potentially failure can occur.==*

Two-phase commit:
- Leader writes a durable transaction record indicating a cross-shard transaction.
- Participants write a permanent record of their willingness to commit and notify the leader.
- The leader commits the transaction by updating the durable transaction record after receiving all responses. (It can abort the transaction if no one responds.)
- Participants can show the new state after the leader announces the commit decision. (They delete the staged state if the leader aborts the transaction.)

    *Read-and-write amplification in the protocol path is a major issue. Write amplification occurs because you must write a transaction record and durably stage a commit, which requires at least one write per participant. Lock contention and application instability can result from excessive writes. ==The database must additionally filter every read to ensure that it does not see any state that is dependent on a pending cross-shard transaction, which affects all reads in the system, even non-transactional ones.*==
