HELM

# Installation

## Kubernetes setup
### create service account for helm
```
kubectl -n kube-system create serviceaccount tiller
```
### clusterRoleBinding 
```
create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
```

## helm binary 

link:
```
https://helm.sh/docs/intro/install/
```

### Step by step
#### Download your desired version

```
https://github.com/helm/helm/releases
```
#### Unpack it 
```
tar -zxvf helm-v3.0.0-linux-amd64.tar.gz)
```

#### Move it to its desired destination 
```
mv linux-amd64/helm /usr/local/bin/helm
```

## Init helm tiller
```
helm init --service-account tiller
```
### NOTEs:
if you hit x.509 unauthorized problem, see below link for solution:
```
https://stackoverflow.com/questions/21181231/server-certificate-verification-failed-cafile-etc-ssl-certs-ca-certificates-c
```
Basically, it is caused by - your host doesn't trust the target server; this can be debug through:
```
# openssl s_client -connect kubernetes-charts.storage.googleapis.com:443
# curl -v https://kubernetes-charts.storage.googleapis.com/index.yaml
```

#### solution:
```
#echo -n | openssl s_client -showcerts -connect kubernetes-charts.storage.googleapis.com:443  2>/dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' >> /etc/ssl/certs/ca-certificates.crt
```
