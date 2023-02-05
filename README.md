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




##

### Ответ:

Создаем неймспейсы и стави на них метки
```
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$ kubectl create ns team-a
namespace/team-a created
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$ kubectl create ns team-b
namespace/team-b created
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$ kubectl label namespace team-a app=team-a
namespace/team-a labeled
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$ kubectl label namespace team-b app=team-b
namespace/team-b labeled
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$ kubectl get ns
NAME              STATUS   AGE
default           Active   181d
kube-node-lease   Active   181d
kube-public       Active   181d
kube-system       Active   181d
netology          Active   24h
team-a            Active   32s
team-b            Active   15s
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$
```

Создаем приложение и запускаем
```
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$ kubectl apply -f app.yaml
pod/ta created
service/ta created
pod/tb created
service/tb created
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$ kubectl get po --namespace team-a
NAME   READY   STATUS    RESTARTS   AGE
ta     1/1     Running   0          63s
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$ kubectl get po --namespace team-b
NAME   READY   STATUS    RESTARTS   AGE
tb     1/1     Running   0          66s
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$
```

Проверяем отсутствие ограничений:
```
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$ kubectl exec -it ta -n team-a -- bash
root@ta:/# curl tb.team-b
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
root@ta:/# curl google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
root@ta:/# exit
exit
command terminated with exit code 127
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$ kubectl exec -it tb -n team-b -- bash
root@tb:/# curl ta.team-a
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
root@tb:/# curl google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
root@tb:/# exit
exit
vagrant@vagrant:~/netology-14-security-05-security-context/netowrk-policy$
```

Создаем манифесты network-policy и применяем их:
```

```
