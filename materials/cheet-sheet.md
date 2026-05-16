



minikube delete
minikube start --memory=4096 --cpus=2 --driver=docker




# Применить пример (из репозитория проекта)
kubectl apply -f src/example/

# Дашборд (откроется в браузере)
minikube dashboard

# Туннели для сервисов
minikube service list
minikube service <имя_сервиса>





kubectl get nodes




minikube status




kubectl get nodes
kubectl get pods -A


