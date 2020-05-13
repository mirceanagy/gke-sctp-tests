# gke-sctp-tests
Tests over various GKE setups with the SCTP protocol

## Test1: Headless service over the deployment
- Based on the k8s configurations in this repository and a Docker image created from this [Dockerfile](https://github.com/hshinde/socatdocker/blob/master/Dockerfile)

```gcloud container clusters create alpha-headless --enable-kubernetes-alpha --zone  us-central1-c --no-enable-autoupgrade --enable-network-policy --image-type=ubuntu
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f pod.yaml
kubectl exec -ti sctp-server1 -- /bin/sh
apt install dnsutils
nslookup sctp-service
socat -x -d -d -d - sctp:sctp-service.default.svc.cluster.local:11111
```

## Test2: Running on COS VMs
- A similar test over COS GKE VMs will fail because the COS image does not contain the SCTP kernel module
`2020/05/11 12:57:30 socat[1] E socket(2, 1, 132): Protocol not supported`
  - To run this test create the cluster like this (without the image-type from above):
```
gcloud container clusters create alpha-headless --enable-kubernetes-alpha --zone  us-central1-c --no-enable-autoupgrade --enable-network-policy
```
