

## Part 1. Использование готового манифеста



## 1. Запустить окружение Kubernetes с памятью 4 GB.






![alt text](<img/image copy 3.png>)





```
(base) s@fedora:~/DO_H_10$ minikube start --memory=4096 --cpus=2 --driver=docker
😄  minikube v1.38.1 на Fedora 43
✨  Используется драйвер docker на основе конфига пользователя
❗  Starting v1.39.0, minikube will default to "containerd" container runtime. See #21973 for more info.
📌  Using Docker driver with root privileges
👍  Starting "minikube" primary control-plane node in "minikube" cluster
🚜  Pulling base image v0.0.50 ...
💾  Скачивается Kubernetes v1.35.1 ...
    > preloaded-images-k8s-v18-v1...:  272.45 MiB / 272.45 MiB  100.00% 7.76 Mi
    > gcr.io/k8s-minikube/kicbase...:  519.58 MiB / 519.58 MiB  100.00% 8.02 Mi
🔥  Creating docker container (CPUs=2, Memory=4096MB) ...
🐳  Подготавливается Kubernetes v1.35.1 на Docker 29.2.1 ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Компоненты Kubernetes проверяются ...
    ▪ Используется образ gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Включенные дополнения: storage-provisioner, default-storageclass
🏄  Готово! kubectl настроен для использования кластера "minikube" и "default" пространства имён по умолчанию
(base) s@fedora:~/DO_H_10$ 
```












![alt text](<img/image copy 4.png>)


(base) s@fedora:~/DO_H_10$ kubectl get nodes
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   60s   v1.35.1
(base) s@fedora:~/DO_H_10$ 


![alt text](<img/image copy 5.png>)



```
(base) s@fedora:~/DO_H_10$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

(base) s@fedora:~/DO_H_10$ 
```






2. Применить манифест из директории `/src/example` к созданному окружению Kubernetes.




![alt text](<img/image copy 6.png>)


```
(base) s@fedora:~/DO_H_10$ kubectl apply -f src/example/microservices.yml
deployment.apps/apache created
deployment.apps/catalog created
deployment.apps/customer created
deployment.apps/order created
service/apache created
service/catalog created
service/customer created
service/order created
```












## 3. Запустить стандартную панель управления Kubernetes с помощью команды `minikube dashboard`.

## 4. Прокинуть туннели для доступа к развернутым сервисам с помощью команды `minikube service`.

## 5. Удостовериться в работоспособности развернутого приложения, открыв в браузере страницу приложения (сервис apache).


kubectl get nodes

kubectl get pods -A