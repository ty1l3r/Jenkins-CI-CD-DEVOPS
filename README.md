# Application DevOps Microservices & CI/CD

Bienvenue dans ce projet démonstration, conçu pour illustrer une architecture microservices et un pipeline CI/CD automatisé via Jenkins.

## Aperçu

Ce projet est constitué de deux microservices principaux :

- **Cast Service** : Gère les informations relatives aux castings (acteurs, rôles, etc.).
- **Movie Service** : Traite les données concernant les films (détails, descriptions, etc.).

Ces services fonctionnent de manière indépendante, permettant ainsi une meilleure scalabilité et facilité de maintenance.

## Fonctionnalités

- **Architecture Microservices** : Séparation des responsabilités entre les services pour une meilleure modularité.
- **Pipeline CI/CD Automatisé** : Intégration et déploiement continus via Jenkins qui automatise les builds, tests et déploiements.
- **Containerisation** : Chaque service est containerisé avec Docker, garantissant portabilité et cohérence entre les environnements.
- **Déploiement sur Kubernetes** : Utilisation de Helm Charts pour déployer et gérer les services sur un cluster Kubernetes, avec des configurations adaptées pour divers environnements (dev, QA, staging, prod).

## Technologies Utilisées

- **Jenkins** : Pour l'automatisation des pipelines CI/CD.
- **Docker** : Pour la containerisation des applications.
- **Kubernetes & Helm** : Pour l'orchestration et le déploiement des microservices.
- **FastAPI** (ou technologie similaire) : Pour la création d'API RESTful.
- **Python** : Langage principal pour le développement des services.

## Utilisation

1. **Clonage du Projet**
   Clonez le dépôt et accédez au répertoire :
   ```bash
   git clone https://github.com/votre-utilisateur/devops-jenkins-main.git
   cd devops-jenkins-main

2. **Configuration du Pipeline Jenkins**

Importez le Jenkinsfile situé à la racine du projet dans votre instance Jenkins.
Configurez les credentials et variables d'environnement nécessaires.

3. **Déploiement via Helm**

    Utilisez les Helm Charts présents dans le dossier charts/ pour déployer les services sur votre cluster Kubernetes.
Des charts spécifiques sont disponibles pour différents environnements (dev, QA, staging, prod).

4. **Exécution**
    Chaque commit dans le dépôt déclenche automatiquement une suite complète de builds, tests et déploiements via Jenkins.

