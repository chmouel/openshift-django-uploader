## OpenShift Deployment

This uses S2I to generate an image against the repo and output it to an
OpenShift ImageStream and then use a Kubernetes `Deployment` to deploy it, with
a few sed for dynamic variables.

The deployment has two containers, the main one is nginx getting all request and
passing the uploads to the uwsgi process in the other container and serves the
static file directly.

## Install

You need first to create a a username password with :

```
htpasswd -b -c osinstall.htpasswd username password
```

Then you just use the makefile target, build and deploy you just need :

```
make build deploy
```

django `SECRET_KEY` is automatically generated.
