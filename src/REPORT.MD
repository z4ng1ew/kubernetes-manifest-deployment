

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


##   1.2 секреты с паролем и логином к БД и ключами межсервисной авторизации (их можно найти в файлах `application.properties`),

##   1.3 поды и сервисы для всех модулей приложения: postgres, rabbitmq и 7 сервисов приложения. Для всех сервисов нужно использовать единственную реплику.




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


















