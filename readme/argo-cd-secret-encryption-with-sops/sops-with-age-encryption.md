# Sops With Age Encryption

* Install [Age Encryption](https://github.com/FiloSottile/age)
* create key

```yaml
age-keygen  -o $HOME/.age/key.txt
```

* put env age sops for decryption necessary

```
echo 'export SOPS_AGE_KEY_FILE="$HOME/.age/key.txt"' >> ~/.zshrc
```

* get public key for test encryption

```
cat $HOME/.age/key.txt | grep "^# public key"
# public key: age12u4z3lcmaf3z92406lndsx85vv7nsz5dn58rwy4rwsqarzg9yg2ss2478s
```

* create folder for testing

```
├── kustomization.yaml
├── my-secret.env
└── secret-generator.yaml
```

* encrypt my-secret-env

```
sops -e --age age12u4z3lcmaf3z92406lndsx85vv7nsz5dn58rwy4rwsqarzg9yg2ss2478s -i my-secret.env   
```

we will see on that content now

```
MY-SECRET=ENC[AES256_GCM,data:278VEfE1,iv:EoIm74dniHvJL8wQBd+O0Vu3hM1TbOIuJHtJXDjn4ak=,tag:/fkoN24A3F4I+MWpZ/ZPBQ==,type:str]
sops_age__list_0__map_enc=-----BEGIN AGE ENCRYPTED FILE-----\nYWdlLWVuY3J5cHRpb24ub3JnL3YxCi0+IFgyNTUxOSBVOXlQRmVNRHhRVnhYNmtM\nSExDdHN4L0JlNWNSai9reVNXbEtrYzJaQ3lvClR2VWpyZ2JraUxpb3FCeEdwbkRs\nbjNYaGRRRzEzbXBHODJKdmsrVU5RK00KLS0tIEZVOXRDa0hEbDlWdi8ySDRBNWo2\nU3Z0VXNtdWpoVTVUT2hrakFKaFkxb28KIM7scpzqXmnjZdXYp4JVfTZcVXzIotHM\nElRzDZYhOM47QmqNbOm/x1hg4y3TEFnfQiPe9UNtOZNvktQcrbed8A==\n-----END AGE ENCRYPTED FILE-----\n
sops_age__list_0__map_recipient=age12u4z3lcmaf3z92406lndsx85vv7nsz5dn58rwy4rwsqarzg9yg2ss2478s
sops_lastmodified=2025-09-22T07:04:04Z
sops_mac=ENC[AES256_GCM,data:rkESPYpQIeoF5pZougqD8rK2cTADOornNhaGBkYSK+NRwztLzsET3FusT4UKbv+nAtux9wYsOb9lSkkEhyxObLbbWZ46PppasO2WkKcSOzUOR0RtDHw0dw/zojaW2KZdDN1zn9jHjkwlrwgpY9EHNBwZQ7X+Y59Wsrlf67DforQ=,iv:ZhKYjajgGiIPmL1yJhF1A21cIdVyhv81wxq9Mjr71CE=,tag:CwN/yxkEAC1oNfBoOf1bTg==,type:str]
sops_unencrypted_suffix=_unencrypted
sops_version=3.9.3

```

* kustomization.yaml

```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

generators:
  - secret-generator.yaml

```

* secret-generator.yaml

```
apiVersion: goabout.com/v1beta1
kind: SopsSecretGenerator
metadata:
  name: my-secret
envs:
  - my-secret.env
```

* build kustomize

```
kustomize build --enable-alpha-plugins --enable-exec .
```

we will see

```
apiVersion: v1
data:
  MY-SECRET: TE9WRS1V
kind: Secret
metadata:
  name: my-secret-fc4477g6tm
```
