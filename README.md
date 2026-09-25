# Тема: Развёртывание веб-сервера с использованием Amazon EC2

**Выполнил:** Andrei Boico
**Группа:** IA-240
**Год:** 2026

## Цель работы

Изучить основные возможности Amazon EC2: создание виртуального сервера, настройку сетевого доступа, подключение по SSH, установку веб-сервера Nginx, размещение HTML-сайта и управление EC2 с помощью AWS CLI.

---

# Пункт 1. Подготовка AWS и настройка доступа

В начале работы была открыта консоль Amazon Web Services. Для работы с EC2 был выбран регион **Europe (Frankfurt) — eu-central-1**.

Также была настроена безопасность AWS и создана необходимая конфигурация для дальнейшей работы с виртуальным сервером.

Для подключения к EC2 был создан ключ:

```text
andrei-keypair
```

**Результат:** AWS был подготовлен для создания EC2-инстанса.

**Скриншот:** консоль AWS с выбранным регионом / настройками.

---

# Пункт 2. Создание EC2-инстанса и установка Nginx

В Amazon EC2 был создан виртуальный сервер с именем:

```text
webserver
```

### Основные параметры:

* **Instance ID:** `i-0b41ce27e75ec723e`
* **Instance type:** `t3.micro`
* **Region:** `eu-central-1`
* **Availability Zone:** `eu-central-1c`
* **Key pair:** `andrei-keypair`
* **Security Group:** `webserver-sg`
* **Операционная система:** Amazon Linux

Для установки веб-сервера использовался следующий User Data:

```bash
#!/bin/bash
dnf -y update
dnf -y install htop nginx
systemctl enable --now nginx
```

После запуска инстанса было получено:

```text
3/3 checks passed
```

Затем через браузер был открыт публичный IP-адрес:

```text
http://3.77.42.105
```

Открылась стандартная страница Nginx.

**Результат:** EC2-инстанс успешно создан, Nginx установлен и работает.

**Скриншот:** EC2 webserver + стандартная страница Nginx.

<img width="1340" height="684" alt="Снимок экрана 2026-09-22 122634" src="https://github.com/user-attachments/assets/644bc546-4690-4de9-bfae-f421c28ce852" />

<img width="1221" height="751" alt="Снимок экрана 2026-09-22 122741" src="https://github.com/user-attachments/assets/f9ce92cf-2b94-4cfb-83e9-bd3049d1d5fe" />

<img width="1437" height="1101" alt="Снимок экрана 2026-09-22 122951" src="https://github.com/user-attachments/assets/8669c174-7668-46b5-8f99-cf9ae0c5726c" />

<img width="1917" height="1101" alt="Снимок экрана 2026-09-22 122945" src="https://github.com/user-attachments/assets/ba2498fa-3cb7-4f25-b67a-8ea63859b7e0" />

---

# Пункт 3. Проверка состояния и мониторинга EC2

После создания сервера были проверены системные проверки EC2.

Инстанс показал:

```text
3/3 checks passed
```

Также был открыт раздел **Monitoring**, где были просмотрены основные показатели работы сервера, включая CPU и сетевую активность.

Был также просмотрен системный журнал EC2 через:

```text
Actions → Monitor and troubleshoot → Get system log
```

Detailed Monitoring дополнительно не включался.

**Результат:** состояние виртуального сервера было проверено, системные показатели и журнал доступны в AWS Console.

**Скриншот:** 3/3 checks passed и/или Monitoring.

<img width="964" height="484" alt="Снимок экрана 2026-09-22 122044" src="https://github.com/user-attachments/assets/afc89484-b7e1-4e57-b046-2c4647437152" />

<img width="1215" height="516" alt="Снимок экрана 2026-09-22 122051" src="https://github.com/user-attachments/assets/67b2b061-b1fd-4a2a-a8f9-38c3169ca66a" />

<img width="1340" height="684" alt="Снимок экрана 2026-09-22 122634" src="https://github.com/user-attachments/assets/e1ca8285-e645-48a5-bf7d-1fe3876a3505" />

<img width="1340" height="684" alt="Снимок экрана 2026-09-22 122634" src="https://github.com/user-attachments/assets/e4982bb9-9bb4-4af6-b1f8-03e4bcb35fda" />

---

# Пункт 4. Подключение к EC2 по SSH

Для удалённого подключения к серверу использовался протокол SSH.

Ключ:

```text
andrei-keypair.pem
```

находился в:

```text
C:\Users\Admin\Downloads\andrei-keypair.pem
```

Подключение выполнялось из Windows CMD:

```cmd
ssh -i "C:\Users\Admin\Downloads\andrei-keypair.pem" ec2-user@3.77.42.105
```

После успешного подключения появился терминал:

