

## Part 1. Использование готового манифеста



## 1. Запустить окружение Kubernetes с памятью 4 GB.






![alt text](<img/image copy 3.png>)


**Запуск minikube с 4 ГБ памяти**











![alt text](<img/image copy 4.png>)

**Проверка**





![alt text](<img/image copy 8.png>)







![alt text](<img/image copy 5.png>)

**Проверка**

































## 2. Применить манифест из директории `/src/example` к созданному окружению Kubernetes.


![alt text](<img/image copy 18.png>)





![alt text](<img/image copy 6.png>)


**Применяем готовый манифест из example**





![alt text](<img/image copy 9.png>)

**Проверяем что все поды запущены**



































## 3. Запустить стандартную панель управления Kubernetes с помощью команды `minikube dashboard`.



![alt text](<img/image copy 11.png>)

![alt text](<img/image copy 10.png>)


**minikube dashboard**

![alt text](<img/image copy 12.png>)

![alt text](<img/image copy 17.png>)

![alt text](<img/image copy 19.png>)






![alt text](<img/image copy 9.png>)

**фиксируем состояние объектов**



## 4. Прокинуть туннели для доступа к развернутым сервисам с помощью команды `minikube service`.




![alt text](<img/image copy 13.png>)





**Главная страница приложения**


![alt text](<img/image copy 14.png>)


**Браузер с apache — скриншот страницы**












![alt text](<img/image copy 16.png>)














![alt text](<img/image copy 20.png>)






![alt text](<img/image copy 21.png>)







