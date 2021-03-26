
Most interesing part here is proxy that accesses database:


Enable in API:
Cloud SQL
Cloud SQL Admin

- create new cluster

- open cloud sql: create mysql instance
 -- create database : myapp
 -- create user/password


- deploy myapp - fix if sth wrong

- create servcie account in cloud-shell
  -- setup proper permission
- create key for this service account -- get permission to project and iam in service account

- create sidecar container that points to clouddb instance and secrets
- secrets are created based credentials.json usign gcloud cmd

# Check rollout status
> kubectl rollout status depployment.v1/apps/myApp...
To tell kubernetes when fail rollout
> kubectl patch deployment depployment.v1/apps/myApp... -p '{"spec":{"progressDeadlineSeconds":120}}'
> kubectl describe deployment <deployment name>
