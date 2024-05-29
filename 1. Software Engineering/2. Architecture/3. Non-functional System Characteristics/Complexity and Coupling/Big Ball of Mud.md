A BIG BALL OF MUD is a casually, even haphazardly, structured system. Its organization, if one can call it that, is dictated more by expediency than design. Yet, its enduring popularity cannot merely be indicative of a general disregard for architecture.

1. The "**throwaway code**"  is code that is created quickly and without proper documentation with the intention of being used only once. However, this code often ends up being used repeatedly and can lead to a messy and disorganized system over time.
   
2. Even systems with well-defined architectures can suffer from **structural erosion** due to changing requirements. As the system evolves and new elements are added gradually, the system can become overgrown and lose its structure. *Regularly refactoring the system can prevent it from becoming disorganised.*
   
3. The analogy of a city can be used to explain that change in a system often happens gradually, with one building or block being changed at a time while the system as a whole continues to function. The strategy of "**keeping it working**" is discussed as a way to preserve the vitality and growth of a system.
   
	Rather than creating throwaway code, developers should adopt a mindset of "keeping it working." *This means dedicating time and effort to ensure that the codebase remains reliable, maintainable, and continuously assesses the potential impact of changes.*

	To achieve this, the authors suggest *implementing testing strategies, refactoring modules to improve their design, applying design patterns, and continuously integrating new code with the existing system to avoid creating isolated sections that can quickly turn into throwaway code.*

	It is important to treat software development as an ongoing process rather than a series of hasty fixes. By focusing on keeping the code working and maintaining its quality, developers can avoid creating a big ball of mud and reduce technical debt in the long run.
   
4. The fourth paragraph discusses how systems and their components evolve at different rates. Elements that change quickly become distinct from those that change more slowly, and the "**shearing layers**" that develop between them can help create enduring abstractions in the system.
   
5. Ways to control decline in a system:m one strategy mentioned is to cordon off problematic areas and hide them behind an attractive facade, which is referred to as "**sweeping it under the rug**". In more extreme cases, the only solution may be to **completely reconstruct** the system, salvaging only the underlying patterns that hold the system together.

	Developers often prioritise immediate results over long-term sustainability, resulting in the proliferation of quick and dirty solutions that are implemented without considering the overall architecture or implications on the system as a whole. These temporary patches and compromises accumulate over time, making the codebase increasingly difficult to maintain, understand, and extend.

	To counteract this trend, fostering a culture of continual improvement, where development teams are encouraged to prioritise technical debt reduction and invest time and resources in refactoring. It also emphasises the importance of early detection and proactive resolution of issues, as well as the need for open and honest communication within the team and with stakeholders.

	To tackle the reconstruction process, it is recommended to break the system down into manageable components or modules. It is advocated to adopt the modular design principle and separation of concerns, which involve dividing the system into distinct layers and functionalities. This approach allows for independent development and maintenance of different parts of the system.

## [Speed matter](https://ai.googleblog.com/2009/06/speed-matters.html)

Speed as perceived by the end user is driven by multiple factors, including how fast results are returned and how long it takes a browser to display the content. Our experiments injected server-side delay to model one of these factors: extending the processing time before and during the time that the results are transmitted to the browser. In other words, we purposefully slowed the delivery of search results to our users to see how they might respond.  
  
All other things being equal, more usage, as measured by number of searches, reflects more satisfied users. Our experiments demonstrate that slowing down the search results page by 100 to 400 milliseconds has a measurable impact on the number of searches per user of -0.2% to -0.6% (averaged over four or six weeks depending on the experiment). That's 0.2% to 0.6% fewer searches for changes under half a second!

Furthermore, users do fewer and fewer searches the longer they are exposed to the experiment. Users exposed to a 200 ms delay since the beginning of the experiment did 0.22% fewer searches during the first three weeks, but 0.36% fewer searches during the second three weeks. Similarly, users exposed to a 400 ms delay since the beginning of the experiment did 0.44% fewer searches during the first three weeks, but 0.76% fewer searches during the second three weeks. Even if the page returns to the faster state, users who saw the longer delay take time to return to their previous usage level. Users exposed to the 400 ms delay for six weeks did 0.21% fewer searches on average during the five week period after we stopped injecting the delay.

While these numbers may seem small, a daily impact of 0.5% is of real consequence at the scale of Google web search, or indeed at the scale of most Internet sites. Because the cost of slower performance increases over time and persists, we encourage site designers to think twice about adding a feature that hurts performance if the benefit of the feature is unproven.

## References:

1. http://www.laputan.org/pub/foote/mud.pdf
2. https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.93.8928 
3. https://ai.googleblog.com/2009/06/speed-matters.html