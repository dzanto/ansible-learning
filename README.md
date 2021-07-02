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

### Запуск модулей ansible
```
ansible -i hosts.yml all -m ping
ansible -i [имя инветори файла] [группа хостов] -m [команда]
```
### Конфигурирование файла ansible.cfg
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
ansible all -m command -a "uptime"
ansible all -m shell -a "ls /etc | grep py"
```
Скопировать файл (copy), указываем что команда должна выполниться с root правами (-b - become):
```
ansible my_ubuntu -m copy -a "src=privet.txt dest=/home mode=777" -b
```
Удалить файл. Используем модуль file, указываем путь к файлу и состояние absent(отсутствует):
```
ansible all -m file -a "path=/home/privet.txt state=absent" -b
```
Скачать файл на хост по ссылке:
```
ansible my_ubuntu -m get_url -a "url=https://www.fodors.com/wp-content/uploads/2018/10/mini-horse-2.jpg dest=/home" -b
```
Получение контента по url:
```
ansible all -m uri -a "https://ya.ru/ return_content=yes"
```
Установка пакетов:
```
ansible all -m apt -a "name=nginx state=latest" -b
```
Управление сервисами, запустить, и включить автозапуск при старте:
```
ansible all -m service -a "name=nginx state=started enabled=yes" -b
```
### Хранение переменных
Переменные записываются в group_vars в файлах соответствующих групп (например my_servers)
### Запуск playbook
```
ansible-playbook playbook-nginx-setup.yml
```
---