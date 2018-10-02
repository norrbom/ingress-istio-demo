### Traffic routing using Ingress Controller and Istio on minikube

#### Ingress Controller Demo
Install the ingress controller and the demo applications 'app1' and 'app2'
```
helm dep update nginx-ingress
helmfile sync
```
Add testhost.com to /etc/hosts, the two apps should be accessible now on http://testhost.com:32080/app1 and http://testhost.com:32080/app2

#### Istio Demo
Download and install istio
```
curl -L https://git.io/getLatestIstio | sh -
cd istio-1.0.2
kubectl apply -f install/kubernetes/helm/helm-service-account.yaml
helm init --service-account tiller
helm install install/kubernetes/helm/istio --name istio --namespace istio-system
```
Inject the Istio side car to sleep demo app
```
istioctl kube-inject -f sleep.yaml > sleep-sidecar.yaml
```
Deploy sleep app with the side car
```
kubectl apply -f sleep-sidecar.yaml
```
Make sure there are two containers running
```
kubectl get deployments -l 'app=sleep' -o wide
```