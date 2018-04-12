# Conjunction

Connecting CI pipelines together so that upstream projects can trigger builds and track results for downstream projects

## Why is this needed?

- An upstream project changes - how do downstream projects know it has changed?
- Downstream projects are failing because of an upstream change but the upstream project is oblivious
- Webhooks for GitHub require the user to be logged in and part of the project
- Something goes wrong downstream but it is not clear who to contact upstream
- It is hard to tell upstream what downstream projects are using your artifacts

## How does it work at high level

<img src="./doc/images/simple-diagram.png">

Conjunction is pretty simple.

An upstream project is configured to send a trigger event to Conjunction upon successful pipeline completion.

A trigger event contains:
- A project ID
- A token used for authentication with Conjunction
- Build metadata (TBD)
- Build result
