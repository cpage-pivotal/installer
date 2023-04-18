## Tanzu Sync

### Pre-Requisites

(TBD)

See also: SETUP-SECRETS.md "Pre-Requisites"


### Pre-Installation

See also: SETUP-SECRETS.md "Pre-Installation"


### Install Tanzu Sync

1. Generate Tanzu Sync configuration file
   ```console
   $ ./tanzu-sync/scripts/configure.sh
   ```
   (review, commit, and push the configuration)

2. Deploy
   ```console
   $ ./tanzu-sync/scripts/deploy.sh
   ```

### Verification

- Verify TAP packages are installed 
  - `kubectl get pkgi -n tap-install`: all should say, "Reconcile succeeded"
- review the contents of the buildservice's OCI registry to ensure that images are getting uploaded
  - Ex: should have new `latest` images at https://console.cloud.google.com/gcr/images/cf-k8s-lifecycle-tooling-klt/global/ryanjo/tap/buildservice?project=cf-k8s-lifecycle-tooling-klt

