# ДЗ №1 Cделано:
Настроено локальное окружение.
Написан Dockerfile, собран контейнер с nginx
Написан манифест web-pod.yaml, запущен соответствующий pod
Разобраны причины по которой pod frontend находится в статусе Error

# Задание №1
Разберитесь почему все pod в namespace kube-system восстановились после удаления. Укажите причину в описании PR

## Ответ:
core-dns запускаются как ReplicaSet, a документация k8s прямо говорит нам что:
"A ReplicaSet ensures that a specified number of pod replicas are running at any given time."
поэтому в случае удаления пода из перечисленных выше ReplicaSet-ов, k8s будет стремиться восстановить кол-во реплик
ref: https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/#when-to-use-a-replicaset

etcd,kindnet,kube-apiserver,kube-controller-manager,kube-proxy,kube-scheduler запускаются как DaemonSet, a документация k8s прямо говорит нам что:
"However, a DaemonSet replaces Pods that are deleted or terminated for any reason, such as in the case of node failure or disruptive node maintenance, such as a kernel upgrade."
поэтому в случае удаления пода из перечисленных выше DaemonSet-ов будет произведена его "замена" на новый
ref: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/#bare-pods

# Задание №1.1 (со ⭐️)
(Hipster Shop) Выясните причину, по которой pod frontend находится в статусе Error

## Ответ
При использовании ad-hoc режима kubectl не знает о том, что нужно описывать env-ы необходимые для frontend приложения, т.к он не знает ни об архитектуре приложения, ни о том какое приложение в контейнере запускается и что ему требуется.
Следовательно, приложение завершается с кодом exit 1 так как не находит с контейнере необходимые env, k8s помечает такой pod статусом error, так как контейнер при таком поведении "разрушается"
Добавление необходимых параметров окружения в frontend-pod-healthy.yaml позволяет запуститься frontend приложению без exit code 1