![http://192.168.49.2:30878/](<img/image copy 22.png>)





![alt text](<img/image copy 23.png>)





















## 5. Удостоверился в работоспособности развернутого приложения, открыв в браузере страницу приложения (сервис apache).





- URL http://192.168.49.2:31726 — IP minikube + NodePort, который выдала команда minikube service apache

- Заголовок «Order Processing» — это страница приложения apache

- Содержимое работает — ссылки на Customer, Catalog, Order с описаниями (List/add/remove customers, items, Search Items, Create an order)





![alt text](<img/image copy 13.png>)




**Главная страница приложения**


![alt text](<img/image copy 14.png>)


**Браузер с apache — скриншот страницы**




























## Part 2. Написание собственного манифеста


## 1. Написал собственные yml-файлы манифестов для приложения из первого проекта (`/src/services`), реализующие следующее:

##   1.1 карта конфигурации со значениями хостов БД и сервисов,






![alt text](<img/image copy 29.png>)






![alt text](<img/image copy 30.png>)



**Доказательство что ConfigMap создан**


![alt text](<img/image copy 31.png>)

![alt text](<img/image copy 32.png>)

**видны все ключи и значения**

**все ключи (POSTGRES_HOST, RABBIT_MQ_HOST, имена сервисов)**








##   1.2 Секреты с паролем и логином к БД и ключами межсервисной авторизации (их можно найти в файлах `application.properties`),




![alt text](<img/image copy 33.png>)

**Содержимое файла - 02-secrets.yaml**





![alt text](<img/image copy 34.png>)

**таблица: TYPE=Opaque, DATA=6**

**Проверка**

**список ключей (значения помечены <bytes>)**

**значения скрыты — это нормально для Secret**

































![alt text](<img/image copy 25.png>)



**все 11 файлов на месте**
















##   1.3 Поды и сервисы для всех модулей приложения: postgres, rabbitmq и 7 сервисов приложения. Для всех сервисов нужно использовать единственную реплику.








![alt text](<img/image copy 39.png>)







![alt text](<img/image copy 36.png>)










![alt text](<img/image copy 37.png>)























![alt text](<img/image copy 38.png>)


**Все 9 строк со значением REPLICAS=1**


































## 2. Запустить приложение путем последовательного применения манифестов командой `kubectl apply -f <манифест>.yaml`.











![alt text](<img/image copy 26.png>)











![alt text](<img/image copy 27.png>)















![alt text](<img/image copy 28.png>)


**minikube dashboard. Все 9 сервисов запущены**















![alt text](<img/image copy 24.png>)



**Загружаем все 7 сервисов + postgres + rabbitmq в minikube**













## 3. Проверить статус созданных объектов (секреты, конфигурационная карта, поды и сервисы) в кластере с помощью команд `kubectl get <тип_объекта> <имя_объекта>` и `kubectl describe <тип_объекта> <имя_объекта>`. Результат отобразить в отчете.








![alt text](<img/image copy 40.png>)













![alt text](<img/image copy 42.png>)







![alt text](<img/image copy 43.png>)













![alt text](<img/image copy 44.png>)












![alt text](<img/image copy 45.png>)
























![alt text](<img/image copy 46.png>)





















![alt text](<img/image copy 47.png>)











![alt text](<img/image copy 48.png>)

 **rabbitmq**
















![alt text](<img/image copy 49.png>)














![alt text](<img/image copy 50.png>)
























## 3.2. Команды kubectl get <тип> <имя> для всех объектов







![alt text](<img/image copy 51.png>)


































## 4. Проверить наличие правильных значений секретов, применив, например, команду `kubectl get secret my-secret -o jsonpath='{.data.password}' | base64 --decode` для декодирования секрета.

**логи**


![alt text](<img/image copy 53.png>)





**Команда дословно из задания**









![alt text](<img/image copy 52.png>)


**Один скриншот с выводом всех 3-4 декодирований — видно postgres, postgres, b51ceda2-bfa5-....**

**Команда дословно из задания**

**Дополнительно — другие ключи для полноты**














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


**session-service**






















7. Запустить функциональные тесты Postman и удостовериться в работоспособности приложения.




![alt text](<img/image copy 68.png>)
















## 8. Запустил стандартную панель управления Kubernetes с помощью команды `minikube dashboard`.  Отобразил в отчете информацию в виде скриншотов с дашборда: 

## 8. Стандартная панель управления Kubernetes


![alt text](<img/image copy 85.png>)

Запуск:
```bash
minikube addons enable metrics-server    
minikube dashboard
```

## 8.1. Узлы кластера

![Pods](<img/image copy 69.png>)

Кластер состоит из одного узла `minikube` со статусом Ready.


![alt text](<img/image copy 87.png>)


![alt text](<img/image copy 88.png>)

![alt text](<img/image copy 89.png>)


## 8.2. Запущенные Pod

![Pods](<img/image copy 70.png>)

![alt text](<img/image copy 71.png>)

Все 9 подов в статусе Running: postgres, rabbitmq и 7 сервисов приложения.



## 8.3. Метрики CPU и Memory

![CPU/Memory](<img/image copy 72.png>)












## 8.4. Логи Pod
![Logs postgres](<img/image copy 73.png>)

![alt text](<img/image copy 74.png>)

Видно успешное выполнение init.sql и init-check.sh — все 6 БД созданы.

**postgres**


![alt text](<img/image copy 75.png>)

**gateway-service**



![alt text](<img/image copy 78.png>)

**report-service**



![alt text](<img/image copy 79.png>)

**booking-service**


![alt text](<img/image copy 80.png>)

**hotel-service**



![alt text](<img/image copy 81.png>)

**loyalty-service**


![alt text](<img/image copy 82.png>)

**payment-service**



![alt text](<img/image copy 83.png>)

**rabbitmq**



![alt text](<img/image copy 84.png>)

**session-service**
































## 8.5. ConfigMap



![ConfigMap](<img/image copy 76.png>)

Все 15 ключей карты конфигурации.






## 8.6. Secret

![Secret](<img/image copy 86.png>)

Secret `app-secrets` содержит 6 секретных ключей:
- `POSTGRES_USER` / `POSTGRES_PASSWORD` — учётные данные для PostgreSQL
- `RABBIT_MQ_USER` / `RABBIT_MQ_PASSWORD` — учётные данные для RabbitMQ
- `GATEWAY_SERVICE_UUID` / `BOOKING_SERVICE_UUID` — ключи межсервисной авторизации (UUID — Universally Unique Identifier, уникальный идентификатор)

По умолчанию dashboard скрывает значения секретных полей (отображается иконка глаза 👁). 
Нажатие на иконку показывает расшифрованное значение — это та же операция, что и 
`base64 --decode`, выполняемая на стороне браузера. На скриншоте значения показаны 
для верификации содержимого.







































## 9. Обновил приложение (добавив новую зависимость в pom-файл) 

![alt text](<img/image copy 90.png>)

- Apache Commons Lang3 — это библиотека Java с утилитами для строк, чисел, дат. Никакого кода для её использования писать не нужно — добавление в pom.xml уже считается «новой зависимостью» (требование задания).

- Не конфликтует со Spring Boot 2.3.3.

- Лёгкая (~500 КБ).




## Пересобрал его со следующими стратегиями развертывания (замерил время переразвертывания приложения для каждого случая и отметил результаты в отчете):

![alt text](<img/image copy 91.png>)

**успешная сборка за 2m41,650s**


![alt text](<img/image copy 92.png>)

![alt text](<img/image copy 93.png>)

**Загрузка v2 в minikube**

Результат:

 1) src-loyalty-service:v2 создан
 2) Загружен в minikube (видны обе версии: latest и v2)
 3) Размер вырос с 198 → 199 МБ (+1 МБ — это добавленный commons-lang3.jar ~580 КБ + накладные)
 4) Время сборки в 2:41 включает в основном dependency:go-offline (132 сек качал зависимости первый раз) и сборку (9 сек) — это говорит о том, что Maven кэш у тебя теперь прогрет











## Замер стратегий



Терминал 1 — «watch» (наблюдатель)

![alt text](<img/image copy 94.png>)



Терминал 2 — «replay» (для повторных запусков с разной стратегией)

**<просто открываем терминал>**


Терминал 3 — «metrics» (опционально, для дополнения) -  мониторинг ресурсов:

![alt text](<img/image copy 95.png>)





## Замер 1













## Замер 1: Recreate

### Шаг 1.1. Применяем манифест с Recreate стратегией (пока образ `:latest`)


В **терминале 2**:

![alt text](<img/image copy 96.png>)




В **терминале 2**:

