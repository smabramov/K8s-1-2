# «Базовые объекты K8S» - Абрамов Сергей

### Цель задания

В тестовой среде для работы с Kubernetes, установленной в предыдущем ДЗ, необходимо развернуть Pod с приложением и подключиться к нему со своего локального компьютера. 

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключенным Git-репозиторием.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. Описание [Pod](https://kubernetes.io/docs/concepts/workloads/pods/) и примеры манифестов.
2. Описание [Service](https://kubernetes.io/docs/concepts/services-networking/service/).

------

### Задание 1. Создать Pod с именем hello-world

1. Создать манифест (yaml-конфигурацию) Pod.

[pod](https://github.com/smabramov/K8s-1-2/blob/7bfe91f7dd0737a17439ac1bae693cb20c42bbac/pod_yaml/pod.yaml)

2. Использовать image - gcr.io/kubernetes-e2e-test-images/echoserver:2.2.

```
serg@k8snode:~/git/K8s-1-2/pod_yaml$ kubectl apply -f pod.yaml 
pod/hello-world created
serg@k8snode:~/git/K8s-1-2/pod_yaml$ kubectl get pods -o wide
NAME          READY   STATUS    RESTARTS   AGE   IP             NODE         NOMINATED NODE   READINESS GATES
hello-world   1/1     Running   0          21s   10.1.218.141   k8sclaster   <none>           <none>
```

3. Подключиться локально к Pod с помощью `kubectl port-forward` и вывести значение (curl или в браузере).

![k1](https://github.com/smabramov/K8s-1-2/blob/7bfe91f7dd0737a17439ac1bae693cb20c42bbac/png/k1.png)

------

### Задание 2. Создать Service и подключить его к Pod

1. Создать Pod с именем netology-web.

[web](https://github.com/smabramov/K8s-1-2/blob/7bfe91f7dd0737a17439ac1bae693cb20c42bbac/pod_yaml/web.yaml)

2. Использовать image — gcr.io/kubernetes-e2e-test-images/echoserver:2.2.

```
^Cserg@k8snode:~/git/K8s-1-2/pod_yaml$ kubectl apply -f web.yaml 
pod/netology-web created
serg@k8snode:~/git/K8s-1-2/pod_yaml$ kubectl get pods -o wide
NAME           READY   STATUS    RESTARTS   AGE   IP             NODE         NOMINATED NODE   READINESS GATES
hello-world    1/1     Running   0          12m   10.1.218.141   k8sclaster   <none>           <none>
netology-web   1/1     Running   0          7s    10.1.218.142   k8sclaster   <none>           <none>
```

3. Создать Service с именем netology-svc и подключить к netology-web.

[service](https://github.com/smabramov/K8s-1-2/blob/7bfe91f7dd0737a17439ac1bae693cb20c42bbac/pod_yaml/service.yaml)

```
serg@k8snode:~/git/K8s-1-2/pod_yaml$ kubectl apply -f service.yaml
service/netology-svc created
serg@k8snode:~/git/K8s-1-2/pod_yaml$ kubectl describe svc netology-svc
Name:                     netology-svc
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=netology
Type:                     ClusterIP
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.152.183.28
IPs:                      10.152.183.28
Port:                     wed  8080/TCP
TargetPort:               8080/TCP
Endpoints:                10.1.218.142:8080
Session Affinity:         None
Internal Traffic Policy:  Cluster
Events:                   <none>
```

4. Подключиться локально к Service с помощью `kubectl port-forward` и вывести значение (curl или в браузере).

![k2](https://github.com/smabramov/K8s-1-2/blob/7bfe91f7dd0737a17439ac1bae693cb20c42bbac/png/k2.png)

------

### Правила приёма работы

1. Домашняя работа оформляется в своем Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода команд `kubectl get pods`, а также скриншот результата подключения.
3. Репозиторий должен содержать файлы манифестов и ссылки на них в файле README.md.

------

### Критерии оценки
Зачёт — выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку — задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
