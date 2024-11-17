University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2024/2025  
Group: K4111c  
Author: Kudryashov Mikhail Ivanovich  
Lab: Lab1  
Date of create: 17.11.2024  
Date of finished:   

# Ход работы

## 1. Установка Docker, minikube, kubectl

Скачиваем с официальных источников и устанавливаем.

![image](https://github.com/user-attachments/assets/ec75e873-f49a-4116-a60b-c40b9a29eee9)
![image](https://github.com/user-attachments/assets/c2b7ff6d-f45b-4031-b6b2-e8ebda059aec)

## 2. Запуск кластера

Устанавливаем Docker по умолчанию
```
minikube config set driver docker
```
Запускаем кластер
```
minikube start
```


## 3. Запуск пода и сервиса

Запускаем под из yaml-файла
```
kubectl apply -f "Downloads\pod.yaml"
```

![image](https://github.com/user-attachments/assets/2ab0ef3c-b76a-4ed2-b4f1-4ec3ed7837a3)


Запускаем сервис консольной командой
```
kubectl expose pod lab-vault --type=NodePort --port=8200
```

![image](https://github.com/user-attachments/assets/b4b8a09c-2df1-4be2-9090-0fe40b8f54ae)


## 4. Обращение к поду

Пробрасываем порт с пода на локальный порт ПК
```
kubectl port-forward service/lab-vault 8200:8200
```

Обращаемся к нему по ссылке
https://127.0.0.1:8200
или 
https://localhost:8200

![image](https://github.com/user-attachments/assets/38afb1ff-8e7e-4c1d-b8ac-97cb4cf87a10)

## 5. Вход по токену

Смотрим логи пода 
```
kubectl logs lab-vault
```

![image](https://github.com/user-attachments/assets/02d2568d-bbfd-4b3d-91b5-0bcd2c9f5090)

Вставляем Root Token, "авторизуемся" в поде

![image](https://github.com/user-attachments/assets/8021b542-1284-4ec1-9fcc-f4be9f20b1a0)


Можно сделать сервис доступным извне, сразу пробросив порты. Для этого запускаем minicube с опциями:
```
minikube start --extra-config=apiserver.service-node-port-range=32760-32767 --ports=127.0.0.1:32760-32767:32760-32767
```
Тогда при обращении по 127.0.0.1:<порт_сервиса> также открывается страничка Vault.
![image](https://github.com/user-attachments/assets/30a4169d-08a5-4c9e-8b33-805c98ca8555)
![image](https://github.com/user-attachments/assets/cdef28a8-c560-4d3e-92ce-cc8b891d21f4)

## 6. Схема
![image](https://github.com/user-attachments/assets/b6750aca-9b8c-4823-aa68-084ad07dde52)
![11](https://github.com/user-attachments/assets/f1f4912c-90f2-4c88-a7cc-8b9881e3a669)


