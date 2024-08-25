Release flags are a useful technique and lots of teams use them. However, they should be [**your last choice**]([in his article](https://en.wikipedia.org/wiki/Feature_toggle)) when you're dealing with putting features into production.

(-) they give PMs an excuse to **not make hard decisions**, such as completely removing a feature.
(-) the codebase becomes more complex and **harder to maintain**.
(-) it is tricky to debug code and fix bugs (hard to reproduce them)
(-) **testing becomes harder (and lower quality)** - figuring out what combination of feature flags needs to be supported ( ‘_a combinatoric explosion of possible toggle states_’.)
(-) waste time on dead code

# How can you deal with it

1. Release toggles: 
	1. _Who is responsible for toggling it:_ Developers. Those types of toggles can exist in a configuration file.
	2. _When should you delete it:_ A week or two after the release of the feature.
2. Ops toggles:
	1. _Who is responsible for toggling it:_ your Ops team - probably through a dedicated tool.
	2. _When should you delete it:_ 1-2 weeks**,** once you gain confidence in the new feature. In rare cases, you might keep some “Kill Switches”, to stop non-critical functionality when the system is under unusually high load.
3. Experiment toggles: allows you to perform[A/B testing](A%20I%20B%20testing.md). You divide the users into groups, each getting a different experience. While the toggle is active, you collect enough data to make the final decision.
	1. _Who is responsible for toggling it:_ It varies, but mostly PMs.
	2. _When should you delete it:_ a few days/weeks - once you get enough data to make a final decision.
4. Permission toggles: “premium” features for paying customers, or “alpha” / “beta” features, for internal users / beta customers
	1. *Who is responsible for toggling it:* PMs
	2. *When should you delete it:* You will probably not be the one deleting it… Some poor developer will do it in a few years.

# References:

1. ~~[Feature flags are ruining your codebase](https://zaidesanton.substack.com/p/feature-flags-are-ruining-your-codebase)~~
2. [Feature Toggles (aka Feature Flags)](https://martinfowler.com/articles/feature-toggles.html)