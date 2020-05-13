# gke-sctp-tests
Tests over various GKE setups with the SCTP protocol

## Test1: Headless service over the deployment
- Based on the k8s configurations in this repository and a Docker image created from this ([Dockerfile]https://github.com/hshinde/socatdocker/blob/master/Dockerfile)

```gcloud container clusters create alpha-headless --enable-kubernetes-alpha --zone  us-central1-c --no-enable-autoupgrade --enable-network-policy --image-type=ubuntu
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f pod.yaml
kubectl exec -ti sctp-server1 -- /bin/sh
apt install dnsutils
nslookup sctp-service
socat -x -d -d -d - sctp:sctp-service.default.svc.cluster.local:11111
```
