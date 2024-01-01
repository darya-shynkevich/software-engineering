*There are many sources of uncertainty in software. The biggest is that we just don't know how to make perfect software: bugs can and will be deployed to production. No matter how much testing you do, bugs will slip through. Because of this fact of software development, all software must be viewed as probabilistic. ==The code you write only has some probability of being correct for all inputs.== Sometimes seemingly minor failures will interact in ways that lead to much greater failures.*

### Engineering for uncertainty

1. **Minimize dependencies**: a tradeoff between minimizing dependencies and minimizing the amount of code you need to produce to implement your application.
2. **Lessen probability of cascading failures**: a) making interactions between components in your system explicitly respect those input ranges by using self-throttling to avoid accidental DOS'ng. b) isolate your components as much as possible and take away the ability for different components to affect each other
3. **Measure and monitor**: software should be designed from the start to be monitored. *==Monitoring is the most important defense against software's inherent uncertainty.==* 

## The one key skill I look for to judge seniority

	1. “can we do this?”
	2. “how do we do this well?”
	3. “should we do this?”
	4. “what should we do?”
	5. “what shouldn’t we do?”



### References:

1. [Principles of Software Engineering](http://nathanmarz.com/blog/principles-of-software-engineering-part-1.html)