# Домашнее задание к занятию "3.9. Элементы безопасности информационных систем"

1. Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.

![Image text](https://github.com/dkanunnikov24/devops-netology/blob/main/1.jpg)

2. Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.

![Image text](https://github.com/dkanunnikov24/devops-netology/blob/main/2a.jpg)

3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.

###

    `vagrant@vagrant:~$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \-keyout /etc/ssl/private/apache-selfsigned.key \-out /etc/ssl/certs/apache-selfsigned.crt \-subj "/C=RU/ST=Moscow/L=Moscow/O=Company Name/OU=Org/CN=test.com"
    Generating a RSA private key
    ..........................................................................................+++++
    ...+++++
    writing new private key to '/etc/ssl/private/apache-selfsigned.key'
    -----
    vagrant@vagrant:~$ sudo vim /etc/apache2/sites-available/test.com.conf
    vagrant@vagrant:~$ sudo mkdir /var/www/test
    vagrant@vagrant:~$ sudo vim /var/www/test/index.html
    vagrant@vagrant:~$ sudo a2ensite test.com.conf
    Enabling site test.com.
    To activate the new configuration, you need to run:
      systemctl reload apache2
    vagrant@vagrant:~$ sudo apache2ctl configtest
    Syntax OK
    vagrant@vagrant:~$ sudo systemctl reload apache2
    vagrant@vagrant:~$ curl https://test.com`


###


 4. Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).

##

    `vagrant@vagrant:~$ git clone --depth 1 https://github.com/drwetter/testssl.sh.git
     Cloning into 'testssl.sh'...
     remote: Enumerating objects: 100, done.
     remote: Counting objects: 100% (100/100), done.
     remote: Compressing objects: 100% (93/93), done.
     Receiving objects: 100% (100/100), 8.55 MiB | 254.00 KiB/s, done.
     remote: Total 100 (delta 14), reused 36 (delta 6), pack-reused 0
     Resolving deltas: 100% (14/14), done.
     vagrant@vagrant:~$ cd testssl.sh
     vagrant@vagrant:~/testssl.sh$ ./testssl.sh -U --sneaky https://www.google.com/

    ###########################################################

    testssl.sh       3.1dev from https://testssl.sh/dev/
    (88e80d2 2022-07-02 22:13:06)

     This program is free software. Distribution and
                 modification under GPLv2 permitted.
     USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

     Please file bugs @ https://testssl.sh/bugs/

     ###########################################################

     Using "OpenSSL 1.0.2-chacha (1.0.2k-dev)" [~183 ciphers]
     on vagrant:./bin/openssl.Linux.x86_64
     (built: "Jan 18 17:12:17 2019", platform: "linux-x86_64")

     Testing all IPv4 addresses (port 443): 64.233.165.106 64.233.165.103 64.233.165.147 64.233.165.99 64.233.165.105 64.233.165.10                             
     4
     ------------------------------------------------------------------------------------
     Start 2022-07-24 10:37:01        -->> 64.233.165.106:443 (www.google.com) <<--

     Further IP addresses:   64.233.165.103 64.233.165.147 64.233.165.99 64.233.165.105 64.233.165.104 2a00:1450:4010:c08::68
                                     2a00:1450:4010:c08::67 2a00:1450:4010:c08::69 2a00:1450:4010:c08::6a
     rDNS (64.233.165.106):  lg-in-f106.1e100.net.
     Service detected:       HTTP

     Testing vulnerabilities

     Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
     CCS (CVE-2014-0224)                       not vulnerable (OK)
     Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK)
     ROBOT                                     not vulnerable (OK)


##


5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.
###

    `vagrant@vagrant:~$ ssh-keygen -C "dkanunnikov24@gmail.com"
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/vagrant/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/vagrant/.ssh/id_rsa
    Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub
    The key fingerprint is:
    SHA256:... dkanunnikov24@gmail.com
    The key's randomart image is:
    +---[RSA 3072]----+
    |o.++             |
    |. ++.            |
    | o.B=            |
    |  *BE...         |
    |  .%=.o.S        |
    |  oooo =..       |
    |  . ++.  oo      |
    |   *o + +...     |
    |  ..+o ..o..     |
    +----[SHA256]-----+
    vagrant@vagrant:~$ cat ~/.ssh/id_rsa.pub
    ssh-rsa ...
    ...
    dkanunnikov24@gmail.com
    vagrant@vagrant:~$ ssh -T git@github.com
    Enter passphrase for key '/home/vagrant/.ssh/id_rsa':
    Hi dkanunnikov24! You've successfully authenticated, but GitHub does not provide shell access.`

###
 
6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.

###
    `vagrant@vagrant:~$ mv ~/.ssh/id_rsa ~/.ssh/id_rsa_git
    vagrant@vagrant:~$ cat ~/.ssh/config
    Host githab
      HostName github.com
      IdentityFile ~/.ssh/id_rsa_git
      User git
    vagrant@vagrant:~$ ssh githab
    Enter passphrase for key '/home/vagrant/.ssh/id_rsa_git':
    PTY allocation request failed on channel 0
    Hi dkanunnikov24! You've successfully authenticated, but GitHub does not provide shell access.
    Connection to github.com closed.`

###

7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.

 ---
## Задание для самостоятельной отработки (необязательно к выполнению)

8*. Просканируйте хост scanme.nmap.org. Какие сервисы запущены?

9*. Установите и настройте фаервол ufw на web-сервер из задания 3. Откройте доступ снаружи только к портам 22,80,443


 ---

## Как сдавать задания

Обязательными к выполнению являются задачи без указания звездочки. Их выполнение необходимо для получения зачета и диплома о профессиональной переподготовке.

Задачи со звездочкой (*) являются дополнительными задачами и/или задачами повышенной сложности. Они не являются обязательными к выполнению, но помогут вам глубже понять тему.

Домашнее задание выполните в файле readme.md в github репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Также вы можете выполнить задание в [Google Docs](https://docs.google.com/document/u/0/?tgif=d) и отправить в личном кабинете на проверку ссылку на ваш документ.
Название файла Google Docs должно содержать номер лекции и фамилию студента. Пример названия: "1.1. Введение в DevOps — Сусанна Алиева".

Если необходимо прикрепить дополнительные ссылки, просто добавьте их в свой Google Docs.

Перед тем как выслать ссылку, убедитесь, что ее содержимое не является приватным (открыто на комментирование всем, у кого есть ссылка), иначе преподаватель не сможет проверить работу. Чтобы это проверить, откройте ссылку в браузере в режиме инкогнито.

[Как предоставить доступ к файлам и папкам на Google Диске](https://support.google.com/docs/answer/2494822?hl=ru&co=GENIE.Platform%3DDesktop)

[Как запустить chrome в режиме инкогнито ](https://support.google.com/chrome/answer/95464?co=GENIE.Platform%3DDesktop&hl=ru)

[Как запустить  Safari в режиме инкогнито ](https://support.apple.com/ru-ru/guide/safari/ibrw1069/mac)

Любые вопросы по решению задач задавайте в чате учебной группы.

---

