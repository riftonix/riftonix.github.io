---
title: Deployment Paths
linkTitle: Deployment Paths
weight: 10
---

RiftOniX publishes production and preview builds under the same canonical site.

Production is rendered with:

```text
https://riftonix.io/
```

Pull request previews are rendered with:

```text
https://riftonix.io/<mr-id>/
```

Preview publishing must preserve the production root site while replacing only the matching preview path.