![alt text](<img/image copy 97.png>)






**В терминале 1 (watch) ВИДИМ как старый под умирает и появляется новый. Подожди пока новый под станет 1/1 Running.**

**Терминал 1 (watch): вся история событий — видно последовательность Terminating → Pending → Running**

![alt text](<img/image copy 98.png>)






```
loyalty-service-5bcdbd4594-7slp4   Running       ← старый под
loyalty-service-5bcdbd4594-7slp4   Terminating   ← старый начал умирать
loyalty-service-5bcdbd4594-7slp4   Error         ← старый умер
loyalty-service-68fc78958f-v8g96   Pending       ← НОВЫЙ только теперь создаётся
loyalty-service-68fc78958f-v8g96   ContainerCreating
loyalty-service-68fc78958f-v8g96   Running       ← новый запустился
```




Это наглядное доказательство что стратегия Recreate работает по принципу «убей старого → создай нового».


> Терминал 1 (watch): вся история событий — старый под (5bcdbd4594-7slp4) переходит в Terminating, и только после его завершения создаётся новый (68fc78958f-v8g96). Это поведение Recreate — есть кратковременный downtime между этими событиями.































## Замер 2: Теперь Rolling — две операции подряд


![alt text](<img/image copy 99.png>)

**В терминале 2:**

Меняет стратегию Recreate → RollingUpdate

Откатывает образ обратно :v2 → :latest (потому что в манифесте написано latest)






![alt text](<img/image copy 101.png>)

**Терминал 1: Ctrl+C, потом:**















![alt text](<img/image copy 103.png>)

**В терминале 2**


![alt text](<img/image copy 104.png>)

**Терминал 1**


 - Rolling отработал — и watch показывает Идеальную картину


Что произошло — разбор по циклам

В первом watch (apply -f rolling.yaml) — переход с Recreate+v2 на Rolling+latest:


```
loyalty-service-68fc78958f-v8g96   1/1   Running     0   54m    ← старый под (v2)
loyalty-service-5bcdbd4594-xmd5p   0/1   Pending     0   0s     ← НОВЫЙ создан РЯДОМ
loyalty-service-5bcdbd4594-xmd5p   0/1   ContainerCreating
loyalty-service-5bcdbd4594-xmd5p   1/1   Running     0   1s     ← НОВЫЙ Ready
loyalty-service-68fc78958f-v8g96   1/1   Terminating 0   65m    ← старый умирает ПОСЛЕ
```


И во втором watch (set image latest → v2) — финальный замер Rolling:


```
loyalty-service-5bcdbd4594-xmd5p   1/1   Running     0   88s    ← старый
loyalty-service-68fc78958f-9pbtr   0/1   Pending     0   0s     ← НОВЫЙ создан
loyalty-service-68fc78958f-9pbtr   0/1   ContainerCreating
loyalty-service-68fc78958f-9pbtr   1/1   Running     0   2s     ← НОВЫЙ Ready
loyalty-service-5bcdbd4594-xmd5p   1/1   Terminating 0   2m20s  ← старый умирает ПОТОМ
```



**Ключевая разница между стратегиями видна не в цифрах, а в watch-выводе**: 
- Recreate: старый под гибнет ДО создания нового → есть downtime
- RollingUpdate: новый под создаётся РЯДОМ со старым → downtime отсутствует















































## Сравнение результатов

| Стратегия | Время (`real`) | Downtime | Пик одновременных подов | Поведение |
|---|---|---|---|---|
| **Recreate** | **1.368 сек** | есть | 1 | OLD → Terminating, **затем** NEW → Pending → Running |
| **RollingUpdate** | **1.591 сек** | **нет** | 2 | NEW → Pending → Running, **затем** OLD → Terminating |

## Выводы

`kubectl rollout status` считает развёртывание завершённым, как только Pod достигает 
состояния `Running` — манифест не содержит `readinessProbe` (проба готовности — 
проверка того, что приложение реально готово принимать трафик). Поэтому абсолютные 
цифры малы и близки друг к другу: измеряется механика оркестрации Kubernetes, а не 
время инициализации Spring Boot (~20-30 сек).

**Главное отличие стратегий видно не в цифрах, а в `kubectl get pods -w`:**

- **Recreate** — старый под уходит в `Terminating` ДО создания нового. В момент 
  переключения приложение недоступно (downtime). Подходит для систем, где две версии 
  не могут сосуществовать (например, миграции БД).
  
- **RollingUpdate** — новый под создаётся РЯДОМ со старым и достигает `Running` 
  раньше, чем старый начинает умирать. Downtime отсутствует. Это стандарт для 
  продакшен-сервисов с требованием высокой доступности (SLA — Service Level 
  Agreement — соглашение об уровне обслуживания).

В минимальной конфигурации (`replicas: 1`) RollingUpdate чуть дольше Recreate из-за 
ожидания готовности нового пода параллельно со старым (1.591 сек vs 1.368 сек). 
При большем количестве реплик эта разница нивелируется преимуществом нулевого 
простоя.


---

#  Задание DO_H_10 выполнено

Все 9 пунктов закрыты: