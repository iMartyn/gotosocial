# Helm chart for gotosocial

Just your average chart for installing gotosocial on a kubernetes cluster.

There's a few things to be aware of :

- `hostname` must be set and not change, and you'll want to ensure your `ingress.hosts[].host` and `ingress.tls.hosts[]` contain the same hostname if your cluster has ingresses (and set `ingress.enabled=true`).
- `gotosocial.dbType` has smarts :
  - `ephemeral` (default, only for testing) is SQLite with no persistent storage.  Probably only useful for checking out the project, because it would break all sorts of things if you went live with this.
  - `sqlite` (not recommended because sqlite and most k8s storage options is... not good) is SQLite on a PVC.  This switches the deployment type to a `StatefulSet` so you get sensible rollouts.  You can tweak the size and storageClass in `storage`
  - `postgresql-depends` is "Install postgresql for me and link it up".  It uses the bitnami postgresql chart on pretty much default settings.  You can override things but the db username, password and database name are set to `postgresql.global.postgresql.auth.[username|password|database]`.  That should make sense to you, if it doesn't, don't change anything except the password in `postgresql.global.postgresql.auth.password`.  You also need to set `postgresql.enabled` for this configuration.
  - `postgresql-ext` is "I already have postgres, thanks, here's the detail".  That's in `extdb.username`, `extdb.hostname`, `extdb.password` and `extdb.database`.

Check out the `values.yaml` for the gory detail.
