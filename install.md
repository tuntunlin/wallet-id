```sh

********************IWaltid Installation using Helm***************************************

kubectl delete ns waltid
kubectl create ns waltid
helm install waltid ./waltid -n waltid

kubectl delete pvc -n waltid waltid-web-wallet-data-volume-claim
kubectl get pvc -A

  cat <<EOF | kubectl apply -f -
  apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
    name: waltid-web-wallet-data-volume-claim
    namespace: waltid
  spec:
    accessModes:
      - ReadWriteOnce
    storageClassName: longhorn
    resources:
      requests:
        storage: 10Gi
  EOF

```
```sh
********************SSL Certificate Input***************************************

kubectl create secret tls waltid-wildcard-tls \
  --cert=/home/walletvm/demo/SSL_Cert/STAR_eid_gov_mm.crt \
  --key=/home/walletvm/demo/SSL_Cert/Priv.key \
  --namespace=waltid

kubectl edit ing waltid-issuer-ingress -n waltid
//input
  tls:
  - hosts:
    - issuer.eid.gov.mm
    secretName: waltid-wildcard-tls

kubectl edit ing waltid-portal-ingress -n waltid
//input
  tls:
  - hosts:
    - didportal.eid.gov.mm
    secretName: waltid-wildcard-tls
kubectl edit ingress waltid-verifier-ingress -n waltid
//input
  tls:
  - hosts:
    - verifier.eid.gov.mm
    secretName: waltid-wildcard-tls
kubectl edit ingress waltid-web-wallet-api-ingress -n waltid
//input
  tls:
  - hosts:
    - api-wallet.eid.gov.mm
    secretName: waltid-wildcard-tls
kubectl edit ingress waltid-web-wallet-ingress -n waltid
//input
  tls:
  - hosts:
    - wallet.eid.gov.mm
    - wallet-dev.eid.gov.mm
    secretName: waltid-wildcard-tls

********************SSL Certificate Input***************************************
```


