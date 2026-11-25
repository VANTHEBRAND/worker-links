# Production deployments

The deploy job is off by default. After the environment policy, account ID and new token are configured, set the repository Actions variable `WORKER_LINKS_DEPLOY_ENABLED` to `true` to enable deployment. Define this switch only at repository scope. Clear it to pause deployments while keeping the current Cloudflare version serving. Enable the switch only in the selected active copy.

Create the `production` environment in this repository’s **Settings → Environments**. Under **Deployment branches and tags**, choose **Selected branches and tags**, add a branch rule for `main`, and leave tag patterns empty. Configure this policy before adding credentials. Repositories whose plans do not support these environment protections should keep deployment credentials unprovisioned until an equivalent protected deployment path exists.

Within that environment, store the new Cloudflare token as the secret `WORKER_LINKS_DEPLOY_TOKEN` and the target account ID as the variable `WORKER_LINKS_ACCOUNT_ID`. Keep the new names exclusive to the environment. A same-named repository or organization secret can remain accessible to workflows outside the environment.

Authorize only one of VANTHEBRAND/worker-links and VANTHEBRAND/workerlinks. Both configure a Worker named worker-links and a route on erisa.link. Compare the actual Cloudflare account, zone, Worker and deployment history to select the active copy before adding its token. Disable the inactive copy’s deployment workflow, remove its Cloudflare tokens and leave its environment without deploy credentials. Use a dedicated token for the active account with Workers Scripts Edit, plus Workers Routes Edit restricted to the verified route zone. Scope any additional KV or zone lookup permissions to the resources needed by this deployment.

Inventory consumers of the old `CF_API_TOKEN` credential. Migrate each authorized consumer to a dedicated token. Remove old Cloudflare deployment secrets from repository scope and remove inherited organization-secret access from this repository. Revoke the replaced token at Cloudflare once its authorized consumers have switched.
