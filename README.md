Простий проект для ansible з плейбуками для розгортання [MHDDOS](https://github.com/MHProDev/MHDDoS) в docker [porthole-ascend-cinnamon/mhddos_proxy](https://github.com/porthole-ascend-cinnamon/mhddos_proxy) на багатьох серверах і в різних датацентрах.

Плейбук написаний для Ubuntu 20.04 (LTS) x64.

Чому Ubuntu 20.04? Просто тому, що це дефолтна операційка для дроплетів в DigitalOcean. :)

**Важливо:** Якщо вірити документації [porthole-ascend-cinnamon/mhddos_proxy](https://github.com/porthole-ascend-cinnamon/mhddos_proxy), для UDP-атак потрібен VPN, тому без нього запускати на серверах цей вид атаки, не має особливого сенсу. Встановлення VPN на сервери плейбуком не передбачено.

Підготовка Mac OS
========================
1. Ставимо [Homebrew](https://brew.sh/):
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
2. Ставимо sshpass:
```shell
brew install hudochenkov/sshpass/sshpass
```

Створення серверів
========================
[Інструкція](docs/digitalocean.md) для [DigitalOcean](https://www.digitalocean.com/).

Конфігурація плейбука
========================
Всі налаштування знаходяться у файлі [hosts](hosts).

1. Прописуємо IP/хости серверів, на яких потрібно розгорнути MHDDOS. Кожний IP/хост потрібно вказувати у новому рядку в секції `[vms]`.
2. Вказуємо ssh-пароль до серверів для юзера root. Вказати у змінній `ansible_ssh_pass`.
3. Вказуємо кількість піднятих контейнерів на одному сервері. Вказати у змінній `docker_container_count`.
4. Вказуємо список цілей. Вказати у змінній `mhddos_targets`.
5. Для більш тонкої конфігурації можна налаштувати значення у змінних `mhddos_threads`, `mhddos_period` та `mhddos_rpc`.

Робота з плейбуком
========================
В проекті присутні три плейбуки:
`docker.yml` - встановлення потрібного софту на сервери
`start.yml` - запуск (створення контейнерів на серверах з потрібними налаштуваннями в потрібній кількості)
`stop.yml` - зупинка (видалення всіх контейнерів)

**Команди запуска**
Для запуску вказаних команд потрібно знаходитись у папці проекта.

```shell
ansible-playbook -i hosts docker.yml
ansible-playbook -i hosts start.yml
ansible-playbook -i hosts stop.yml
```