# DevOps TP3 - Automatisation du Déploiement avec Ansible

## Introduction

Ce TP de DevOps a pour objectif d'automatiser le déploiement d'une application sur un serveur distant en utilisant Ansible. Ansible est un outil d'automatisation et de gestion de configuration qui permet de déployer des applications de manière efficace et reproductible.

## Inventaire (Inventory)

L'inventaire Ansible permet de spécifier les hôtes sur lesquels les tâches Ansible seront exécutées. Nous avons créé un inventaire spécifique au projet dans le fichier `inventories/setup.yml`. Cet inventaire définit les hôtes sur lesquels nous allons déployer notre application.

```yaml
all:
 vars:
   ansible_user: centos
   ansible_ssh_private_key_file: /home/takima/DevOps/cle-SSH/id_rsa
 children:
   prod:
     hosts: marvinbidzana.takima.cloud
```

Nous pouvons tester notre inventaire en utilisant la commande Ansible `ping`.

```bash
ansible all -i inventories/setup.yml -m ping
```

## Faits (Facts)

Les "faits" (facts) dans Ansible sont des informations sur les hôtes distants, telles que le système d'exploitation, la version, etc. Nous pouvons obtenir des informations sur les hôtes en utilisant le module `setup`.

```bash
ansible all -i inventories/setup.yml -m setup -a "filter=ansible_distribution*"
```

## Playbooks

Les playbooks Ansible sont des fichiers YAML qui contiennent des tâches à exécuter sur les hôtes. Nous avons créé un playbook simple dans le fichier `playbook.yml` pour tester la connexion.

```yaml
- hosts: all
  gather_facts: false
  become: true

  tasks:
   - name: Test connection
     ping:
```

Nous pouvons exécuter ce playbook avec la commande `ansible-playbook`.

```bash
ansible-playbook -i inventories/setup.yml playbook.yml
```

## Déploiement Docker

Nous avons créé un playbook pour installer Docker sur le serveur distant. Ce playbook installe les dépendances nécessaires et configure Docker. Le playbook est situé dans le fichier `playbook.yml`.

Nous utilisons des rôles Ansible pour organiser notre configuration. Le rôle Docker a été créé en utilisant `ansible-galaxy`. Il contient les tâches nécessaires pour installer Docker.

```bash
ansible-galaxy init roles/docker
```

Nous pouvons appeler le rôle Docker dans notre playbook pour installer Docker.

```yaml
- hosts: all
  gather_facts: false
  become: true

  tasks:
    - name: Include Docker role
      include_role:
        name: install_docker
```

## Déploiement de l'Application

Le déploiement de l'application sur le serveur se fait en utilisant des rôles Ansible spécifiques. Nous avons créé des rôles pour chaque partie de l'application, tels que l'installation de Docker, la création d'un réseau, le lancement de la base de données, le lancement de l'application et le lancement du proxy.

Chaque rôle utilise le module `docker_container` pour lancer les conteneurs Docker nécessaires pour l'application. Les variables d'environnement sont également configurées dans les rôles.

## Déploiement Frontend

Pour le déploiement du frontend, nous avons configuré un serveur HTTP (Apache HTTPD) en tant que proxy pour rediriger le trafic vers l'API. Le frontend est hébergé sur ce serveur HTTP.

Le serveur HTTP est configuré pour rediriger les demandes vers l'API, assurant ainsi une communication fluide entre le frontend et l'API.

## Déploiement Continu

Nous avons configuré GitHub Actions pour effectuer un déploiement continu de l'application. Lorsqu'une nouvelle version est publiée sur la branche de production du référentiel GitHub, GitHub Actions déclenche le déploiement de l'application sur le serveur distant.

Le déploiement est effectué en se connectant au serveur distant à l'aide d'une clé privée chiffrée. Le déploiement est géré par un script Ansible qui lance l'application sur le serveur.

## Conclusion

Ce TP de DevOps a permis de mettre en place un processus d'automatisation du déploiement d'une application en utilisant Ansible. L'inventaire, les playbooks, les rôles et les modules Ansible ont été utilisés pour déployer l'application de manière efficace et reproductible. Le déploiement continu est assuré par GitHub Actions, garantissant que les mises à jour de l'application sont déployées automatiquement.
