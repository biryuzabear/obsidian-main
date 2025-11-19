---
tags:
aliases:
---
# Мои домены

## subair.org

Чтобы создать платформу для аренды, то что мы делаем с семьей. 

# Техническое

## Cloudflare Tunnel

### Настройка для пробрасывания с wildcard

Я использую этот файл с [[Dokploy]], потому что у меня были какие-то проблемы с ним. Возможно это не необходимо при других тоннелях.

Файл находится в папке `/etc/cloudflared/config.yml`

```
tunnel: 577b4afa-9a28-4f73-9c7b-e76976d52f98
credentials-file: /home/biryuzabear/.cloudflared/577b4afa-9a28-4f73-9c7b-e76976d52f98.json

ingress:
  - hostname: dev.subair.org
    service: http://[::1]:3000
  - hostname: "*.subair.org"
    service: http://[::1]:80
  - service: http_status:404
```

Вот это пробрасывание с помощью IPv6 (если я правильно помню и понимаю) - без этого не работало с Traefic установленном в [[Dokploy]].