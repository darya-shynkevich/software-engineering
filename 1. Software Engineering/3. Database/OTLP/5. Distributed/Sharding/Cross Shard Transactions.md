# [Distributed Transactions](../Distributed%20Transactions.md)

! No transaction support => Client decides to which server it will connect:

IF client_id IN range(0....10000000) => connect to server_1
ELIF client_id IN range(10000000....) => connect to server_2
ELIF ....

# References:

1. vitess.io - middleware for MySQL to do a magic sharding