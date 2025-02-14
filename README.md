# Reverse Proxy Proof of concept

# Installation
```
oc create -f rbac.yml
oc create -f deploy.yml
oc create -f dev-route.yml
```

# Basic Usage
- Create a secret with the namespace/name of your choice.
```
apiVersion: v1
kind: Secret
metadata:
  namespace: foo
  name: bar
stringData:
  url: 'https://api.openshift.cluster.example.com:6443'
type: Opaque
```

- For development a route will be created for the proxy at https://proxy-openshift-migration.cluster.base-domain.
- `oc get route -n openshift-migration proxy -o go-template='{{ .spec.host }}'` can be used to view the URL.
- Clusters are proxied externally via the dev route at `https://proxy-openshift-migration.apps.cluster.basedomain/namespace/name/` where `/namespace/name/` corresponds to the location of the secret containing the url
- The service is also reachable within the cluster at `https://proxy.openshift-migration.svc.cluster.local:8443`
