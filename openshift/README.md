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
htpasswd -b -c config/osinstall.htpasswd username password
```

Then you just use the makefile target, build and deploy you just need :

```
oc new-project os4install
make build deploy
```

django `SECRET_KEY` is automatically generated.

## Usage

Get the route of your deployment with :

```shell
oc get route openshift4-nightly-reinstall -o jsonpath='{.spec.host}'`
```

Test if it's working with :

```shell
route=http://$(oc get route openshift4-nightly-reinstall -o jsonpath='{.spec.host}')
echo "HELLO WORLD" > /tmp/hello.txt
curl -u username:password -F path=hello-upload.txt -X POST -F file=@/tmp/hello.txt \
    ${route}/upload
curl ${route}/hello-upload.txt
```
