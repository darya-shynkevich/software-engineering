1. Race conditions between execution threads
2. Time-based sensitivity in tests, ex. not using a mocked clock
3. Lack of sorting non-deterministic data structures

# Solutions

#### CI test grinder (proactive)

Whenever a test is introduced into the codebase, grind (run) the test 10 times to try to catch any flaky tests before they get merged into main.

#### CI reliability report cards (reactive)

Daily cron analyzes all the CI builds for the main branch from the past day, finds the builds that failed, and then team will look over the logs to parse out the specific jobs that failed.

Post the resulting “report card” into a Slack channel for visibility. CI owners can glance at which jobs failed and drill in for more detail if necessary.

#### Automated flaky test skipping (reactive)

If, for a specific test, there is a pattern of pass → fail → pass, then automatically generate a PR to skip it. JIRA tickets are generated for every flaky test and assigned to owning teams.

#### Selective testing (reactive)

Introduced logic that looks at a PR’s changed files, and matches those files with a set of glob → CI check mappings to determine which checks need to be run.

In addition, statically analyze the import graph of our codebase to detect which test suites need to be run. If a Go service A was changed, and Go service B does not depend on A at all, there’s no need to run B’s tests.

Note that selective testing reduces the chances of flakiness. Imagine having a test that flakes 50% of the time, but only half the CI runs actually trigger the flaky test. It’s effectively only flaking 25% of the time now!

# References:

1. ~~[Improving CI/CD with a Focus on Developer Velocity](https://www.samsara.com/blog/improving-ci-cd-with-a-focus-on-developer-velocity)~~
2. ~~[Flaky Tests Overhaul at Uber](https://www.uber.com/en-IN/blog/flaky-tests-overhaul/?utm_source=substack&utm_medium=email)~~