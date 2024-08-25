- **Join promotion** refers to changing inner joins to left outer joins.
- **Join demotion** is the action of changing left joins to inner joins.

The reason Django ORM uses inner joins is that they are much more performant
(the difference can be orders of magnitude). There is no other reason to
demote joins to inner joins. That is, when using the Django ORM, results will
always be correct even when all joins are set to left outer joins.

Join demotion is done in case the ORM can prove the results will be correct even
when inner joins are used. We can always demote any join that must have a value
on the remote side of the join (that is, join along non-nullable foreign key).
We will also demote joins when filtering in such a way that the filter can only
match when the value is non-null.

# References:

1. [Django join promotion](!https://gist.github.com/abrookins/533d5a1371667443bd7d7973375f3400)