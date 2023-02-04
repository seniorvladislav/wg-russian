## Установка

### 1. Подключение к серверу
```
ssh root@<IP_СЕРВЕРА>
```

Сервер можно приобрести у любого хостера.

Советую [этот](https://hshp.host/?from=7102) (из-за высокой производительности за небольшие деньги).

### 2. Установка Docker
```
apt update && apt install curl -y && curl -fsSL https://get.docker.com | sh
```

Понадобится для запуска готового образа, который я для вас подготовил.

*Приветствуется* **любой фидбэк**.

### 3. Запуск контейнера

```
docker run -d \
  --name=wg-russian \
  --pull=always \
  -e PORT=3000 \
  -e WG_HOST=<IP_СЕРВЕРА> \
  -e PASSWORD= \
  -v ~/.wg-russian:/etc/wireguard \
  -p 51820:51820/udp \
  -p 80:3000/tcp \
  --cap-add=NET_ADMIN \
  --cap-add=SYS_MODULE \
  --sysctl="net.ipv4.conf.all.src_valid_mark=1" \
  --sysctl="net.ipv4.ip_forward=1" \
  --restart unless-stopped \
  bradtraversyfollower/wg-russian
```

> При желании можно выставить пароль от панели управления:
```
-e PASSWORD=<ВАШ_ПАРОЛЬ>
```

> Также можно поменять стандартные DNS-сервера:
```
-e WG_DEFAULT_DNS=77.88.8.88,77.88.8.2
```

Панель управления доступна по адресу
> http://<IP_СЕРВЕРА>

## Обновление до последней версии

### 1.
```
docker rm -f wg-russian
```

### 2. [Повторить шаг #3](https://github.com/seniorvladislav/wg-russian/blob/main/README.md#3-%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D0%B0)
