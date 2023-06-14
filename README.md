## Установка

[Видео-инструкция](https://youtu.be/_Y9_Fq-ayfE)

### 1. Подключение к серверу
```
ssh root@<IP_СЕРВЕРА>
```

Сервер можно приобрести у любого хостера.

Советую [неплохой вариант](https://hshp.host/?from=7102) (для тех, кто готов отдавать небольшую сумму за аренду собственного сервера).

Если необходима максимальная скорость подключения к интернету (1000 мбит/сек) — [попробуйте надёжного хостинг-провайдера](https://vdsina.ru/?partner=atrxagdukh).

### 2. Установка Docker

Если вы впервые подключились к серверу, выполните следующие команды:

```
apt update && apt install curl -y
```

А затем установите Docker следующей командой:

```
curl -fsSL https://get.docker.com | sh
```

### 3. Запуск контейнера

```
docker run -d \
  --name=wg-russian \
  --pull=always \
  -e PORT=3000 \
  -e WG_HOST=<IP_СЕРВЕРА> \
  -v ~/.wg-russian:/etc/wireguard \
  -p 51820:51820/udp \
  -p 51821:3000/tcp \
  --cap-add=NET_ADMIN \
  --cap-add=SYS_MODULE \
  --sysctl="net.ipv4.conf.all.src_valid_mark=1" \
  --sysctl="net.ipv4.ip_forward=1" \
  --restart unless-stopped \
  bradtraversyfollower/wg-russian
```

> Изменить порт, на котором запускается веб-приложение - панель управления:
```
-p <любой свободный порт>:3000/tcp
```

> Например, можно сделать так:
```
-p 5678:3000/tcp
```

Тогда веб-приложение будет доступно по адресу:
> http://<IP_СЕРВЕРА>:5678

> В целях безопасности рекомендуется выставить пароль от панели управления:
```
-e PASSWORD=<ВАШ_ПАРОЛЬ>
```

> Также можно поменять стандартные DNS-сервера:
```
-e WG_DEFAULT_DNS=77.88.8.88,77.88.8.2
```

В примере выше показываются сервера [Яндекс DNS](https://dns.yandex.ru/)

## Обновление до последней версии

1.
```
docker rm -f wg-russian
```

2. [Повторить шаг #3](https://github.com/seniorvladislav/wg-russian/blob/main/README.md#3-%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D0%B0)
