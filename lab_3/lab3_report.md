University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2024/2025  
Group: K4111c  
Author: Kudryashov Mikhail Ivanovich  
Lab: Lab3  
Date of create:   
Date of finished:   
 
# Ход работы

## 1. Развёртываем ConfigMap и ReplicaSet из Yaml-файлов

`kubectl apply -f Downloads\configMap.yaml`  
`kubectl apply -f Downloads\ReplicaSet.yaml`

## 2. Создаём свой TLS-сертификат и активируем аддон Ingress

Создаём TLS secret 
```
kubectl -n kube-system create secret tls mkcert --key key.pem --cert cert.pem
```
Конфигурариуем Ingress на его использование
```
minikube addons configure ingress
-- Enter custom cert(format is "namespace/secret"): kube-system/mkcert
✅  ingress was successfully configured
```
Включаем аддон Ingress 
```
minikube addons enable ingress
💡  ingress is an addon maintained by Kubernetes. For any concerns contact minikube on GitHub.
You can view the list of minikube maintainers at: https://github.com/kubernetes/minikube/blob/master/OWNERS
💡  After the addon is enabled, please run "minikube tunnel" and your ingress resources would be available at "127.0.0.1"
    ▪ Используется образ registry.k8s.io/ingress-nginx/controller:v1.11.2
    ▪ Используется образ registry.k8s.io/ingress-nginx/kube-webhook-certgen:v1.4.3
    ▪ Используется образ registry.k8s.io/ingress-nginx/kube-webhook-certgen:v1.4.3
🔎  Verifying ingress addon...
🌟  The 'ingress' addon is enabled
```
Проверяем, что сертификат включён
`kubectl -n ingress-nginx get deployment ingress-nginx-controller -o yaml`
![Pasted image 20241207170948](https://github.com/user-attachments/assets/5ca2b9d3-cd4c-4bee-81a9-5e97b84d73d0)

## 3. Запускаем Service и Ingress из Yaml-файлов

`kubectl apply -f Downloads\Service.yaml`  
`kubectl apply -f Downloads\Ingress.yaml`

## 4. Добавляем домен в hosts и проверяем его работу
![Pasted image 20241207184939](https://github.com/user-attachments/assets/2b30dec3-917e-47a1-b0e2-6da99d10e6d4)
![Pasted image 20241207185720](https://github.com/user-attachments/assets/55103b2b-cbd4-4f04-8862-d2e14253354d)

Сертификат присутствует
![image](https://github.com/user-attachments/assets/8dae265a-c3ba-40c4-b6c9-25b880815b2c)

## 5. Схема

![13](https://github.com/user-attachments/assets/610c601a-8e60-4f71-8b6f-e96541d55e10)


