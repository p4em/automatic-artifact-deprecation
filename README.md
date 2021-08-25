# [Draft] Automatic artifact deprecation

Distribution of this memo is unlimited.

## Overview

<https://github.com/p4em/artifact-deprecation> allows an artifact to be marked deprecated. In its most basic form, this relies on an active signal from the maintainer to deprecate the artifact. That means some maintainers will do it and some won't. Each deprecation is good for the Maven ecosystem, so this is still an improvement on the status quo. But what if we want to go further, and enable the deprecation of artifacts when maintainers don't get round to it?

We propose some automatic mechanisms that allow artifact deprecation when various conditions are met. These mechanisms are optional, and the Maven repository admin decides whether to enable them.

This will expand the reach of artifact deprecation, and improve the health of the Maven ecosystem at large.

## Definitions

- Deprecation: See <https://github.com/p4em/artifact-deprecation>.
- Maintainer: Anyone who can manage an artifact or deploy new versions of it. (In typical Maven repo software this would correspond to anyone with 'manage', 'deploy', or 'admin' permissions for an artifact.)

## Keep-alive mechanism

1. No artifact activity conditions like the following happen for a certain `<inactivity-period>`:
  - New versions of the artifact are deployed.
  - The artifact is otherwise managed. (Management actions might include changing its metadata.)
2. The repository pings the maintainer(s) with something like this:
  > "There has been no activity on the artifact `<foo>` for `<inactivity-period>`. Please confirm that `<foo>` is still active. If no confirmation is received before `<notice-period>`, `<foo>` will be automatically deprecated."
3. If any of the following happen during the `<notice-period>`, the inactivity clock is reset and we go back to (1):
  - Artifact activity conditions happen
  - A separate confirmation mechanism is used (e.g. click link in email)
4. Otherwise, the repository automatically marks the artifact as deprecated after `<notice-period>` ends.
5. In the future, if a new maintainer is assigned to the artifact or the existing maintainer changes their mind, they can undeprecate the artifact and we go back to (1).

`<inactivity-period>` and `<notice-period>` should be configurable values in the repository server settings. We suggest that `<notice-period>` is shorter than `<inactivity-period>`, but beyond that the values will depend on community or organization policy.
