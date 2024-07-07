> **[debugging code is twice as hard as writing it](https://read.engineerscodex.com/p/clever-code-is-probably-the-worst#:~:text=There%E2%80%99s%20a-,popular%20saying,-that%20debugging%20code)**

Maintainability has many facets, but in essence it’s about *making life better for the engineering and operations teams who need to work with the system.* Good abstractions can help reduce complexity and make the system easier to modify and adapt for new use cases. Good operability means having good visibility into the system’s health, and having effective ways of managing it.

***Operability***: Make it easy for operations teams to keep the system running smoothly.

***Simplicity***: Make it easy for new engineers to understand the system, by removing as much complexity as possible from the system. (Note this is not the same as simplicity of the user interface.)

*There are various possible symptoms of complexity: explosion of the state space, tight coupling of modules, tangled dependencies, inconsistent naming and terminology, hacks aimed at solving performance problems, special-casing to work around issues elsewhere, and many more. Much has been said on this topic already*

***Evolvability***: Make it easy for engineers to make changes to the system in the future, adapting it for unanticipated use cases as requirements change. Also known as extensibility, modifiability, or plasticity.

# How to measure?

We use (mean time to repair) MTTR as the metric to measure `M`.

![](../../../_Attachments/Pasted%20image%2020240118165301.png)

In other words, MTTR is the average amount of time required to repair and restore a failed component. Our goal is to have as low a value of MTTR as possible.

# Maintainability and [[Reliability]]

Maintainability can be defined more clearly in close relation to reliability. The only difference between them is the variable of interest. Maintainability refers to `time-to-repair`, whereas reliability refers to both `time-to-repair` and the `time-to-failure`. Combining maintainability and reliability analysis can help us achieve availability, downtime, and uptime insights.
# [Documentation](Documentation.md)

# Technical Debt

Early on when you are standing up your system its important to keep things simple and as [loosely coupled](Coupling.md) as possible. This is both in architecture and implementation.

*The perfect race car crosses the finish line in first place and immediately falls into pieces.*

[[Complexity]] is inevitable, don't worry you won't need to try hard. Each new component is more one component that needs to be accounted for when making a change.

# Reference:

1. ~~[Clever code is probably the worst code you could write](https://read.engineerscodex.com/p/clever-code-is-probably-the-worst)~~
2. ~~[The Controversial Truth about Tech Debt](https://medium.com/@raphael.moutard/the-controversial-truth-about-tech-debt-09ad0f787888)~~