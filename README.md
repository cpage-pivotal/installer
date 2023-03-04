# Tanzu GitOps Reference Implementation

## Getting Started Quick

For example, to initialize configuration for a cluster named "build-01" to use SOPS for secrets management:
```
$ ./setup-repo.sh build-01 sops
```

## Overview

```
./.catalog/      # products that can be configure into one or more clusters
./clusters/      # configuration for each cluster
./setup-repo.sh  # helper script for setting up contents of ./clusters
```
