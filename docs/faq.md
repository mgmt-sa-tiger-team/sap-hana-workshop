# FAQ for the Provisioner
Frequently Asked Questions... or rather common problems that people have hit.

## How do I do a release PR?

Read this [guide](release.md)

## Problem: Windows workshop: MacOS breaking on a fork

```TASK [Gathering Facts] **********************************************
objc[43678]: +[__NSPlaceholderDate initialize] may have been in progress in another thread when fork() was called. We cannot safely call it or ignore it in the fork() child process. Crashing instead. Set a breakpoint on objc_initializeAfterForkError to debug.
```

### Solution:

```
$ export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES
```
