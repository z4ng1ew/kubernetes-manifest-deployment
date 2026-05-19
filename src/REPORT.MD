

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


![alt text](<img/image copy 18.png>)




```
s@Vivo:~/kubernetes-manifest-deployment$ minikube service list
┌─────────────┬────────────────┬──────────────┬─────┐
│  NAMESPACE  │      NAME      │ TARGET PORT  │ URL │
├─────────────┼────────────────┼──────────────┼─────┤
│ default     │ kubernetes     │ No node port │     │
│ kube-system │ kube-dns       │ No node port │     │
│ kube-system │ metrics-server │ No node port │     │
└─────────────┴────────────────┴──────────────┴─────┘
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/example/microservices.yml
deployment.apps/apache created
deployment.apps/catalog created
deployment.apps/customer created
deployment.apps/order created
service/apache created
service/catalog created
service/customer created
service/order created
s@Vivo:~/kubernetes-manifest-deployment$ minikube service list
┌─────────────┬────────────────┬──────────────┬───────────────────────────┐
│  NAMESPACE  │      NAME      │ TARGET PORT  │            URL            │
├─────────────┼────────────────┼──────────────┼───────────────────────────┤
│ default     │ apache         │ 80           │ http://192.168.49.2:32359 │
│ default     │ catalog        │ 8080         │ http://192.168.49.2:31760 │
│ default     │ customer       │ 8080         │ http://192.168.49.2:30294 │
│ default     │ kubernetes     │ No node port │                           │
│ default     │ order          │ 8080         │ http://192.168.49.2:30878 │
│ kube-system │ kube-dns       │ No node port │                           │
│ kube-system │ metrics-server │ No node port │                           │
└─────────────┴────────────────┴──────────────┴───────────────────────────┘
s@Vivo:~/kubernetes-manifest-deployment$ 
```


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

![alt text](<img/image copy 17.png>)

![alt text](<img/image copy 19.png>)

```
Workload Status
Running: 4
Deployments
Running: 4
Pods
Running: 4
Replica Sets
Deployments
Name
Images
Labels
Pods
Created
apache
minivozhd/example-apache:latest
run: apache
1 / 1
20 hours ago
catalog
minivozhd/example-catalog:latest
run: catalog
1 / 1
20 hours ago
customer
minivozhd/example-customers:latest
run: customer
1 / 1
20 hours ago
order
minivozhd/example-order:latest
run: order
1 / 1
20 hours ago
Pods
Name
Images
Labels
Node
Status
Restarts
CPU Usage (cores)
Memory Usage (bytes)
Created
apache-7f5f9d6b55-vhxqg
minivozhd/example-apache:latest
pod-template-hash: 7f5f9d6b55
run: apache
minikube
Running
2
1.00m
27.03Mi
20 hours ago
catalog-56d5b6f446-lhg7q
minivozhd/example-catalog:latest
pod-template-hash: 56d5b6f446
run: catalog
minikube
Running
2
2.00m
740.20Mi
20 hours ago
customer-99d48549d-fjqcc
minivozhd/example-customers:latest
pod-template-hash: 99d48549d
run: customer
minikube
Running
2
2.00m
669.83Mi
20 hours ago
order-779f66c67f-dtpzb
minivozhd/example-order:latest
pod-template-hash: 779f66c67f
run: order
minikube
Running
2
2.00m
642.32Mi
20 hours ago
Replica Sets
Name
Images
Labels
Pods
Created
apache-7f5f9d6b55
minivozhd/example-apache:latest
pod-template-hash: 7f5f9d6b55
run: apache
1 / 1
20 hours ago
catalog-56d5b6f446
minivozhd/example-catalog:latest
pod-template-hash: 56d5b6f446
run: catalog
1 / 1
20 hours ago
customer-99d48549d
minivozhd/example-customers:latest
pod-template-hash: 99d48549d
run: customer
1 / 1
20 hours ago
order-779f66c67f
minivozhd/example-order:latest
pod-template-hash: 779f66c67f
run: order
1 / 1
20 hours ago 
```


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



**Главная страница приложения**


![alt text](<img/image copy 14.png>)


**Браузер с apache — скриншот страницы**


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












![alt text](<img/image copy 16.png>)



```
(base) s@fedora:~/DO_H_10$ minikube service catalog
┌───────────┬─────────┬─────────────┬───────────────────────────┐
│ NAMESPACE │  NAME   │ TARGET PORT │            URL            │
├───────────┼─────────┼─────────────┼───────────────────────────┤
│ default   │ catalog │ 8080        │ http://192.168.49.2:31142 │
└───────────┴─────────┴─────────────┴───────────────────────────┘
🎉  Opening service default/catalog in default browser...
(base) s@fedora:~/DO_H_10$ 
```












![alt text](<img/image copy 20.png>)



```
s@Vivo:~/kubernetes-manifest-deployment$ minikube service customer
┌───────────┬──────────┬─────────────┬───────────────────────────┐
│ NAMESPACE │   NAME   │ TARGET PORT │            URL            │
├───────────┼──────────┼─────────────┼───────────────────────────┤
│ default   │ customer │ 8080        │ http://192.168.49.2:30294 │
└───────────┴──────────┴─────────────┴───────────────────────────┘
🎉  Opening service default/customer in default browser...
s@Vivo:~/kubernetes-manifest-deployment$ Окно или вкладка откроются в текущем сеансе браузера.
```




![alt text](<img/image copy 21.png>)


```
{
  "_links": {
    "customer": {
      "href": "http://192.168.49.2:30294/customer{?page,size,sort}",
      "templated": true
    },
    "profile": {
      "href": "http://192.168.49.2:30294/profile"
    }
  }
}
```







