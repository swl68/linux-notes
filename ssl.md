# Подготовка
Устанавливаем venv если отсуствует: \
```sudo apt install python3-venv```
Создаем директорию с окружением: \
```sudo python3 -m venv /opt/certbot/```
Обновляем pip:\\
```sudo /opt/certbot/bin/pip install --upgrade pip```

# Установка 
Установка certbot через pip: \
```sudo /opt/certbot/bin/pip install certbot```

Делаем короткую ссылку: \
```sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot```

# Обновление 
 TODO
# Дериктивы certbot 

--dry-run - позволит выполнить тестовый запуск команды если ошибок нет в выводе, значит все в порядке: \
```certbot renew --dry-run```

--standalone - режим при котором требуется вручную останавливать Ваш сервер (освобождать 80 порт): \

```sudo certbot certonly --standalone```
  
2. Режим при котором возможность остановки сервера отсуствует:
При таком режиме сервер должен быть готов ответить на запрос ```curl your_host.ru/.well-known/acme-challenge```
nginx или apache нужно настроить location на этот путь.

    ```sudo certbot certonly --webroot```
    
3. Режим полностью ручной с поэтопным выводом происходящего:
    ```sudo certbot certonly --manual```
