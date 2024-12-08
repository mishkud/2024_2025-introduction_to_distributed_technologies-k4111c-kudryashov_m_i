University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2024/2025  
Group: K4111c  
Author: Kudryashov Mikhail Ivanovich  
Lab: Lab4  
Date of create: 07.12.2024  
Date of finished:   

# Ход работы

## 1. Запускаем minikube в режиме двух нод и Calico

```
minikube start --nodes 2 -p lab4-node --network-plugin=cni --cni=calico
```

![Pasted image 20241207202304](https://github.com/user-attachments/assets/b6816349-63bd-4683-8cbf-e47a93bc7de9)
![Pasted image 20241207202335](https://github.com/user-attachments/assets/3f3eb1fc-294a-4d44-82db-69f2c47c0f82)
![Pasted image 20241207202437](https://github.com/user-attachments/assets/7c73d35a-0a41-4ec2-b1a9-da19cd9001fa)

## 2. Устанавливаем Calicoctl

Скачиваем calicoctl и добавляем его в PATH
```
Invoke-WebRequest -Uri "https://github.com/projectcalico/calico/releases/download/v3.29.1/calicoctl-windows-amd64.exe" -OutFile "calicoctl.exe"
```

Создаём конфигурационный файл calico.cfg.yaml со следующим содержимым:
```
apiVersion: projectcalico.org/v3
kind: CalicoAPIConfig
metadata:
spec:
  datastoreType: "kubernetes"
  kubeconfig: "C:/users/myusername/.kube/config"
```
Используем следующие параметры при вызове calicoctl
`--config=D:\Programs\Minikube\calico.cfg.yaml --allow-version-mismatch`

## 3. Настраиваем пулы

Удаляем стандартный пул

```
calicoctl delete ippools default-ipv4-ippool --config=D:\Programs\Minikube\calico.cfg.yaml --allow-version-mismatch
```
![Pasted image 20241208183707](https://github.com/user-attachments/assets/41e3efdb-01ea-435c-91e9-36632cf97b71)

Добавляем метки нодам
```
kubectl label nodes lab4-node zone=north
kubectl label nodes lab4-node-m02 zone=south
```

Создаём пулы для каждой метки
```
calicoctl create --config=D:\Programs\Minikube\calico.cfg.yaml --allow-version-mismatch -f Downloads\north-pool.yaml
```
```
calicoctl create --config=D:\Programs\Minikube\calico.cfg.yaml --allow-version-mismatch -f Downloads\south-pool.yaml
```

Проверяем, что они создались
```
calicoctl get ippool -o wide --config=D:\Programs\Minikube\calico.cfg.yaml --allow-version-mismatch
```
![Pasted image 20241208184521](https://github.com/user-attachments/assets/ea764806-5e81-4295-ad6d-c32b32f0c3e5)

## 4. Разворачиваем Deployment из 2 лабораторной, проверяем их работу и ping между подами

```
kubectl apply -f Downloads\Deployment.yaml
```
![Pasted image 20241208204239](https://github.com/user-attachments/assets/92e04d72-4912-4dc5-9714-07608d4e01de)

Пробрасываем порты и проверяем работу
```
kubectl port-forward service/lab-service 3000:3000
```
![Pasted image 20241208210947](https://github.com/user-attachments/assets/4b7ff781-44f5-489d-add3-90a8938963ca)

Пингуем с пода lab2-deployment-db684fd5c-7xwqc под lab2-deployment-db684fd5c-6ctvx
```
kubectl exec lab2-deployment-db684fd5c-7xwqc -- ping -c 4 192.168.35.71
```
![Pasted image 20241208214209](https://github.com/user-attachments/assets/bc369b30-b64b-4d89-8652-c3bca9b4d93c)

## 5. Схема

![Pasted image 20241208214857](https://github.com/user-attachments/assets/8a83c87d-7fe0-4cb8-aa5b-51f503c7da1d)

![14](https://github.com/user-attachments/assets/446d6060-f1d0-47f0-a2c8-290d8de2ac35)




