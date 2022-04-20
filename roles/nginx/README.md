# Ansible Role: Nginx

Install Nginx on Linux and generate Let's Encrypt certificate for used domain if it's necessary.

Requirements
------------

On RedHat-based distributions, requires the EPEL repository (you can simply add the role `geerlingguy.repo-epel` to install ensure EPEL is available).

Role Variables
--------------
Available variables are listed below, along with default values (see `defaults/main.yml`):

nginx_ver: present    
use_tls: false    
use_php: true    
worker_processes: auto    
worker_connections: 1024    
client_max_body_size: 32M    
fastcgi_pass_path: 127.0.0.1:9000


Dependencies
------------

The role is related to the PHP role in this repository. However, it can be set separately when setting a variable `use_php: false`.

License
-------

MIT / BSD

Author Information
------------------

This role was created in 2022 by Timofey Biryukov.
