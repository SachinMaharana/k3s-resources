# K3S

## TLS in k3s with cert-manager

Assuming you have a ubuntu server(EC2,DO..etc), with IP address.

```
ssh root@207.246.79.37

apt update && apt upgrade -y

// install k3s
curl -sfL https://get.k3s.io | sh -

// verify
kubectl cluster-info
kubectl get no

// optional (run from your laptop)
docker run -it --rm --name rustscan rustscan/rustscan:1.10.0 --no-nmap -b 65535 207.246.79.37


// install cert-manager
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.1.0/cert-manager.yaml


// verify
kubectl get all -n cert-manager

// domain name pointing to server public ip
dig +short pour.sachinmaharana.com                                                       
172.67.205.240
104.21.22.169

// test cert-manager with staging
kubectl apply -f test.yaml

// verify
kubectl get certificates 
NAME          READY   SECRET        AGE
test-sachin   True    test-secret   31s

// for troubleshooting, follow the owned k8s resources
certificates > certificaterequest > orders > challenges
kubectl get certificates 
kubectl get certificatereques
... 

// delete test
kubectl delete -f test.yaml

kubectl apply -f issuer.yaml

// ingress without tls
kubectl create configmap mysite-html --from-file index.html

kubectl apply -f workload.yaml

// verify by going to pour.sachinmaharana.com


kubectl delete -f workload.yaml


kubectl apply -f workload-tls.yaml

// verify (wait till READY becomes true)
kubectl get certificates 

// verify by going to https://pour.sachinmaharana.com

:)

```