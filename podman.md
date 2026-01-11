# Запуск gitea_runner podman
```
podman pod create --name gitea_runner -p 8088:8088 && \
podman run -it \
--pod gitea_runner \
--name runner1  \
--env CONFIG_FILE=/etc/config.yaml \
--env GITEA_INSTANCE_URL=http://IP_YOUR_GITEA_INSTANCE:3000 \
--env GITEA_RUNNER_REGISTRATION_TOKEN=TOKEN_FROM_GITEA \
--volume ./runner/config.yaml:/etc/config.yaml \
--volume ./runner/data:/var/data \
--volume /run/user/${UID}/podman/podman.sock:/var/run/docker.sock \
gitea/act_runner:nightly
```

# Для временного запуска, удаляется после остановки:
```
podman run -it --rm --name=temp \
	-e MYSQL_ROOT_PASSWORD=123 \
	-e MYSQL_USER=den \
	-e MYSQL_DATABASE=test \
	-p 3306:3306 mysql:latest
```

# Сгенерировать yaml для k8s на основе пода:
```podman generate kube starlink-wiki >> starlink-wiki.yaml```

# Запустить под из файла манифеста:
```podman play kube starlink-wiki.yaml```
