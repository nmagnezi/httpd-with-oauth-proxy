# must-gather: nginx-with-oauth-proxy

This is intended to become a part of the [must-gather-operator](https://github.com/masayag/must-gather-operator).

## Working assumptions
1. Use ServiceAccount must-gather-operator as defined [here](https://github.com/masayag/must-gather-operator/blob/master/deploy/service_account.yaml)
2. [OAuth-proxy](https://github.com/openshift/oauth-proxy) serves as a sidecar container alongside an Nginx container that exposes a downloable zipped report.
3. Users identify to [OAuth-proxy](https://github.com/openshift/oauth-proxy) before they gain access to Nginx to verify only cluster administrators access the must-gather report.
* Tested Okay with 'kubeadmin' (Access granted)
* Tested Okay witg 'developer' (403 Permission Denied)
4. Current Flow, access via: Port 443 (Route) --> Port 8443 ([OAuth-proxy container](https://github.com/openshift/oauth-proxy)) --> Port 8080 (Nginx container)
5. Missing - WIP: The [must-gather-operator](https://github.com/masayag/must-gather-operator) will expose the zipped report in a PV that will get attached to the Nginx container for download. This is currently not reflected in the .yaml file.

## How to deploy

1. Follow the must-gather-operator [README file](https://github.com/masayag/must-gather-operator/blob/master/README.md).
2. Create the objects from yaml: 

`oc create -f deploy/nginx-with-oauth-proxy.yaml`

Note: you'll need the ServiceAccount annotation [serviceaccounts.openshift.io/oauth-redirectreference.primary](https://github.com/nmagnezi/must-gather-nginx-with-oauth-proxy/blob/master/deploy/nginx-with-oauth-proxy.yaml#L10) for OAuth proxy (not currently set in must-gather-operator)
