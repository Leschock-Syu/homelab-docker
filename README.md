# 🏠 My Homelab Docker Setup

Моя домашняя лаборатория. Проект для себя, где настроены скачивание, VPN, сетевое хранилище, DNS и полноценный мониторинг.

---

## 🗺 Карта сервисов и портов

| Сервис | Назначение | Доступ / Ссылка | Конфиги / Заметки |
| :--- | :--- | :--- | :--- |
| **qBitTorrent** | Скачивание торрентов | `http://<IP_СЕРВЕРА>:8080` | Проверить дефолтный пароль при первом запуске |
| **AmneziaWG** | Личный VPN (WireGuard) | Через консоль / файлы | Конфиги клиентов генерируются в volumes |
| **Samba (SMB)** | Сетевые папки для ПК/ТВ | `smb://<IP_СЕРВЕРА>` | Шары для локальной сети |
| **DuckDNS** | Динамический DNS | Работает в фоне | Обновляет домен для внешнего доступа |

### 📊 Стек Мониторинга (папка `/monitoring`)

| Сервис | Назначение | Доступ / Ссылка |
| :--- | :--- | :--- |
| **Grafana** | Красивые дашборды и графики | `http://<IP_СЕРВЕРА>:3000` |
| **Prometheus** | Сборщик метрик и бд | `http://<IP_СЕРВЕРА>:9090` |
| **cAdvisor** | Аналитика по Docker-контейнерам | `http://<IP_СЕРВЕРА>:8081` (или внутренний) |
| **Node Exporter** | Метрики железа сервера (CPU, RAM, HDD) | Внутренний порт `9100` |

---

## 🚀 Быстрая шпаргалка по командам

**Управление основным стеком:**
```bash
# Запустить (qBitTorrent, VPN, Samba, DuckDNS)
sudo docker compose up -d

# Остановить весь стек
sudo docker compose down

# Перезапустить один контейнер, например qBitTorrent (например, если завис)
sudo docker compose restart qbittorrent
```

**Управление мониторингом:**
```bash
# Запустить стек мониторинга
cd monitoring && docker compose up -d && cd ..

# Остановить мониторинг
cd monitoring && docker compose down && cd ..
```

**Полезные команды для отладки:**
```bash
# Посмотреть логи контейнера, например AmnewziaWG Easy
sudo docker compose logs -f amneziawg-easy

# Проверить потребление ресурсов контейнерами
sudo docker stats
```

---

## 🛠 Порядок развертывания с нуля

1. **Клонировать репозиторий:**
   ```bash
   git clone https://github.com
   cd homelab-docker
   ```
2. **Настроить секреты:**
   ```bash
   cp .env.example .env
   nano .env
   ```
   *Обязательно прописать токен DuckDNS, пароли для Samba и пути к жестким дискам.*
3. **Запустить всё:**
   ```bash
   sudo docker compose up -d
   cd monitoring && docker compose up -d && cd ..
   ```

---

## 📌 Памятка по обновлению сервисов
Чтобы обновить все контейнеры до актуальных версий:
```bash
docker compose pull && docker compose up -d
cd monitoring && docker compose pull && docker compose up -d && cd ..
```
