## Tanzu Sync (w/ Secrets OPerationS)

### Pre-Requisites

- SOPS CLI : https://github.com/mozilla/sops#download
- AGE CLI  : https://github.com/FiloSottile/age#installation
- a Kubernetes cluster (any IaaS)

### Setting up SOPS and Age for encryption

0. Generate age public/secret keys for use during SOPS encryption / decryption.
   ```console
   $ cd /tmp
   $ mkdir enc
   $ chmod 700 enc
   $ cd enc
   $ age-keygen -o key.txt
   $ cat key.txt

   # created: 2023-02-08T10:55:35-07:00
   # public key: age1ql3z7hjy54pw3hyww5ayyfg7zqgvc7w3j2elw8zmrj2kg5sfn9aqmcac8p
   AGE-SECRET-KEY-my-secret-key
   ```

   > NOTE: Tanzu Sync only uses "Age" for "X25519 identities". Encrypting with SSH keys via Age is not yet supported by SOPS.

1. Create a plain YAML file (named `tap-sensitive-values.yaml`) that envelopes the sensitive portion of TAP values. \
   The following is only an example.
   ```yaml
   ---
   tap_install:
     sensitive_values:
       # TAP values that are sensitive/secret go here.
       shared:
         image_registry:
           project_path: "gcr.io/my-project/tap"
           username: "_json_key"
           password: |
             {
               "type": "service_account",
               "project_id": "my-project",
               "private_key_id": "my-private-key-id",
               "private_key": "-----BEGIN PRIVATE KEY-----\n..........\n-----END PRIVATE KEY-----\n",
               ...
             }
       ...
   ```

2. Encrypt `tap-sensitive-values.yaml` with Age using SOPS:
   ```console
   $ export SOPS_AGE_RECIPIENTS=$(cat key.txt | grep "# public key: " | sed 's/# public key: //')
   $ sops --encrypt tap-sensitive-values.yaml > tap-sensitive-values.sops.yaml
   ```

3. (Optional) Verify the encrypted file can be decrypted
   ```console
   $ export SOPS_AGE_KEY_FILE=key.txt
   $ sops --decrypt tap-sensitive-values.sops.yaml
   ```
   and can be edited directly using SOPS:
   ```console
   $ sops tap-sensitive-values.sops.yaml
   ```
 
4. Move the sensitive TAP values into the cluster config
   ```console
   $ mv tap-sensitive-values.sops.yaml GIT-REPO-ROOT/clusters/CLUSTER-NAME/cluster-config/values/
   ```

5. Retain the Age identity (key file) in a safe and secure place (e.g. a password manager) and purge the scratch space:
   ```console
   $ mv key.txt SAFE-LOCATION/
   $ cd /tmp
   $ rm -rf enc
   ```

Next: go to README.md's "Install Tanzu Sync" section.

