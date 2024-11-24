University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2024/2025  
Group: K4111c  
Author: Kudryashov Mikhail Ivanovich  
Lab: Lab2  
Date of create: 24.11.2024  
Date of finished:   

# Ход работы

## 1. Запускаем Deployment и Service.

Запускаем Deployment и сервис Node-Port из yaml-файла.

```kubectl apply -f "Downloads\deployment.yaml"```

Пробрасываем порт, чтобы подключиться к подам.

```kubectl port-forward service/lab-service 3000:3000```

Смотрим в браузере.

![image](https://github.com/user-attachments/assets/5f644b5b-495d-45cf-acda-149e5e0d2b3f)


Переменные `REACT_APP_USERNAME` и `REACT_APP_COMPANY_NAME` не меняются, т.к. они заданы в yaml-файле. Container меняется, если, например, пересоздать deployment. Т.к. эта переменная берётся из названия пода.

![image](https://github.com/user-attachments/assets/e29212da-958b-462c-a0b2-75c34fd8401f)

Схема

![12](https://github.com/user-attachments/assets/ca78988b-fb2d-4fe3-8f23-f329e13debf8)
