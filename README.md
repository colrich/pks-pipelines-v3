
# `platform-automation`-powered Concourse Pipelines


Use these pipelines to deploy PKS via Concourse with Platform Automation

Start by building a configuration repo (see here for starter: https://github.com/colrich/pks-config-v3) and follow the steps in its readme such as importing credentials into credhub.

Once that's done, clone this repo and open vars-labverify/vars-common.yml. You'll need to edit the values in this file to match your environment
- foundation - this is a text label for the deployment, should match the directory name in your config repo (in my example it's "labverify")
- s3.endpoint - set to your blobstore - can be real s3 or s3-compatible like minio
- s3.access_key_id - for the s3 user
- s3.secret_access_key - for the s3 user
- s3.region_name - needed for real s3
- s3.buckets.platform_automation - name of a bucket you will create to hold the platform automation artifacts
- s3.buckets.foundation - name of a bucket you will create to hold exported platform artifacts
- credhub.server - url of the credhub server holding your deployment variables
- credhub.prefix - prefix you used for credhub entries for this deployment
- git.configuration.uri - url of the configuration git repo you created above
- git.user.email - email address of the git user to fetch the config repo
- git.user.username - username of the git user to fetch the config repo
- git.private_key - ssh key for git user (if using ssh authentication)

Create the buckets named above.
Grab the artifacts from pivnet - https://network.pivotal.io/products/platform-automation - get both files and put them in the bucket you named in s3.buckets.platform_automation

Set the pipeline by using the "3-fly-install-upgrade-products.sh" script. The parameters are your concourse target name, the foundation label you chose above, and the name you want to use for the pipeline. In my case, I would do "./3-fly-install-upgrade-products.sh lab labverify install-pks-labverify"

