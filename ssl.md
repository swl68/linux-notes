<!--Подготовка-->
## Подготовка
Устанавливаем venv если отсуствует: \
```sudo apt install python3-venv``` \
Создаем директорию с окружением: \
```sudo python3 -m venv /opt/certbot/``` \
Обновляем pip: \
```sudo /opt/certbot/bin/pip install --upgrade pip``` \

<!--Установка-->
## Установка 
Установка certbot через pip: \
```sudo /opt/certbot/bin/pip install certbot``` \
Делаем короткую ссылку: \
```sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot``` \
<!--Получение сертификата-->
## Получение сертификата 
Директива --dry-run для проверки наличия ошибок, без получения самого сертификата, уберем её если всё в порядке \
```
sudo certbot certonly --dry-run --standalone \
    --pre-hook "systemctl stop nginx.service" \
    -d domain \
    -m mail \
    --post-hook "systemctl start nginx.service"
```

<!--Обновление сертификата-->
## Обновление сертификата
ВЖАНО! Обновление можно произвести только если получение сертификата делалось таким же способом и на той же машине.
Для проверки выполним: \
```sudo certbot renew --dry-run``` \
 Если все хорошо, создаем systemd unit .service и .timer для автоматического продления 
```
sudo tee /etc/systemd/system/cert-bot-update.service << EOF
[Unit]
Description=Auto update ssl certs by Certbot
Documentation=file:///usr/share/doc/python-certbot-doc/html/index.html
Documentation=https://certbot.eff.org/docs
[Service]
Type=oneshot
ExecStart=/usr/bin/certbot --quiet renew
EOF
```
Таймер, который запускает проверку каждую ночь в 01:00 
```
sudo tee /etc/systemd/system/cert-bot-update.timer << EOF
[Unit]
Description=Run certbot daily

[Timer]
OnCalendar=*-*-* 01:00:00
Persistent=true
Unit=cert-bot-update.service

[Install]
WantedBy=timers.target
EOF
```
<!--Дериктивы certbot-->
## Дириктивы certbot 

1. --dry-run - позволит выполнить тестовый запуск команды если ошибок нет в выводе, значит все в порядке: 
```certbot renew --dry-run```

2. --standalone - режим при котором требуется вручную останавливать Ваш сервер (освобождать 80 порт): \

```sudo certbot certonly --standalone```
  
3. Режим при котором возможность остановки сервера отсуствует:\
При таком режиме сервер должен быть готов ответить на запрос\
```curl your_host.ru/.well-known/acme-challenge```
nginx или apache нужно настроить location на этот путь.

    ```sudo certbot certonly --webroot```
    
4. Режим полностью ручной с поэтопным выводом происходящего:\
    ```sudo certbot certonly --manual```
