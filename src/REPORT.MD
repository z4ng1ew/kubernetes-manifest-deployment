

## Part 1. Использование готового манифеста



## 1. Запустить окружение Kubernetes с памятью 4 GB.






![alt text](<img/image copy 3.png>)


**Запуск minikube с 4 ГБ памяти**


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

**Проверка**

(base) s@fedora:~/DO_H_10$ kubectl get nodes
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   60s   v1.35.1
(base) s@fedora:~/DO_H_10$ 






![alt text](<img/image copy 8.png>)


```
(base) s@fedora:~/DO_H_10$ kubectl cluster-info
Kubernetes control plane is running at https://192.168.49.2:8443
CoreDNS is running at https://192.168.49.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
(base) s@fedora:~/DO_H_10$ 
```




![alt text](<img/image copy 5.png>)

**Проверка**

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


































## 2. Применить манифест из директории `/src/example` к созданному окружению Kubernetes.




![alt text](<img/image copy 6.png>)


**Применяем готовый манифест из example**

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





![alt text](<img/image copy 9.png>)

**Проверяем что все поды запущены**

```
(base) s@fedora:~/DO_H_10$ kubectl get pods 
NAME                       READY   STATUS    RESTARTS      AGE
apache-7f5f9d6b55-vhxqg    1/1     Running   1 (30m ago)   15h
catalog-56d5b6f446-lhg7q   1/1     Running   1 (30m ago)   15h
customer-99d48549d-fjqcc   1/1     Running   1 (30m ago)   15h
order-779f66c67f-dtpzb     1/1     Running   1 (30m ago)   15h
(base) s@fedora:~/DO_H_10$ 
```



































## 3. Запустить стандартную панель управления Kubernetes с помощью команды `minikube dashboard`.

![alt text](<img/image copy 11.png>)

![alt text](<img/image copy 10.png>)


**minikube dashboard**

![alt text](<img/image copy 12.png>)


**фиксируем состояние объектов**

```
(base) s@fedora:~/DO_H_10$ kubectl get pods
NAME                       READY   STATUS    RESTARTS       AGE
apache-7f5f9d6b55-vhxqg    1/1     Running   1 (3h4m ago)   18h
catalog-56d5b6f446-lhg7q   1/1     Running   1 (3h4m ago)   18h
customer-99d48549d-fjqcc   1/1     Running   1 (3h4m ago)   18h
order-779f66c67f-dtpzb     1/1     Running   1 (3h4m ago)   18h
(base) s@fedora:~/DO_H_10$ kubectl get services
NAME         TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
apache       LoadBalancer   10.101.79.77     <pending>     80:31726/TCP     18h
catalog      LoadBalancer   10.109.14.125    <pending>     8080:31142/TCP   18h
customer     LoadBalancer   10.108.72.96     <pending>     8080:31295/TCP   18h
kubernetes   ClusterIP      10.96.0.1        <none>        443/TCP          18h
order        LoadBalancer   10.108.101.152   <pending>     8080:30939/TCP   18h
(base) s@fedora:~/DO_H_10$ 
```





## 4. Прокинуть туннели для доступа к развернутым сервисам с помощью команды `minikube service`.




![alt text](<img/image copy 13.png>)



```
(base) s@fedora:~/DO_H_10$ minikube service apache
┌───────────┬────────┬─────────────┬───────────────────────────┐
│ NAMESPACE │  NAME  │ TARGET PORT │            URL            │
├───────────┼────────┼─────────────┼───────────────────────────┤
│ default   │ apache │ 80          │ http://192.168.49.2:31726 │
└───────────┴────────┴─────────────┴───────────────────────────┘
🎉  Opening service default/apache in default browser...
(base) s@fedora:~/DO_H_10$ 
```


![alt text](<img/image copy 14.png>)


```
Order Processing
Customer
List / add / remove customers
Catalog
List / add / remove items
Catalog
Search Items
Order
Create an order
```










































## 5. Удостовериться в работоспособности развернутого приложения, открыв в браузере страницу приложения (сервис apache).


kubectl get nodes

kubectl get pods -A