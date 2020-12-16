# Present at the meeting:

* Andrey Mokhov (@snowleopard)
* Arseniy Alekseyev (@aalekseyev)
* Quentin Hocquet (@mefyl)

# Discussed

* Quentin worked out why hint mode wasn't helping with performances: we need to check for artifact presence in the cache twice:
  * When we first encounter and before sending the hint
  * When we obtain a job slot and before actually starting the process
* Because dune locks a job slot as many times as there are external commands in a rule, we will check multiple times if the cache has been filled and may short-circuit the rest of the actions in the middle of the rule. This may or may not be a problem.
* Andrey is worried that Jenga has a throttle on the number of rules looked-ahead to limit memory consumption, which could limit hinting efficiency.
* We discussed whether we could quickly and synchronously ask the distributed cache whether remote artifacts are available right away when encountering a rule. This would slightly slow down builds when the cache is empty and could hang for a long time if the RPC connection is saturated, so this should be seen as a last resort if the current approach fails.
* We discussed whether we could check for artifacts even after a command has been started, to potentially kill it and pull from the cache. While this could be a nice optimization, it sounds tricky and dangerous wrt external commands, so should be considered carefully in a second time.