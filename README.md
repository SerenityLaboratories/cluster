# cluster

This setup is missing `mastodon/config/secrets.yaml` for obvious reasons.

Values set in that file are:

* SMTP_PASSWORD
* AWS_ACCESS_KEY_ID
* AWS_SECRET_ACCESS_KEY
* SECRET_KEY
* OTP_SECRET
* VAPID_PRIVATE_KEY
* VAPID_PUBLIC_KEY

They file looks like this:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: secrets
type: Opaque
data:
  KEY: "base64 encoded value"
```

## setup

* get a rancher cluster up, for the love of god don't pick canal as your network provider
    * seriously you will regret it
* install cert-manager from helm (*not* from the included catalog, unless you enable the helm stable one)
* install kubedb from kubedb.com
* check if the rook config will work on your cluster (it should, but it may fail if you have less than 3 nodes)
* `kubectl apply -f rook/`
* `kubectl apply -f certman/`
* Modify certificates, ingresses and config to your domain
* `kubectl apply -f mastodon/common/`
* `kubectl create ns <your namespace>`
* `kubectl apply -f mastodon/config/ -n <your namespace>`
* `kubectl apply -f mastodon/instance/ -n <your namespace>`
* (wait for databases to provision)
* `kubectl apply -f mastodon/jobs/db-migrate.yaml -n <your namespace>`