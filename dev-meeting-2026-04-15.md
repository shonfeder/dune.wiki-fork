# Agenda

 - (none)

# Meeting notes

Attendees: Chukwuma Akunyili, @ElectreAAS, @Sudha247, @Leonidas-from-XIV, @Alizter, @shonfeder, @art-w

## Next release (Ali)

- Shon plans on releaseing 3.23
- Branching today or tomorrow
- Ali is not aware of any in-progress work that would block releasing
- Maybe we can just cut the release so if something needs to be done it can be accomodated after the fact instead of waiting for everyone's OK
- What is our release philosophy?
  * If we change it so `main` is always releasable
    + we could do patch releases if there are no breaking chnages
    + minor releases otherwise
- Process improvements are slowing down in the short-term but hopefully speed up in the long-term
- If it doesn't work we can always just re-cut the release branch
- Release ticket with deadline to announce the branch-date
- People will give us feedback on the release process so if it is bad we can improve it

## Dev tools (Shon)

- Are we developing dev-tools and dune pkg in alignment with the Platform Roadmap
- "Dune becomes the frontend for the OCaml ecosystem"
- Is it, if it does not support system-wide installation?
- What about non-project attached tools?
  * Like odig or utop
- There are different kinds of "global installation"
  * Installing stuff in `/usr/bin`
  * Using stuff outside of particular projects
- Tools are often fairly tiled to the OCaml version
  * e.g. when using utop you want a specific compiler version most likely
- Non-requirement in the spec does not mean anti-requirement
- If we want to have global installation as requirement, we need to specify it closer instead of just saying it should be supported
- Do we need to have it as requirement to make sure that it can be implemented in the future (i.e. not dig ourselves into a hole)?
- How do we determine whether it is even a requirement?
  * Should we post on discuss and ask?
  * Users when faced with a question will always say yes
  * But to determine if users need it is requirement engineering
- How exact does a specification need to be?