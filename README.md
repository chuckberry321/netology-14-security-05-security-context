# Домашнее задание к занятию "14.5 SecurityContext, NetworkPolicies"

## Задача 1: Рассмотрите пример 14.5/example-security-context.yml

### Ответ:

Переключаемся в namespace netology и создаем модуль

```
vagrant@vagrant:~/netology-14-security-05-security-context$ kubectl config set-context --current --namespace=netology
Context "minikube" modified.
vagrant@vagrant:~/netology-14-security-05-security-context$ kubectl apply -f ./example-security-context.yml
pod/security-context-demo created
vagrant@vagrant:~/netology-14-security-05-security-context$
```

Проверяем настройки внутри контейнера

```
vagrant@vagrant:~/netology-14-security-05-security-context$ kubectl logs security-context-demo
uid=1000 gid=3000 groups=3000
vagrant@vagrant:~/netology-14-security-05-security-context$
```

