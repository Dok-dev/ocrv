# Описание задачи

#### Базовый уровень сложности:    
Необходимо написать сценарий на Ansible, который будет разворачивать следующие компоненты:    
•        Nginx;    
•        PHP;    
•        MySQL.    

Критерии выполнения задачи:    
1. CentOS.    
2. Nginx должен по умолчанию отдавать index.php со след. содержанием <?php phpinfo(); ?>    

#### Продвинутый уровень сложности:    
Необходимо написать сценарий на Ansible, который будет разворачивать следующие компоненты:    
•        Nginx;    
•        PHP;    
•        MySQL с репликацией master-slave;    
•        Redis.    

Критерии выполнения задачи:    
1. Можно использовать любую из популярных ОС: CentOS, Ubuntu, Debian.    
2. Для каждого пакета должен быть выбор версии.    
3. Должна быть возможность настройки или установки отдельного компонента сценария.    
4. Nginx должен по умолчанию отдавать index.php со след. содержанием <?php phpinfo(); ?>


# Решение:
Разработан комплект ролей  - Nginx, PHP + PHP-FPM, MySQL, Redis.    
Комплект совместим с различными версиями ОС Linux семейства RedHat и Debian.     
Протестирован на CentOS 7, CentOS Stream 8, Ubuntu 20.4, Debian 11.

Роль Nginx полностью написана в рамках данной задачи, но кроме того поддерживает генерацию сертификатов Let's Encrypt для используемого домена.    

Роли PHP(php-fmp), MySQL, Redis с небольшими доработками взяты из этих источников:    
- [php role](https://github.com/geerlingguy/ansible-role-php) (fixed)
- [mysql role](https://github.com/geerlingguy/ansible-role-mysql) (fixed)
- [redis role](https://github.com/geerlingguy/ansible-role-redis)

### Основные переменные group_vars/all.yml

| Variable           | Default | Comments (type)                                             |
| :----------------- | :------ | :---------------------------------------------------------- |
| use_tls | false    | Use SSL Let's Encrypt certificate by Nginx. |
| use_php | true    | Configure Nginx to use PHP. |
| php_enable_webserver | true    | Up webserver with PHP. |
| php_enable_php_fpm | true    | Install PHP with php-fpm. |
| mysql_root_password     | root      | Configure MySQL root password.                 |
| mysql_databases | ocrv_db    | Creates MySQL database. |
| nginx_ver | latest    | Set Nginx version. |
| php_ver | present    | Set PHP version. |
| mysql_ver | present    | Set MySQL version. |
| redis_ver | latest    | Set Redis version. |

Контент для index.php расположен в шаблоне [roles/nginx/templates/index.j2](roles/nginx/templates/index.j2)    

При отключении php-роли в playbook для безошибочной работы Nginx необходимо задать переменную `use_php: false`.    
А при установке PHP без/перед Nginx установить `php_enable_webserver: false`.

#### Пример запуска [прилагающегося плейбука](main-depl.yml) для установки всех ролей:

```bash
ansible-playbook main-depl.yml -i inventory/stage.yml --vault-password-file  vault_pass.txt
```

### Dependencies

None

### Author

- [Timofey Biryukov](https://github.com/Dok-dev)

### License

This project is under the MIT License.

### Copyright

(c) 2022, Timofey Biryukov
