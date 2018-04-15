# Conjunction

Connecting CI pipelines together so that upstream projects can trigger builds and track results for downstream projects

## Why is this needed?

- An upstream project changes - how do downstream projects know it has changed?
- Downstream projects are failing because of an upstream change but the upstream project is oblivious
- Webhooks for GitHub require the user to be logged in and part of the project
- Something goes wrong downstream but it is not clear who to contact upstream about it
- It is hard to tell upstream what downstream projects are using your artifacts

## How does it work at high level?

<img src="./doc/images/simple-diagram.png">

Conjunction is pretty simple.

1. Upon successful pipeline completion, an upstream project sends its status to Conjunction.
2. Conjunction sends a notification to each project that is depending on the upstream project.
3. When a dependent project's build has finished, it reports back to Conjunction its status.

## What do the data structures look like?

### A trigger event looks like:

- A build event ID
- A token used for authentication with the CI system
- A project ID
- Build metadata (TBD)

### A status event looks like:

- A build event ID
- A token used for authentication with Conjunction
- A project ID
- Build metadata (TBD)
- Build result

## What are the advantages of this approach?

### Simple
- Just one server. Simple messaging system.

### Integrates across existing build systems easily
- Use webhooks / triggers and existing build configs to integrate with the system.

## What other features could be implemented?

### Badges
- For including into README files and websites.
- Indicate percentage of downstream that succeeded last build.
- Statistics showing how many improved / broke / stayed the same compared to the build before.

### Auxilliary notifications
- IRC, Slack, Twitter, custom system

### Hierarchical builds
- Project A triggers dependent project B. Project B completes and then triggers project C.
- Map the whole build tree from starting project to the last project.
- Check for repeated nodes in the build tree to stop build loops.
