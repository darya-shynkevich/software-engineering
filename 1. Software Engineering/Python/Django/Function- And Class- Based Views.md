
Whenever you implement a view, think about whether it would make more sense to implement as an FBV or as a CBV. Some views are best implemented as CBVs, and others are best implemented as FBVs.

# Django Views Are Functions

When it comes down to it, every Django view is a function. This function takes an HTTP request object and turns it into an HTTP response object.

Django’s CBVs appear to be very different than FBVs. However, the `View.as_view()` classmethod called in URLConfs is actually returning a callable instance of the view. In other words, a callback function that handles the request/response cycle in exactly the same manner as a function-based view!

![[Pasted image 20231126234230.png]]

# Best Practices for Function-Based Views

1. Less view code is better.
2. Never repeat code in views.
3. Views should handle presentation logic. Try to keep business logic in models when possible, or in forms if you must.
4. Keep your views simple.
5.  Use them to write custom 403, 404, and 500 error handlers.
6. Complex nested-if blocks are to be avoided.

# Best Practices for Class-Based Views

1. Less view code is better.
2. Never repeat code in views.
3. Views should handle presentation logic. Try to keep business logic in models when possible, or in forms if you must.
4.  Keep your views simple.
5. Keep your mixins simpler.

> ***ccbv.co.uk**  takes all the attributes and methods that every CBV defines or inherits and flattens it into one comprehensive page per view.*

In programming, ***a mixin is a class that provides functionality to be inherited, but isn’t meant for instantiation on its own***. In programming languages with multiple inheritance, mixins can be used to add enhanced functionality and behaviour to classes.

When using mixins to compose our own view classes, we recommend these rules of inheritance provided by Kenneth Love. The rules follow Python’s method resolution order, which in the most simplistic definition possible, proceeds from left to right:
1. The base view classes provided by Django always go to the right.
2. Mixins go to the left of the base view.
3. Mixins should not inherit from any other class. Keep your inheritance chain simple!

Django CBV Usage Table

![[Pasted image 20231126235524.png]]


# Reference:

1. [Django Views — The Right Way](!https://spookylukey.github.io/django-views-the-right-way/)