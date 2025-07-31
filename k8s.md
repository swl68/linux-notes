# k8s notes

# Состояния: DaemonSet, StatefulSet и Deployment:
DaemonSet, когда требуется, чтобы функции работали на всех узлах кластера;\\
StatefulSet, для работы приложений, которые нужнаются в сохранении своего состояния, например, базы данных;\\
Deployment - подходит для остального, он порождает Replicaset, который уже создает pod`ы.

# Подключение к терминалу кубпода:
```kubectl exec -it -n NAMESPACE NAME_POD -с CONTAINER_NAME -- /bin/sh```

# Выполнить команду внутри кубпода:
```kubectl exec -n NAMESPACE NAME_POD -- ls -la /```

# Скопировать файл с рабочей машины в контейнер:
```kubectl cp index.php -n NAMESPACE NAME_POD:/var/www/dokuwiki/ -c CONTAINER_NAME```

# Просмотра подов контроллера в определенном пространстве имен: 
```kubectl get pods -n NAMESPACE -o wide```

# Просмотр логов:
```kubetl logs -p POD_NAME -c CONTAINER_NAME```

# Показать последние 20 строк вывода журнала ищ выбранного пода:
```kubectl logs --tail=20 -p POD_NAME -c CONTAINER_NAME```

# Просмотр сервисов в пространстве имен:
```kubectl get service -n NAMESPACE```

# Подробное описание сервиса:
```kubectl describe service -n NAMESPACE```

# Подробное описание пода:
```kubectl describe pods -n NAMESPACE```

# Частичный запуск контенера, только с заданными командами:
```kubectl run -it gitea --image=gitea```

# Подробный вывод деплоя в пространстве имен:
```kubectl get deployments -n NAMESPACE -o wide```

# Изменить кол-во реплик в deploy, данном случае остановить все экземпляры:
```kubectl scale deployments -n NAMESPACE NAME --replicas=0```

# Изменить кол-во реплик в stateful
```kubectl scale statefulset -n NAMESPACE NAME --replicas=0```

# Удалить конкретный pod:
```kubectl delete pod -n NAMESPACE POD_NAME --now```

# Запуск с точкой монитрования: 
```kubectl run -it --rm giteaforbkp --overrides=```'
{
  "apiVersion": "v1",
  "spec": {
    "containers": [
      {
        "name": "giteaforbkp",
        "image": "gitea:1.22.3-rootless",
        "args": [
          "sh"
        ],
        "stdin": true,
        "stdinOnce": true,
        "tty": true,
        "volumeMounts": [{
          "mountPath": "/dev/shm",
          "name": "store"
        }]
      }
    ],
    "volumes": [{
      "name":"store",
      "emptyDir": {
        "medium": "Memory",
        "sizeLimit": "1Gi"
      }
    }]
  }
}
'```  --image=gitea:1.22.3-rootless -n gitea --restart=Never -- /bin/sh```
