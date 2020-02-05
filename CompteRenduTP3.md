#ANSIBLE

## Création d'un inventaire 

On crée un répertoire dédié au projet, à l'interieur un répertoire ansible puis un répertoire dédié aux inventaires puis le fichier correspondant :

my-project/ansible/inventories/setup.yml

Puis on écris dedans :

```
all:
  vars:
    ansible_user: centos
    ansible_ssh_private_key_file: ~/.ssd/DevOpsSSHKey.pub
  children:
    prod:
      hosts: amaury-queron-formation.takima.io
```

Pour le tester :

```
ansible all -i inventories/setup.yml -m setup -a "filter=ansible_distribution*"
```

On oublie pas de supprimer apache sur la machine centos qui à étét installé dans le TD masi qui n'est plus utile :

```
ansible all -i inventories/setup.yml -m yum -a "name=httpd state=absent" --become
```

Création d'un playbook dans le répertoire ansible :

```touch playbook.yml```

Puis on y écris :
```
- hosts: all
  gather_facts: false
  become: yes
 
  tasks:
  - name: Test connection
    ping:
```

On le lance avec la commande suivante pour tester (l'option --check-syntax permet de vérifier le playbook sans l'éxecuter):
```
ansible-playbook -i inventories/setup.yml playbook.yml (--check-syntax)
```

Maintenant on crée un playbook plus complet pour installer Docker: 
Dans la partie "tasks", chaque nom correspond à une nouvelle tâche, on indique ensuite quel outil on utilise (ici par exemple  yum) 
```
- hosts: all
gather_facts: false
become: yes
tasks:
# Install Docker
- name: Install yum-utils
yum:
name: yum-utils
state: latest
- name: Install device-mapper-persistent-data
yum:
name: device-mapper-persistent-data
state: latest
- name: Install lvm2
yum:
name: lvm2
state: latest
- name: Add Docker stable repository
yum_repository:
name: docker-ce
description: Docker CE Stable - $basearch
baseurl: https://download.docker.com/linux/centos/7/$basearch/stable
state: present
enabled: yes
gpgcheck: yes
gpgkey: https://download.docker.com/linux/centos/gpg
- name: Install Docker
yum:
name: docker-ce
state: present
- name: Make sure Docker is running
service: name=docker state=started
tags: docker
```

Nous allons maintenant utiliser des roles afin de séparer les tâches par types
Tou d'abord on crée le répertoire de roles/docker :
```
ansible-galaxy init roles/docker
```
Une arborescence est créée dans docker avec un répertoires dédié aux tasks ou encore aux handlers
Il ne faut surtout pas indiqué le mot clé "tasks" dedans mais directement écrire les noms des tâches

Cela donne pour le fichier /roles/docker/main.yml
```
-   name: Install python setup tools
    yum: name=python-setuptools
    tags: docker

-   name: Install Pypi
    easy_install: name=pip
    tags: docker

-   name: Install docker-py
    pip: name=docker-py
    tags: docker

- name: Add Docker stable repository
  yum_repository:
    name: docker-ce
    description: Docker CE Stable - $basearch
    baseurl: https://download.docker.com/linux/centos/7/$basearch/stable
    state: present
    enabled: yes
    gpgcheck: yes
    gpgkey: https://download.docker.com/linux/centos/gpg

- name: Install Docker
  yum:
    name: docker-ce
    state: present

- name: Make sure Docker is running
  service: name=docker state=started
  tags: docker
```

Ensuite on crée un common role afin de bloqué tous les ports puis d'activer uniquement les port 22 (ssh) et 80 (http)

on utilise la commande :
```ansible-galaxy init roles/common```

puis on ajoute dans le ficheir roles/common/tasks/main.yml:
```
- name: Close all ports
    - firewalld:
        port: 0-65000/all
        permanent: true
        state: disabled

- name: Open http port
    - firewalld:
        port: 80/http
        permanent: true
        state: enabled

- name: Open ssh port
    - firewalld:
        port: 22/ssh
        permanent: true
        state: enabled

        - name: Set the policy for the INPUT chain to DROP
        iptables:
          chain: INPUT
          policy: DROP
```

