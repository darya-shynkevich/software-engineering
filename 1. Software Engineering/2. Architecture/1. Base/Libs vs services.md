**Library** - any software that can be [run by the user](https://catern.com/run.html): shared objects, modules, servers, command line utilities, and others

**Service** - any software which the user can't run on their own; anything which depends (usually through an API) on a service provider for its functionality.

(-) administration costs => a library (where viable) can provide the same functionality to the user, at a lower cost to the developer than a service
(+) centralisation of these administration costs => can avoid slow-to-upgrade problem
	**BUT** these users can't have a negative impact on other users, then you don't care if some users are slow to upgrade; they're only hurting themselves.

"Write a library", in this sense, can still mean writing a standalone server reached through a network protocol. ***As long as the user runs the server themselves, you're still saving the administration costs.***

# References:

1. ~~[Write libraries instead of services, where possible](https://catern.com/services.html?ref=architecturenotes.co)~~