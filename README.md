# Jenkins Pipeline for Docker and Kubernetes Deployment

## Description

Ce projet automatise le processus de construction, de test et de déploiement d'une application containerisée en utilisant Docker, Jenkins et Kubernetes. Il suit un pipeline CI/CD structuré qui comprend les étapes suivantes :

- Préparation du projet : Le pipeline commence par récupérer la dernière version du code source depuis le dépôt GitHub.
- Construction de l'image Docker : Il crée une image Docker basée sur le Dockerfile présent dans le dépôt. Cette étape inclut la suppression de tout conteneur Jenkins existant et la construction d'une nouvelle image avec un tag basé sur l'identifiant de la construction Jenkins (BUILD_ID).
- Exécution du conteneur Docker : Après la construction de l'image, un conteneur est démarré à partir de l'image créée. Le conteneur expose l'application sur le port 80.
- Test d'acceptation : Le pipeline effectue un test de validation de l'application en exécutant une commande curl pour vérifier que l'application répond correctement sur le serveur local.
- Poussée de l'image Docker vers Docker Hub : L'image construite est ensuite poussée sur Docker Hub en utilisant les identifiants de connexion sécurisés stockés dans Jenkins.
- Déploiement sur Kubernetes (en Dev, Staging et Production) :
- Déploiement en Dev : L'image Docker est déployée dans un environnement de développement à l'aide de Helm, après avoir configuré le fichier values.yml et mis à jour le tag de l'image Docker.
- Déploiement en Staging : Le même processus de déploiement est effectué pour l'environnement de staging.
- Déploiement en Production : Avant de déployer en production, une validation manuelle est requise pour garantir que le déploiement est approuvé. Après validation, l'application est déployée en production sur Kubernetes.

## Fonctionnement du Pipeline

Le pipeline Jenkins se compose de plusieurs étapes, chacune ayant un rôle spécifique dans le processus de CI/CD :

- Checkout : Récupère le code source depuis GitHub.
- Docker Build : Crée une image Docker à partir du code source.
- Docker Run : Lance un conteneur à partir de l'image Docker pour tester l'application.
- Test Acceptance : Vérifie que l'application fonctionne correctement en utilisant curl pour tester l'accès à l'application via HTTP.
- Docker Push : Pousse l'image Docker sur Docker Hub pour faciliter le partage et le déploiement.
- Kubernetes Deployment :
- Déploie l'application dans un environnement Dev, Staging, puis Production en utilisant Helm et un fichier values.yml.

## Prérequis

- Jenkins avec accès aux Docker Hub credentials et Kubernetes secrets.
- Kubernetes configuré avec kubectl et accès aux clusters de développement, staging et production.
- Docker installé pour construire et pousser les images.
- Helm installé pour gérer les déploiements sur Kubernetes.

## Variables d'Environnement

Les variables d'environnement suivantes sont utilisées dans le pipeline :

- DOCKER_ID : Identifiant Docker de l'utilisateur.
- DOCKER_IMAGE : Nom de l'image Docker.
- DOCKER_TAG : Tag de l'image basé sur l'identifiant de la construction.
- KUBECONFIG : Fichier de configuration Kubernetes stocké dans les secrets Jenkins pour accéder aux clusters Kubernetes.

## Sécurité

Le pipeline utilise des credentials Jenkins pour sécuriser les informations sensibles comme les identifiants Docker Hub et la configuration Kubernetes.

## Conclusion

Ce pipeline Jenkins permet une intégration et un déploiement continus (CI/CD) pour une application basée sur Docker et Kubernetes, assurant une gestion automatisée du cycle de vie de l'application à travers plusieurs environnements.

## Auteur
Nelly G.
