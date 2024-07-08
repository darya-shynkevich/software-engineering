# [_Base](1.%20Software%20Engineering/3.%20Database/OTLP/SQL/5.%20Distributed/Distributed%20Transactions/_Base.md)

! No transaction support => Client decides to which server it will connect:

IF client_id IN range(0....10000000) => connect to server_1
ELIF client_id IN range(10000000....) => connect to server_2
ELIF ....

# References:

1. vitess.io - middleware for MySQL to do a magic sharding