![http://192.168.49.2:30878/](<img/image copy 22.png>)



```
Home List
Order : View all
ID	Customer	Total Price	
No orders
Add Order
```




![alt text](<img/image copy 23.png>)


```
s@Vivo:~/kubernetes-manifest-deployment$ minikube service order
┌───────────┬───────┬─────────────┬───────────────────────────┐
│ NAMESPACE │ NAME  │ TARGET PORT │            URL            │
├───────────┼───────┼─────────────┼───────────────────────────┤
│ default   │ order │ 8080        │ http://192.168.49.2:30878 │
└───────────┴───────┴─────────────┴───────────────────────────┘
🎉  Opening service default/order in default browser...
s@Vivo:~/kubernetes-manifest-deployment$ Окно или вкладка откроются в текущем сеансе браузера.


```


















## 5. Удостоверился в работоспособности развернутого приложения, открыв в браузере страницу приложения (сервис apache).





- URL http://192.168.49.2:31726 — IP minikube + NodePort, который выдала команда minikube service apache

- Заголовок «Order Processing» — это страница приложения apache

- Содержимое работает — ссылки на Customer, Catalog, Order с описаниями (List/add/remove customers, items, Search Items, Create an order)





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



**Главная страница приложения**


![alt text](<img/image copy 14.png>)


**Браузер с apache — скриншот страницы**


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



























## Part 2. Написание собственного манифеста


## 1. Написал собственные yml-файлы манифестов для приложения из первого проекта (`/src/services`), реализующие следующее:

##   1.1 карту конфигурации со значениями хостов БД и сервисов,






![alt text](<img/image copy 29.png>)


```
s@Vivo:~/kubernetes-manifest-deployment$ cat src/k8s/01-configmap.yaml
# ConfigMap — несекретные настройки.
# Хосты внутри Kubernetes — это имена Service-объектов (внутренний DNS).
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  # PostgreSQL
  POSTGRES_HOST: postgres
  POSTGRES_PORT: "5432"

  # RabbitMQ
  RABBIT_MQ_HOST: rabbitmq
  RABBIT_MQ_PORT: "5672"
  RABBIT_MQ_QUEUE_NAME: booking_statistics_queue
  RABBIT_MQ_EXCHANGE: booking_statistics_exchange

  # Хосты сервисов = имена Service-объектов
  SESSION_SERVICE_HOST: session-service
  SESSION_SERVICE_PORT: "8081"
  HOTEL_SERVICE_HOST: hotel-service
  HOTEL_SERVICE_PORT: "8082"
  BOOKING_SERVICE_HOST: booking-service
  BOOKING_SERVICE_PORT: "8083"
  PAYMENT_SERVICE_HOST: payment-service
  PAYMENT_SERVICE_PORT: "8084"
  LOYALTY_SERVICE_HOST: loyalty-service
  LOYALTY_SERVICE_PORT: "8085"
  REPORT_SERVICE_HOST: report-service
  REPORT_SERVICE_PORT: "8086"
```





![alt text](<img/image copy 30.png>)

```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/k8s/01-configmap.yaml
configmap/app-config unchanged
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get configmap app-config
NAME         DATA   AGE
app-config   18     25m
```

**Доказательство что ConfigMap создан**


![alt text](<img/image copy 31.png>)

![alt text](<img/image copy 32.png>)

**видны все ключи и значения**

**все ключи (POSTGRES_HOST, RABBIT_MQ_HOST, имена сервисов)**

```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl describe configmap app-config
Name:         app-config
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
BOOKING_SERVICE_HOST:
----
booking-service

BOOKING_SERVICE_PORT:
----
8083

HOTEL_SERVICE_HOST:
----
hotel-service

HOTEL_SERVICE_PORT:
----
8082

LOYALTY_SERVICE_HOST:
----
loyalty-service

LOYALTY_SERVICE_PORT:
----
8085

PAYMENT_SERVICE_HOST:
----
payment-service

PAYMENT_SERVICE_PORT:
----
8084

POSTGRES_HOST:
----
postgres

POSTGRES_PORT:
----
5432

RABBIT_MQ_EXCHANGE:
----
booking_statistics_exchange

RABBIT_MQ_HOST:
----
rabbitmq

RABBIT_MQ_PORT:
----
5672

RABBIT_MQ_QUEUE_NAME:
----
booking_statistics_queue

REPORT_SERVICE_HOST:
----
report-service

REPORT_SERVICE_PORT:
----
8086

SESSION_SERVICE_HOST:
----
session-service

SESSION_SERVICE_PORT:
----
8081


BinaryData
====

Events:  <none>
```









##   1.2 Секреты с паролем и логином к БД и ключами межсервисной авторизации (их можно найти в файлах `application.properties`),




![alt text](<img/image copy 33.png>)

**Содержимое файла - 02-secrets.yaml**



```
s@Vivo:~/kubernetes-manifest-deployment$ cat src/k8s/02-secrets.yaml
# Secret — секреты (пароли, ключи). Значения в base64 (echo -n 'значение' | base64).
# Декодирование: kubectl get secret app-secrets -o jsonpath='{.data.POSTGRES_PASSWORD}' | base64 --decode
apiVersion: v1
kind: Secret
metadata:
  name: app-secrets
type: Opaque
data:
  # postgres — пароль вшит в твой Dockerfile postgres-образа
  POSTGRES_USER: cG9zdGdyZXM=
  # postgres
  POSTGRES_PASSWORD: cG9zdGdyZXM=
  # guest — стандартный пользователь rabbitmq
  RABBIT_MQ_USER: Z3Vlc3Q=
  # guest
  RABBIT_MQ_PASSWORD: Z3Vlc3Q=
  # b51ceda2-bfa5-11eb-8529-0242ac130003 — UUID gateway-сервиса (межсервисная авторизация)
  GATEWAY_SERVICE_UUID: YjUxY2VkYTItYmZhNS0xMWViLTg1MjktMDI0MmFjMTMwMDAz
  # 911ccb4c-c055-11eb-8529-0242ac130003 — UUID booking-сервиса
  BOOKING_SERVICE_UUID: OTExY2NiNGMtYzA1NS0xMWViLTg1MjktMDI0MmFjMTMwMDAz
```



![alt text](<img/image copy 34.png>)

**таблица: TYPE=Opaque, DATA=6**

**Проверка**

**список ключей (значения помечены <bytes>)**

**значения скрыты — это нормально для Secret**











































##   1.3 Поды и сервисы для всех модулей приложения: postgres, rabbitmq и 7 сервисов приложения. Для всех сервисов нужно использовать единственную реплику.








![alt text](<img/image copy 39.png>)




```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get pods -o wide
NAME                               READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
booking-service-6db4557dd9-fvm5w   1/1     Running   0          4h36m   10.244.0.7    minikube   <none>           <none>
gateway-service-85788889bc-ns8ht   1/1     Running   0          4h36m   10.244.0.11   minikube   <none>           <none>
hotel-service-76cd4f799-8q5bs      1/1     Running   0          4h36m   10.244.0.6    minikube   <none>           <none>
loyalty-service-5bcdbd4594-7slp4   1/1     Running   0          4h36m   10.244.0.9    minikube   <none>           <none>
payment-service-74b69df5df-tncgd   1/1     Running   0          4h36m   10.244.0.8    minikube   <none>           <none>
postgres-54b5c94c4c-ts52f          1/1     Running   0          4h38m   10.244.0.3    minikube   <none>           <none>
rabbitmq-746df59b5c-xjg26          1/1     Running   0          4h38m   10.244.0.4    minikube   <none>           <none>
report-service-684989fdcf-7hjbx    1/1     Running   0          4h36m   10.244.0.10   minikube   <none>           <none>
session-service-5cfcc4db9-448hm    1/1     Running   0          4h36m   10.244.0.5    minikube   <none>           <none>
```







![alt text](<img/image copy 36.png>)




```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get deployments
NAME              READY   UP-TO-DATE   AVAILABLE   AGE
booking-service   1/1     1            1           4h36m
gateway-service   1/1     1            1           4h36m
hotel-service     1/1     1            1           4h36m
loyalty-service   1/1     1            1           4h36m
payment-service   1/1     1            1           4h36m
postgres          1/1     1            1           4h38m
rabbitmq          1/1     1            1           4h38m
report-service    1/1     1            1           4h36m
session-service   1/1     1            1           4h36m
```









![alt text](<img/image copy 37.png>)








```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get services
NAME              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)              AGE
booking-service   ClusterIP   10.98.24.93      <none>        8083/TCP             4h36m
gateway-service   NodePort    10.105.174.71    <none>        8087:32003/TCP       4h36m
hotel-service     ClusterIP   10.98.139.148    <none>        8082/TCP             4h36m
kubernetes        ClusterIP   10.96.0.1        <none>        443/TCP              5h12m
loyalty-service   ClusterIP   10.101.26.223    <none>        8085/TCP             4h36m
payment-service   ClusterIP   10.97.72.30      <none>        8084/TCP             4h36m
postgres          ClusterIP   10.98.27.86      <none>        5432/TCP             4h38m
rabbitmq          ClusterIP   10.98.207.200    <none>        5672/TCP,15672/TCP   4h38m
report-service    ClusterIP   10.107.193.152   <none>        8086/TCP             4h36m
session-service   NodePort    10.100.177.26    <none>        8081:32562/TCP       4h37m
```
























![alt text](<img/image copy 38.png>)


**Все 9 строк со значением REPLICAS=1**






```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get deployments -o custom-columns=NAME:.metadata.name,REPLICAS:.spec.replicas
NAME              REPLICAS
booking-service   1
gateway-service   1
hotel-service     1
loyalty-service   1
payment-service   1
postgres          1
rabbitmq          1
report-service    1
session-service   1
```

































## 2. Запустить приложение путем последовательного применения манифестов командой `kubectl apply -f <манифест>.yaml`.











![alt text](<img/image copy 26.png>)


```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/k8s/05-session-service.yaml
deployment.apps/session-service created
service/session-service created
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/k8s/06-hotel-service.yaml
deployment.apps/hotel-service created
service/hotel-service created
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/k8s/07-booking-service.yaml
deployment.apps/booking-service created
service/booking-service created
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/k8s/08-payment-service.yaml
deployment.apps/payment-service created
service/payment-service created
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/k8s/09-loyalty-service.yaml
deployment.apps/loyalty-service created
service/loyalty-service created
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/k8s/10-report-service.yaml
deployment.apps/report-service created
service/report-service created
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/k8s/11-gateway-service.yaml
deployment.apps/gateway-service created
service/gateway-service created
s@Vivo:~/kubernetes-manifest-deployment$ 
```














## 3. Проверить статус созданных объектов (секреты, конфигурационная карта, поды и сервисы) в кластере с помощью команд `kubectl get <тип_объекта> <имя_объекта>` и `kubectl describe <тип_объекта> <имя_объекта>`. Результат отобразить в отчете.








![alt text](<img/image copy 40.png>)




```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/booking-service-6db4557dd9-fvm5w   1/1     Running   0          21h
pod/gateway-service-85788889bc-ns8ht   1/1     Running   0          21h
pod/hotel-service-76cd4f799-8q5bs      1/1     Running   0          21h
pod/loyalty-service-5bcdbd4594-7slp4   1/1     Running   0          21h
pod/payment-service-74b69df5df-tncgd   1/1     Running   0          21h
pod/postgres-54b5c94c4c-ts52f          1/1     Running   0          21h
pod/rabbitmq-746df59b5c-xjg26          1/1     Running   0          21h
pod/report-service-684989fdcf-7hjbx    1/1     Running   0          21h
pod/session-service-5cfcc4db9-448hm    1/1     Running   0          21h

NAME                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)              AGE
service/booking-service   ClusterIP   10.98.24.93      <none>        8083/TCP             21h
service/gateway-service   NodePort    10.105.174.71    <none>        8087:32003/TCP       21h
service/hotel-service     ClusterIP   10.98.139.148    <none>        8082/TCP             21h
service/kubernetes        ClusterIP   10.96.0.1        <none>        443/TCP              22h
service/loyalty-service   ClusterIP   10.101.26.223    <none>        8085/TCP             21h
service/payment-service   ClusterIP   10.97.72.30      <none>        8084/TCP             21h
service/postgres          ClusterIP   10.98.27.86      <none>        5432/TCP             21h
service/rabbitmq          ClusterIP   10.98.207.200    <none>        5672/TCP,15672/TCP   21h
service/report-service    ClusterIP   10.107.193.152   <none>        8086/TCP             21h
service/session-service   NodePort    10.100.177.26    <none>        8081:32562/TCP       21h

NAME                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/booking-service   1/1     1            1           21h
deployment.apps/gateway-service   1/1     1            1           21h
deployment.apps/hotel-service     1/1     1            1           21h
deployment.apps/loyalty-service   1/1     1            1           21h
deployment.apps/payment-service   1/1     1            1           21h
deployment.apps/postgres          1/1     1            1           21h
deployment.apps/rabbitmq          1/1     1            1           21h
deployment.apps/report-service    1/1     1            1           21h
deployment.apps/session-service   1/1     1            1           21h

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/booking-service-6db4557dd9   1         1         1       21h
replicaset.apps/gateway-service-85788889bc   1         1         1       21h
replicaset.apps/hotel-service-76cd4f799      1         1         1       21h
replicaset.apps/loyalty-service-5bcdbd4594   1         1         1       21h
replicaset.apps/payment-service-74b69df5df   1         1         1       21h
replicaset.apps/postgres-54b5c94c4c          1         1         1       21h
replicaset.apps/rabbitmq-746df59b5c          1         1         1       21h
replicaset.apps/report-service-684989fdcf    1         1         1       21h
replicaset.apps/session-service-5cfcc4db9    1         1         1       21h
s@Vivo:~/kubernetes-manifest-deployment$ 
```










![alt text](<img/image copy 42.png>)




```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl describe pod booking-service-6db4557dd9-fvm5w
Name:             booking-service-6db4557dd9-fvm5w
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Mon, 18 May 2026 13:23:00 +0300
Labels:           app=booking-service
                  pod-template-hash=6db4557dd9
Annotations:      <none>
Status:           Running
IP:               10.244.0.7
IPs:
  IP:           10.244.0.7
Controlled By:  ReplicaSet/booking-service-6db4557dd9
Containers:
  booking-service:
    Container ID:   docker://fe20c44364d446dd03510dcd29bb5ae318faea9627f18ba3c7a864d180fd07ea
    Image:          src-booking-service:latest
    Image ID:       docker://sha256:ef69d350f34904a0ee58db241b2bba79359001a9818c0b9c3a0731b6d9564eb7
    Port:           8083/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Mon, 18 May 2026 13:23:00 +0300
    Ready:          True
    Restart Count:  0
    Environment:
      POSTGRES_HOST:         <set to the key 'POSTGRES_HOST' of config map 'app-config'>  Optional: false
      POSTGRES_PORT:         <set to the key 'POSTGRES_PORT' of config map 'app-config'>  Optional: false
      POSTGRES_DB:           reservations_db
      POSTGRES_USER:         <set to the key 'POSTGRES_USER' in secret 'app-secrets'>            Optional: false
      POSTGRES_PASSWORD:     <set to the key 'POSTGRES_PASSWORD' in secret 'app-secrets'>        Optional: false
      RABBIT_MQ_HOST:        <set to the key 'RABBIT_MQ_HOST' of config map 'app-config'>        Optional: false
      RABBIT_MQ_PORT:        <set to the key 'RABBIT_MQ_PORT' of config map 'app-config'>        Optional: false
      RABBIT_MQ_QUEUE_NAME:  <set to the key 'RABBIT_MQ_QUEUE_NAME' of config map 'app-config'>  Optional: false
      RABBIT_MQ_EXCHANGE:    <set to the key 'RABBIT_MQ_EXCHANGE' of config map 'app-config'>    Optional: false
      RABBIT_MQ_USER:        <set to the key 'RABBIT_MQ_USER' in secret 'app-secrets'>           Optional: false
      RABBIT_MQ_PASSWORD:    <set to the key 'RABBIT_MQ_PASSWORD' in secret 'app-secrets'>       Optional: false
      HOTEL_SERVICE_HOST:    <set to the key 'HOTEL_SERVICE_HOST' of config map 'app-config'>    Optional: false
      HOTEL_SERVICE_PORT:    <set to the key 'HOTEL_SERVICE_PORT' of config map 'app-config'>    Optional: false
      PAYMENT_SERVICE_HOST:  <set to the key 'PAYMENT_SERVICE_HOST' of config map 'app-config'>  Optional: false
      PAYMENT_SERVICE_PORT:  <set to the key 'PAYMENT_SERVICE_PORT' of config map 'app-config'>  Optional: false
      LOYALTY_SERVICE_HOST:  <set to the key 'LOYALTY_SERVICE_HOST' of config map 'app-config'>  Optional: false
      LOYALTY_SERVICE_PORT:  <set to the key 'LOYALTY_SERVICE_PORT' of config map 'app-config'>  Optional: false
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-v658l (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  kube-api-access-v658l:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:                      <none>
s@Vivo:~/kubernetes-manifest-deployment$ 
```







![alt text](<img/image copy 43.png>)

```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl describe deployment gateway-service
Name:                   gateway-service
Namespace:              default
CreationTimestamp:      Mon, 18 May 2026 13:23:31 +0300
Labels:                 app=gateway-service
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=gateway-service
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=gateway-service
  Containers:
   gateway-service:
    Image:      src-gateway-service:latest
    Port:       8087/TCP
    Host Port:  0/TCP
    Environment:
      SESSION_SERVICE_HOST:  <set to the key 'SESSION_SERVICE_HOST' of config map 'app-config'>  Optional: false
      SESSION_SERVICE_PORT:  <set to the key 'SESSION_SERVICE_PORT' of config map 'app-config'>  Optional: false
      HOTEL_SERVICE_HOST:    <set to the key 'HOTEL_SERVICE_HOST' of config map 'app-config'>    Optional: false
      HOTEL_SERVICE_PORT:    <set to the key 'HOTEL_SERVICE_PORT' of config map 'app-config'>    Optional: false
      BOOKING_SERVICE_HOST:  <set to the key 'BOOKING_SERVICE_HOST' of config map 'app-config'>  Optional: false
      BOOKING_SERVICE_PORT:  <set to the key 'BOOKING_SERVICE_PORT' of config map 'app-config'>  Optional: false
      LOYALTY_SERVICE_HOST:  <set to the key 'LOYALTY_SERVICE_HOST' of config map 'app-config'>  Optional: false
      LOYALTY_SERVICE_PORT:  <set to the key 'LOYALTY_SERVICE_PORT' of config map 'app-config'>  Optional: false
      REPORT_SERVICE_HOST:   <set to the key 'REPORT_SERVICE_HOST' of config map 'app-config'>   Optional: false
      REPORT_SERVICE_PORT:   <set to the key 'REPORT_SERVICE_PORT' of config map 'app-config'>   Optional: false
    Mounts:                  <none>
  Volumes:                   <none>
  Node-Selectors:            <none>
  Tolerations:               <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   gateway-service-85788889bc (1/1 replicas created)
Events:          <none>
```












![alt text](<img/image copy 44.png>)




```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl describe deployment hotel-service
Name:                   hotel-service
Namespace:              default
CreationTimestamp:      Mon, 18 May 2026 13:22:53 +0300
Labels:                 app=hotel-service
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=hotel-service
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=hotel-service
  Containers:
   hotel-service:
    Image:      src-hotel-service:latest
    Port:       8082/TCP
    Host Port:  0/TCP
    Environment:
      POSTGRES_HOST:      <set to the key 'POSTGRES_HOST' of config map 'app-config'>  Optional: false
      POSTGRES_PORT:      <set to the key 'POSTGRES_PORT' of config map 'app-config'>  Optional: false
      POSTGRES_DB:        hotels_db
      POSTGRES_USER:      <set to the key 'POSTGRES_USER' in secret 'app-secrets'>      Optional: false
      POSTGRES_PASSWORD:  <set to the key 'POSTGRES_PASSWORD' in secret 'app-secrets'>  Optional: false
    Mounts:               <none>
  Volumes:                <none>
  Node-Selectors:         <none>
  Tolerations:            <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   hotel-service-76cd4f799 (1/1 replicas created)
Events:          <none>
```












![alt text](<img/image copy 45.png>)



```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl describe deployment loyalty-service
Name:                   loyalty-service
Namespace:              default
CreationTimestamp:      Mon, 18 May 2026 13:23:17 +0300
Labels:                 app=loyalty-service
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=loyalty-service
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=loyalty-service
  Containers:
   loyalty-service:
    Image:      src-loyalty-service:latest
    Port:       8085/TCP
    Host Port:  0/TCP
    Environment:
      POSTGRES_HOST:      <set to the key 'POSTGRES_HOST' of config map 'app-config'>  Optional: false
      POSTGRES_PORT:      <set to the key 'POSTGRES_PORT' of config map 'app-config'>  Optional: false
      POSTGRES_DB:        balances_db
      POSTGRES_USER:      <set to the key 'POSTGRES_USER' in secret 'app-secrets'>      Optional: false
      POSTGRES_PASSWORD:  <set to the key 'POSTGRES_PASSWORD' in secret 'app-secrets'>  Optional: false
    Mounts:               <none>
  Volumes:                <none>
  Node-Selectors:         <none>
  Tolerations:            <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   loyalty-service-5bcdbd4594 (1/1 replicas created)
Events:          <none>
```





















![alt text](<img/image copy 46.png>)






```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl describe deployment payment-service
Name:                   payment-service
Namespace:              default
CreationTimestamp:      Mon, 18 May 2026 13:23:09 +0300
Labels:                 app=payment-service
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=payment-service
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=payment-service
  Containers:
   payment-service:
    Image:      src-payment-service:latest
    Port:       8084/TCP
    Host Port:  0/TCP
    Environment:
      POSTGRES_HOST:      <set to the key 'POSTGRES_HOST' of config map 'app-config'>  Optional: false
      POSTGRES_PORT:      <set to the key 'POSTGRES_PORT' of config map 'app-config'>  Optional: false
      POSTGRES_DB:        payments_db
      POSTGRES_USER:      <set to the key 'POSTGRES_USER' in secret 'app-secrets'>      Optional: false
      POSTGRES_PASSWORD:  <set to the key 'POSTGRES_PASSWORD' in secret 'app-secrets'>  Optional: false
    Mounts:               <none>
  Volumes:                <none>
  Node-Selectors:         <none>
  Tolerations:            <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   payment-service-74b69df5df (1/1 replicas created)
Events:          <none>
```



















![alt text](<img/image copy 47.png>)





```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl describe deployment postgres
Name:                   postgres
Namespace:              default
CreationTimestamp:      Mon, 18 May 2026 13:20:59 +0300
Labels:                 app=postgres
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=postgres
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=postgres
  Containers:
   postgres:
    Image:      src-postgres:latest
    Port:       5432/TCP
    Host Port:  0/TCP
    Environment:
      POSTGRES_USER:      <set to the key 'POSTGRES_USER' in secret 'app-secrets'>      Optional: false
      POSTGRES_PASSWORD:  <set to the key 'POSTGRES_PASSWORD' in secret 'app-secrets'>  Optional: false
      POSTGRES_DB:        postgres
    Mounts:               <none>
  Volumes:                <none>
  Node-Selectors:         <none>
  Tolerations:            <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   postgres-54b5c94c4c (1/1 replicas created)
Events:          <none>
```










![alt text](<img/image copy 48.png>)






```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl describe deployment rabbitmq
Name:                   rabbitmq
Namespace:              default
CreationTimestamp:      Mon, 18 May 2026 13:21:06 +0300
Labels:                 app=rabbitmq
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=rabbitmq
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=rabbitmq
  Containers:
   rabbitmq:
    Image:       rabbitmq:3.12-management-alpine
    Ports:       5672/TCP, 15672/TCP
    Host Ports:  0/TCP, 0/TCP
    Environment:
      RABBITMQ_DEFAULT_USER:  <set to the key 'RABBIT_MQ_USER' in secret 'app-secrets'>      Optional: false
      RABBITMQ_DEFAULT_PASS:  <set to the key 'RABBIT_MQ_PASSWORD' in secret 'app-secrets'>  Optional: false
    Mounts:                   <none>
  Volumes:                    <none>
  Node-Selectors:             <none>
  Tolerations:                <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   rabbitmq-746df59b5c (1/1 replicas created)
Events:          <none>
```
















![alt text](<img/image copy 49.png>)




```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl describe deployment report-service
Name:                   report-service
Namespace:              default
CreationTimestamp:      Mon, 18 May 2026 13:23:23 +0300
Labels:                 app=report-service
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=report-service
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=report-service
  Containers:
   report-service:
    Image:      src-report-service:latest
    Port:       8086/TCP
    Host Port:  0/TCP
    Environment:
      POSTGRES_HOST:         <set to the key 'POSTGRES_HOST' of config map 'app-config'>  Optional: false
      POSTGRES_PORT:         <set to the key 'POSTGRES_PORT' of config map 'app-config'>  Optional: false
      POSTGRES_DB:           statistics_db
      POSTGRES_USER:         <set to the key 'POSTGRES_USER' in secret 'app-secrets'>            Optional: false
      POSTGRES_PASSWORD:     <set to the key 'POSTGRES_PASSWORD' in secret 'app-secrets'>        Optional: false
      RABBIT_MQ_HOST:        <set to the key 'RABBIT_MQ_HOST' of config map 'app-config'>        Optional: false
      RABBIT_MQ_PORT:        <set to the key 'RABBIT_MQ_PORT' of config map 'app-config'>        Optional: false
      RABBIT_MQ_QUEUE_NAME:  <set to the key 'RABBIT_MQ_QUEUE_NAME' of config map 'app-config'>  Optional: false
      RABBIT_MQ_EXCHANGE:    <set to the key 'RABBIT_MQ_EXCHANGE' of config map 'app-config'>    Optional: false
      RABBIT_MQ_USER:        <set to the key 'RABBIT_MQ_USER' in secret 'app-secrets'>           Optional: false
      RABBIT_MQ_PASSWORD:    <set to the key 'RABBIT_MQ_PASSWORD' in secret 'app-secrets'>       Optional: false
    Mounts:                  <none>
  Volumes:                   <none>
  Node-Selectors:            <none>
  Tolerations:               <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   report-service-684989fdcf (1/1 replicas created)
Events:          <none>
```















![alt text](<img/image copy 50.png>)


```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl describe deployment session-service
Name:                   session-service
Namespace:              default
CreationTimestamp:      Mon, 18 May 2026 13:22:48 +0300
Labels:                 app=session-service
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=session-service
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=session-service
  Containers:
   session-service:
    Image:      src-session-service:latest
    Port:       8081/TCP
    Host Port:  0/TCP
    Environment:
      POSTGRES_HOST:      <set to the key 'POSTGRES_HOST' of config map 'app-config'>  Optional: false
      POSTGRES_PORT:      <set to the key 'POSTGRES_PORT' of config map 'app-config'>  Optional: false
      POSTGRES_DB:        users_db
      POSTGRES_USER:      <set to the key 'POSTGRES_USER' in secret 'app-secrets'>      Optional: false
      POSTGRES_PASSWORD:  <set to the key 'POSTGRES_PASSWORD' in secret 'app-secrets'>  Optional: false
    Mounts:               <none>
  Volumes:                <none>
  Node-Selectors:         <none>
  Tolerations:            <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   session-service-5cfcc4db9 (1/1 replicas created)
Events:          <none>
s@Vivo:~/kubernetes-manifest-deployment$ 
```


























## 3.2. Команды kubectl get <тип> <имя> для всех объектов







![alt text](<img/image copy 51.png>)







```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get pod booking-service-6db4557dd9-fvm5w
NAME                               READY   STATUS    RESTARTS   AGE
booking-service-6db4557dd9-fvm5w   1/1     Running   0          22h
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get pod gateway-service-85788889bc-ns8ht
NAME                               READY   STATUS    RESTARTS   AGE
gateway-service-85788889bc-ns8ht   1/1     Running   0          22h
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get pod loyalty-service-5bcdbd4594-7slp4
NAME                               READY   STATUS    RESTARTS   AGE
loyalty-service-5bcdbd4594-7slp4   1/1     Running   0          22h
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get pod payment-service-74b69df5df-tncgd
NAME                               READY   STATUS    RESTARTS   AGE
payment-service-74b69df5df-tncgd   1/1     Running   0          22h
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get pod postgres-54b5c94c4c-ts52f
NAME                        READY   STATUS    RESTARTS   AGE
postgres-54b5c94c4c-ts52f   1/1     Running   0          22h
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get pod rabbitmq-746df59b5c-xjg26
NAME                        READY   STATUS    RESTARTS   AGE
rabbitmq-746df59b5c-xjg26   1/1     Running   0          22h
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get pod report-service-684989fdcf-7hjbx
NAME                              READY   STATUS    RESTARTS   AGE
report-service-684989fdcf-7hjbx   1/1     Running   0          22h
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get pod session-service-5cfcc4db9-448hm
NAME                              READY   STATUS    RESTARTS   AGE
session-service-5cfcc4db9-448hm   1/1     Running   0          22h
s@Vivo:~/kubernetes-manifest-deployment$ 
```





























## 4. Проверить наличие правильных значений секретов, применив, например, команду `kubectl get secret my-secret -o jsonpath='{.data.password}' | base64 --decode` для декодирования секрета.

**логи**


![alt text](<img/image copy 53.png>)





**Команда дословно из задания**









![alt text](<img/image copy 52.png>)


**Один скриншот с выводом всех 3-4 декодирований — видно postgres, postgres, b51ceda2-bfa5-....**

**Команда дословно из задания**

**Дополнительно — другие ключи для полноты**

```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get secret app-secrets -o jsonpath='{.data.POSTGRES_PASSWORD}' | base64 --decode
echo  
postgres
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get secret app-secrets -o jsonpath='{.data.POSTGRES_USER}' | base64 --decode
echo
postgres
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get secret app-secrets -o jsonpath='{.data.GATEWAY_SERVICE_UUID}' | base64 --decode
echo
b51ceda2-bfa5-11eb-8529-0242ac130003
s@Vivo:~/kubernetes-manifest-deployment$ 
```















## 5. Проверить логи приложения, запущенного в кластере, командой `kubectl logs <имя_контейнера>`. Скриншот отобразить в отчете.




![alt text](<img/image copy 55.png>)

**gateway-service**

![alt text](<img/image copy 56.png>)



**booking**

![alt text](<img/image copy 57.png>)


**postgres**



















![alt text](<img/image copy 63.png>)




**hotel-service**






![alt text](<img/image copy 62.png>)

**loyalty-service**









![alt text](<img/image copy 60.png>)

**payment-service**





![alt text](<img/image copy 61.png>)




**rabbitmq**




![alt text](<img/image copy 59.png>)


**report-service**








![alt text](<img/image copy 58.png>)


**session-service**













## 6. Прокинуть туннели для доступа к gateway service и session service.

**Gateway**

![alt text](<img/image copy 64.png>)


![alt text](<img/image copy 65.png>)


```bash
$ minikube service gateway-service
┌───────────┬─────────────────┬─────────────┬───────────────────────────┐
│ NAMESPACE │      NAME       │ TARGET PORT │            URL            │
├───────────┼─────────────────┼─────────────┼───────────────────────────┤
│ default   │ gateway-service │ 8087        │ http://192.168.49.2:32003 │
└───────────┴─────────────────┴─────────────┴───────────────────────────┘
```



Браузер показывает Whitelabel Error Page от Spring Boot — это стандартная страница 
ошибки фреймворка для несуществующего пути. Сам факт получения этой страницы 
доказывает, что:
- туннель проброшен корректно
- под gateway-service запущен и отвечает  
- Spring Boot приложение работает
- сетевая цепочка NodePort → Service → Pod функционирует

Корневой путь `/` не имеет mapping, поскольку gateway маршрутизирует запросы только 
на endpoint'ы `/api/v1/...`. Полноценная проверка функциональности — в пункте 7 
(Postman-тесты).






![alt text](<img/image copy 66.png>)


![alt text](<img/image copy 67.png>)







7. Запустить функциональные тесты Postman и удостовериться в работоспособности приложения.

8. Запустить стандартную панель управления Kubernetes с помощью команды `minikube dashboard`. Отобразить в отчете следующую информацию в виде скриншотов с дашборда: текущее состояние узлов кластера, список запущенных Pod, а также другие метрики, такие как загрузка ЦП и память, логи Pod, конфигурации и секреты.

9. Обновить приложение (добавив новую зависимость в pom-файл) и пересобрать его со следующими стратегиями развертывания (замерить время переразвертывания приложения для каждого случая и отметить результаты в отчете):
   - пересоздание (recreate),
   - последовательное обновление (rolling).


























































































































![alt text](<img/image copy 25.png>)



**все 11 файлов на месте**

```
s@Vivo:~/kubernetes-manifest-deployment$ tree src/k8s/
src/k8s/
├── 01-configmap copy.yaml
├── 01-configmap.yaml
├── 02-secrets.yaml
├── 03-postgres.yaml
├── 04-rabbitmq.yaml
├── 05-session-service.yaml
├── 06-hotel-service.yaml
├── 07-booking-service.yaml
├── 08-payment-service.yaml
├── 09-loyalty-service.yaml
├── 10-report-service.yaml
└── 11-gateway-service.yaml

1 directory, 12 files
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/k8s/01-configmap.yaml
configmap/app-config created
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/k8s/02-secrets.yaml
secret/app-secrets created
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/k8s/03-postgres.yaml
deployment.apps/postgres created
service/postgres created
s@Vivo:~/kubernetes-manifest-deployment$ kubectl apply -f src/k8s/04-rabbitmq.yaml
deployment.apps/rabbitmq created
service/rabbitmq created
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
postgres-54b5c94c4c-ts52f   1/1     Running   0          19s
rabbitmq-746df59b5c-xjg26   1/1     Running   0          12s
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get pods -w
NAME                        READY   STATUS    RESTARTS   AGE
postgres-54b5c94c4c-ts52f   1/1     Running   0          25s
rabbitmq-746df59b5c-xjg26   1/1     Running   0          18s
s@Vivo:~/kubernetes-manifest-deployment$ 
```














![alt text](<img/image copy 27.png>)

```
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
booking-service-6db4557dd9-fvm5w   1/1     Running   0          2m26s
gateway-service-85788889bc-ns8ht   1/1     Running   0          115s
hotel-service-76cd4f799-8q5bs      1/1     Running   0          2m33s
loyalty-service-5bcdbd4594-7slp4   1/1     Running   0          2m9s
payment-service-74b69df5df-tncgd   1/1     Running   0          2m17s
postgres-54b5c94c4c-ts52f          1/1     Running   0          4m27s
rabbitmq-746df59b5c-xjg26          1/1     Running   0          4m20s
report-service-684989fdcf-7hjbx    1/1     Running   0          2m3s
session-service-5cfcc4db9-448hm    1/1     Running   0          2m38s
s@Vivo:~/kubernetes-manifest-deployment$ kubectl get services
NAME              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)              AGE
booking-service   ClusterIP   10.98.24.93      <none>        8083/TCP             2m32s
gateway-service   NodePort    10.105.174.71    <none>        8087:32003/TCP       2m1s
hotel-service     ClusterIP   10.98.139.148    <none>        8082/TCP             2m39s
kubernetes        ClusterIP   10.96.0.1        <none>        443/TCP              37m
loyalty-service   ClusterIP   10.101.26.223    <none>        8085/TCP             2m15s
payment-service   ClusterIP   10.97.72.30      <none>        8084/TCP             2m23s
postgres          ClusterIP   10.98.27.86      <none>        5432/TCP             4m33s
rabbitmq          ClusterIP   10.98.207.200    <none>        5672/TCP,15672/TCP   4m26s
report-service    ClusterIP   10.107.193.152   <none>        8086/TCP             2m9s
session-service   NodePort    10.100.177.26    <none>        8081:32562/TCP       2m44s
s@Vivo:~/kubernetes-manifest-deployment$ 
```

















2. Запустить приложение путем последовательного применения манифестов командой `kubectl apply -f <манифест>.yaml`.



![alt text](<img/image copy 28.png>)


**minikube dashboard. Все 9 сервисов запущены**




```
Workload Status
Running: 9
Deployments
Running: 9
Pods
Running: 9
Replica Sets
Deployments
Name
Images
Labels
Pods
Created
gateway-service
src-gateway-service:latest
app: gateway-service
1 / 1
12 minutes ago
report-service
src-report-service:latest
app: report-service
1 / 1
12 minutes ago
loyalty-service
src-loyalty-service:latest
app: loyalty-service
1 / 1
12 minutes ago
payment-service
src-payment-service:latest
app: payment-service
1 / 1
12 minutes ago
booking-service
src-booking-service:latest
app: booking-service
1 / 1
12 minutes ago
hotel-service
src-hotel-service:latest
app: hotel-service
1 / 1
13 minutes ago
session-service
src-session-service:latest
app: session-service
1 / 1
13 minutes ago
rabbitmq
rabbitmq:3.12-management-alpine
app: rabbitmq
1 / 1
14 minutes ago
postgres
src-postgres:latest
app: postgres
1 / 1
14 minutes ago
Pods
Name
Images
Labels
Node
Status
Restarts
CPU Usage (cores)
Memory Usage (bytes)
Created
gateway-service-85788889bc-ns8ht
src-gateway-service:latest
app: gateway-service
pod-template-hash: 85788889bc
minikube
Running
0
-
-
12 minutes ago
report-service-684989fdcf-7hjbx
src-report-service:latest
app: report-service
pod-template-hash: 684989fdcf
minikube
Running
0
-
-
12 minutes ago
loyalty-service-5bcdbd4594-7slp4
src-loyalty-service:latest
app: loyalty-service
pod-template-hash: 5bcdbd4594
minikube
Running
0
-
-
12 minutes ago
payment-service-74b69df5df-tncgd
src-payment-service:latest
app: payment-service
pod-template-hash: 74b69df5df
minikube
Running
0
-
-
12 minutes ago
booking-service-6db4557dd9-fvm5w
src-booking-service:latest
app: booking-service
pod-template-hash: 6db4557dd9
minikube
Running
0
-
-
12 minutes ago
hotel-service-76cd4f799-8q5bs
src-hotel-service:latest
app: hotel-service
pod-template-hash: 76cd4f799
minikube
Running
0
-
-
13 minutes ago
session-service-5cfcc4db9-448hm
src-session-service:latest
app: session-service
pod-template-hash: 5cfcc4db9
minikube
Running
0
-
-
13 minutes ago
rabbitmq-746df59b5c-xjg26
rabbitmq:3.12-management-alpine
app: rabbitmq
pod-template-hash: 746df59b5c
minikube
Running
0
-
-
14 minutes ago
postgres-54b5c94c4c-ts52f
src-postgres:latest
app: postgres
pod-template-hash: 54b5c94c4c
minikube
Running
0
-
-
14 minutes ago
Replica Sets
Name
Images
Labels
Pods
Created
gateway-service-85788889bc
src-gateway-service:latest
app: gateway-service
pod-template-hash: 85788889bc
1 / 1
12 minutes ago
report-service-684989fdcf
src-report-service:latest
app: report-service
pod-template-hash: 684989fdcf
1 / 1
12 minutes ago
loyalty-service-5bcdbd4594
src-loyalty-service:latest
app: loyalty-service
pod-template-hash: 5bcdbd4594
1 / 1
12 minutes ago
payment-service-74b69df5df
src-payment-service:latest
app: payment-service
pod-template-hash: 74b69df5df
1 / 1
12 minutes ago
booking-service-6db4557dd9
src-booking-service:latest
app: booking-service
pod-template-hash: 6db4557dd9
1 / 1
12 minutes ago
hotel-service-76cd4f799
src-hotel-service:latest
app: hotel-service
pod-template-hash: 76cd4f799
1 / 1
13 minutes ago
session-service-5cfcc4db9
src-session-service:latest
app: session-service
pod-template-hash: 5cfcc4db9
1 / 1
13 minutes ago
rabbitmq-746df59b5c
rabbitmq:3.12-management-alpine
app: rabbitmq
pod-template-hash: 746df59b5c
1 / 1
14 minutes ago
postgres-54b5c94c4c
src-postgres:latest
app: postgres
pod-template-hash: 54b5c94c4c
1 / 1
14 minutes ago
```















3. Проверить статус созданных объектов (секреты, конфигурационная карта, поды и сервисы) в кластере с помощью команд `kubectl get <тип_объекта> <имя_объекта>` и `kubectl describe <тип_объекта> <имя_объекта>`. Результат отобразить в отчете.






4. Проверить наличие правильных значений секретов, применив, например, команду `kubectl get secret my-secret -o jsonpath='{.data.password}' | base64 --decode` для декодирования секрета.






5. Проверить логи приложения, запущенного в кластере, командой `kubectl logs <имя_контейнера>`. Скриншот отобразить в отчете.






6. Прокинуть туннели для доступа к gateway service и session service.








7. Запустить функциональные тесты Postman и удостовериться в работоспособности приложения.


























8. Запустить стандартную панель управления Kubernetes с помощью команды `minikube dashboard`. Отобразить в отчете следующую информацию в виде скриншотов с дашборда: текущее состояние узлов кластера, список запущенных Pod, а также другие метрики, такие как загрузка ЦП и память, логи Pod, конфигурации и секреты.




































9. Обновить приложение (добавив новую зависимость в pom-файл) и пересобрать его со следующими стратегиями развертывания (замерить время переразвертывания приложения для каждого случая и отметить результаты в отчете):
   - пересоздание (recreate),
   - последовательное обновление (rolling).
















































![alt text](<img/image copy 24.png>)



**Загружаем все 7 сервисов + postgres + rabbitmq в minikube**

```
for img in src-postgres src-session-service src-hotel-service src-booking-service \
           src-payment-service src-loyalty-service src-report-service src-gateway-service; do
  echo "=== Загрузка $img:latest в minikube ==="
  minikube image load $img:latest
done
minikube image load rabbitmq:3.12-management-alpine
=== Загрузка src-postgres:latest в minikube ===
=== Загрузка src-session-service:latest в minikube ===
=== Загрузка src-hotel-service:latest в minikube ===
=== Загрузка src-booking-service:latest в minikube ===
=== Загрузка src-payment-service:latest в minikube ===
=== Загрузка src-loyalty-service:latest в minikube ===
=== Загрузка src-report-service:latest в minikube ===
=== Загрузка src-gateway-service:latest в minikube ===
s@Vivo:~/kubernetes-manifest-deployment$ 
```


















