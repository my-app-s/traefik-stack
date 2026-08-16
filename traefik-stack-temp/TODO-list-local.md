# TODO list local

## Main list HOT

- Разделение работы
    - Отделить структурировать стеки с Traefik
    - Отдельно его и отдельно развертывание рецептов
        - Возможность работы как с Traefik так и локально
        - секреты по .env

## Приверный набросок

```yml
version: '3.8'

services:
  web-app:
    image: nginx:alpine
    ports:
      - "80:80"
    
    # 1. Запуск от некорневого пользователя (если образ это поддерживает)
    # 101 — это стандартный UID пользователя nginx в alpine-образе
    user: "101:101"

    # 2. Делаем файловую систему только для чтения
    read_only: true

    # Поскольку nginx нужно куда-то писать логи и временные файлы,
    # выделяем ему изолированную память (tmpfs), чтобы не сломать его работу
    tmpfs:
      - /var/cache/nginx
      - /var/run
      - /tmp

    # 3. Отсекаем лишние права ядра (Capabilities)
    cap_drop:
      - ALL
    cap_add:
      - NET_BIND_SERVICE # Позволяет nginx привязаться к 80 порту

    # 4. Жестко ограничиваем ресурсы, чтобы избежать DoS-атак
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 256M

  database:
    image: postgres:alpine
    environment:
      POSTGRES_PASSWORD: super_secret_password
    
    # Для баз данных read_only: true использовать сложнее, так как они активно пишут в файлы.
    # Поэтому здесь минимизируем привилегии через отсечение capabilities и лимиты ресурсов.
    cap_drop:
      - ALL
    cap_add:
      - CHOWN
      - SETGID
      - SETUID
    
    # Базе данных обязательно нужен внешний volume, иначе данные сотрутся.
    # Права внутри этого volume будут ограничены пользователем postgres.
    volumes:
      - pgdata:/var/lib/postgresql/data
      
    deploy:
      resources:
        limits:
          cpus: '1.0'
          memory: 1G

volumes:
  pgdata:
```

## Самое главное аккуратно и минимально главное что бы самое главное рабоало
- Без нагромождений
- Не перегружать
- Лишнего не надо
- Универсально

## Сделать обязательное уведомление о безопасности работы с контейнером помимо дисклеймера

# Обязательно