```text
[ec2-user@ip-172-31-xx-xx ~]$
```

Для проверки пользователя была выполнена команда:

```bash
whoami
```

Результат:

```text
ec2-user
```

Также была проверена операционная система:

```bash
cat /etc/os-release
```

Состояние Nginx проверялось командой:

```bash
systemctl status nginx
```

Nginx находился в состоянии:

```text
active (running)
```

**Результат:** удалённое подключение к EC2 через SSH успешно выполнено.

**Скриншот:** терминал с подключением к EC2 и командой whoami.

<img width="706" height="426" alt="Снимок экрана 2026-09-24 164407" src="https://github.com/user-attachments/assets/3da3979a-6c01-4820-9230-aeb98a464929" />

<img width="1005" height="478" alt="Снимок экрана 2026-09-24 164522" src="https://github.com/user-attachments/assets/c6482d31-8e13-411e-868f-f163009bd6de" />

<img width="937" height="460" alt="Снимок экрана 2026-09-24 170257" src="https://github.com/user-attachments/assets/4260c5ef-7ec2-4c70-a749-ff69918bd0ad" />

---

# Пункт 5. Размещение HTML-сайта на EC2

На локальном компьютере была создана папка:

```text
C:\Users\Admin\Desktop\my-site
```

В ней были созданы три HTML-файла:

* `index.html`
* `about.html`
* `contact.html`

Главная страница содержала ссылки на страницы «О нас» и «Контакты».

Для передачи файлов на EC2 использовалась команда `scp`:

```cmd
scp -i "C:\Users\Admin\Downloads\andrei-keypair.pem" index.html about.html contact.html ec2-user@3.77.42.105:~
```

После передачи файлов они были скопированы в директорию Nginx:

```bash
sudo cp ~/index.html ~/about.html ~/contact.html /usr/share/nginx/html/
```

После этого были проверены страницы сайта:

```text
http://3.77.42.105
http://3.77.42.105/about.html
http://3.77.42.105/contact.html
```

Все три страницы успешно открывались в браузере.

**Результат:** простой HTML-сайт был размещён на веб-сервере Nginx в EC2.

### Скриншоты

1. Главная страница.
2. Страница «О нас».
3. Страница «Контакты».

<img width="1056" height="196" alt="Снимок экрана 2026-09-24 170059" src="https://github.com/user-attachments/assets/1ccf8cdf-1379-4438-826e-2b50bb0d870c" />

<img width="1732" height="847" alt="Снимок экрана 2026-09-24 170244" src="https://github.com/user-attachments/assets/cdaee872-7741-407d-bb15-48682f36c9fe" />

<img width="1033" height="419" alt="image" src="https://github.com/user-attachments/assets/f7ce8f8d-c398-4ec1-ba6d-de413b49e541" />

---

# Пункт 6. Остановка EC2 с помощью AWS CLI

Для выполнения данного пункта был открыт **AWS CloudShell**.

Сначала была проверена информация об EC2:

```bash
aws ec2 describe-instances --region eu-central-1
```

Затем виртуальный сервер был остановлен с помощью AWS CLI:

```bash
aws ec2 stop-instances --instance-ids i-0b41ce27e75ec723e --region eu-central-1
```

После выполнения команды состояние инстанса сначала изменилось на:

```text
stopping
```

После завершения операции состояние стало:

```text
stopped
```

Таким образом, EC2-инстанс был успешно остановлен.

**Результат:** управление состоянием EC2 с помощью AWS CLI успешно выполнено.

**Скриншот:** AWS CloudShell с командой `stop-instances` и состояние Stopped в EC2 Console.

---

# Итоговый результат

В ходе выполнения лабораторной работы были выполнены все основные этапы работы с Amazon EC2:

1. Подготовлена AWS-среда.
2. Создан EC2-инстанс `webserver`.
3. Настроена Security Group.
4. Установлен и запущен Nginx.
5. Проверено состояние EC2 и мониторинг.
6. Выполнено SSH-подключение к серверу.
7. Создан и размещён HTML-сайт.
8. Файлы сайта переданы на сервер с помощью SCP.
9. EC2 был остановлен через AWS CLI.

В результате был получен практический опыт создания и управления виртуальным сервером в облачной инфраструктуре AWS.

# Вывод

В данной лабораторной работе я изучил основные возможности сервиса Amazon EC2.

Я научился создавать виртуальный сервер, настраивать правила сетевого доступа, подключаться к нему через SSH, устанавливать Nginx и размещать HTML-файлы на веб-сервере.

Также была изучена передача файлов с помощью SCP и управление состоянием EC2 через AWS CLI.

Полученные навыки могут использоваться для базового развёртывания веб-сайтов и веб-приложений в облаке.
