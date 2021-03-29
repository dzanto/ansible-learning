### Хранение переменных
Переменные записываются в group_vars в файлах соответствующих групп (например my_servers)
```
---
ansible_user: testuser
ansible_ssh_private_key_file: ~/.ssh/id_rsa
server_name: newserver
new_user: testuser
user_password: testpass
```
