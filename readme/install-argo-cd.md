# Install Argo CD

1. Create File in Platform Folder

```
└── platform
    └── argocd
        ├── base
        │   ├── kustomization.yaml
        │   └── namespace.yaml
        └── overlays
            └── staging
                ├── argocd-ingress.yaml
                ├── kustomization.yaml
                ├── argocd-cmd-params-patch.yaml
```

2. platform/argocd/base/kustomization.yaml

```
resources:
  - namespace.yaml
  - https://raw.githubusercontent.com/argoproj/argo-cd/v3.1.5/manifests/install.yaml

namespace: argocd
```

3. platform/argocd/base/namespace.yaml

```
apiVersion: v1
kind: Namespace
metadata:
  name: argocd
```

4. overlays/staging/argocd-ingress.yaml

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: argocd-server-ingress
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  rules:
    - host: staging-argocd.127.0.0.1.nip.io
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: argocd-server
                port:
                  number: 80
```

5. overlays/staging/kustomization.yaml

```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
namespace: argocd

resources:
- ../../base
- argocd-ingress.yaml

patches:
- path: argocd-cmd-params-patch.yaml
```

6. overlays/staging/argocd-cmd-params-patch.yaml

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cmd-params-cm
data:
  server.insecure: 'true'
```

7. Apply

```
kustomize build platform/argocd/overlays/staging | kubectl apply -f -
```

8. Make sure each pods are running

```
kubectl get po                                                                                                  
NAME                                                READY   STATUS    RESTARTS   AGE
argocd-application-controller-0                     1/1     Running   0          8m41s
argocd-applicationset-controller-8676b8756b-wmh24   1/1     Running   0          8m45s
argocd-dex-server-68b5d79cb9-n8wfm                  1/1     Running   0          8m45s
argocd-notifications-controller-bfc494cf8-bwnps     1/1     Running   0          8m45s
argocd-redis-656fdf7c5d-5md6j                       1/1     Running   0          8m44s
argocd-repo-server-6bcdd69bbb-t9dlj                 1/1     Running   0          8m42s
argocd-server-648fc5d9df-nbtdl  
```

9. Open Argo CD

open this ingress [http://staging-argocd.127.0.0.1.nip.io/](http://staging-argocd.127.0.0.1.nip.io/)

ensure, you're ran `minikube tunnel` before open the ingress

10. Get Initial Admin Password

username using `admin`

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode;echo
```

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
