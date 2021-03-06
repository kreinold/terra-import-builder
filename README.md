# Terra Import Builder
Cloud Function for staging JSON payloads in GCS for use by Terra's pull-based import API.

## Adding a new environment
To set up a new deployment env for the import-builder, do the following in the GCP Cloud Console:
1. Create a new project in GCP to host the env
2. Enable APIs in the new project:
   * IAM
   * Cloud Function
3. Create a Service Account to run the import-builder
   * Give it the "Service Account Token Creator" IAM role
   * Generate and download a JSON key for it
4. Create a GCS bucket in the project to host import bundles
   * Grant "Storage Object Creator" permissions on the bucket to the account created in 3
   * Set a lifecycle policy on the bucket to delete files after 1 day

Then, from your terminal do:
```bash
$ vault write secret/dsde/monster/${env}/biccn/terra-import-builder/env \
    project=${the-new-project} \
    bucket=${the-new-bucket}

$ vault write secret/dsde/monster/${env}/biccn/terra-import-builer/service-account.json \
    @${path-to-the-downloaded-json-keyfile}
```

Finally, add the env name to the `VALID_ENVS` constant declared at the top of `deploy.sh`.
