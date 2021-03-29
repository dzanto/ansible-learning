# Основы работы с Ansible
## Конфигурирование ansible
Просмотр версии ansible
```
ansible --version
```
### Inventroy файл
В inventory файле описаны сервера и группы, так же могут быть указаны переменные
Просмотр всех групп и серверов описанных в inventory файле (hosts.yml) и переменных, которые к ним относятся:
```
ansible-inventory --list
```
Просмотр структуры групп и серверов в виде дерева:
```
ansible-inventory --graph
```

### Заупск модулей ansible
```
ansible -i hosts.yml all -m ping
ansible -i [имя инветори файла] [группа хостов] -m [команда]
```
### Конфиг файл ansible.cfg
```
[defaults]
host_key_checking = false # отключить подтверждение fingerprint при первом подключении к хосту по ssh
inventory = ./hosts.yml   # указываем inventory файл
```
После указания inventory в конфигурационном файле запуск ansible выглядит так:
```
ansible all -m ping
```
## Ad-Hoc команды ansible
Просмотр подробной информации и основных переменных о хосте:
```
ansible my_ubuntu -m setup
```
Выполнение shell команды на хосте (при использовании command нельзя использовать env системы(например $HOME) и команды перенаправления <>|;&
```
ansible all -m shell -a "uptime"
ansible all -m command -a "uptime"
```
