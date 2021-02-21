# ckad-exercise-1

## 1) *pod1*
* Create a pod of image bash which runs once to execute the command `hostname > /tmp/hostname && sleep 1d`. Then run the pod to check content of file


```shell
kubectl create -f pod1.yaml
kubectl exec pod1 -- cat /tmp/hostname
```
## 2) *nginx-deployment*
* Create a new namespace k8s-challenge-2-a and assure all following operations (unless different namespace is mentioned) are done in this namespace.
* Create a deployment named nginx-deployment of three pods running image nginx with a memory limit of 64MB.
* Expose this deployment under the name nginx-service inside our cluster on port 4444, so point the service port 4444 to pod ports 80.
* Spin up a temporary pod named pod1 of image cosmintitei/bash-curl, ssh into it and request the default nginx page on port 4444 of our nginx-service using curl.
* Spin up a temporary pod named pod2 of image cosmintitei/bash-curl in namespace k8s-challenge-2-b, ssh into it and request the default nginx page on port 4444 of our nginx-service in namespace k8s-challenge-2-a using curl.


```shell
kubectl create ns k8n-challenge-2-a
kubectl config set-context --current --namespace=k8n-challenge-2-a
kubectl create -f nginx-deployment.yaml
kubectl expose deployment nginx-deployment --name=nginx-service --port=4444 --target-port=80
```
```
kubectl run -it pod1 --image=cosmintitei/bash-curl --restart=Never --rm
curl http://nginx-service:4444
```
```
kubectl create ns k8n-challenge-2-b
kubectl run -it pod2 --image=cosmintitei/bash-curl --restart=Never --namespace=k8n-challenge-2-b --rm
curl http://nginx-service.k8n-challenge-2-a:4444
```
