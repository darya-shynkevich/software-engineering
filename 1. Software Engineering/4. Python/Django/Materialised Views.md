Sometimes, queries are just slow, and no matter how much you tune them they don’t get fast enough to run during a web request.

A very useful way to cache slow queries in the database itself: materialised views

**A materialised view** ***looks like a regular table for our purposes, but instead of containing its own data, it holds the results of a query.*** 

A timeline of events in the life of a materialised view might make this easier to understand: 
1. You give the database the name of a view like count_of_goal_viewed_events and a (probably slow) query that the database should run whenever you ask, which will refresh the view 
2. At some time, you tell the database to run the query it stored for the view and refresh the contents of the view with the result of the query 
3. Later, a client queries the count_of_goal_viewed_events view — your slow query does not run; instead, the database returns the results from step #2
4. Later, another client queries the count_of_goal_viewed_event view — once again, the database returns the results it saved from step #2

In summary, a materialised view lets you run a slow query, store the results, and reuse the results indefinitely.

**Drawbacks:**
1. The query you moved into a materialised view is still slow — which means it’s consuming database server resources. Because of this, you may have to scale that server up sooner than you’d like. 
2. A materialised view only helps with a slow query. What if your code is slow partially because of network latency on an API request? For that, you’ll need a cache.