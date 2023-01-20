## Установка

### 1. Подключение к серверу
```
ssh root@<IP_СЕРВЕРА>
```

### 2. Установка Docker
```
curl -sSL https://get.docker.com | sh
```

### 3. Запуск WireGuard VPN

```
docker run -d \
  --name=wg-russian \
  -e WG_HOST=<IP_СЕРВЕРА> \
  -e PASSWORD= \
  -v ~/.wg-easy:/etc/wireguard \
  -p 51820:51820/udp \
  -p 51821:51821/tcp \
  --cap-add=NET_ADMIN \
  --cap-add=SYS_MODULE \
  --sysctl="net.ipv4.conf.all.src_valid_mark=1" \
  --sysctl="net.ipv4.ip_forward=1" \
  --restart unless-stopped \
  bradtraversyfollower/wg-easy
```
