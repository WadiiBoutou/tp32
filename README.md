# TP 32 : Mise en place d'un pipeline CI/CD microservices avec Jenkins, GitHub, SonarQube, Docker Compose et Ngrok

## Description
Ce lab met en place un pipeline CI/CD complet pour une application Spring Boot en microservices (car, client, gateway, server_eureka). Le pipeline est orchestré par Jenkins : il récupère le code depuis GitHub, compile les services via Maven, exécute l'analyse de qualité via SonarQube, puis déploie l'ensemble avec Docker Compose. Comme Jenkins est en local, Ngrok expose temporairement une URL publique afin d'activer les webhooks GitHub. À la fin, chaque push sur GitHub déclenche automatiquement une exécution du pipeline et un déploiement des conteneurs.

## Architecture
L'application est composée de 4 microservices :
- **car** : Service de gestion des voitures
- **client** : Service de gestion des clients  
- **gateway** : Passerelle API
- **server_eureka** : Serveur de découverte Eureka

## Structure du projet
```
├── car/                    # Microservice Car
├── client/                 # Microservice Client
├── gateway/                # API Gateway
├── server_eureka/          # Eureka Discovery Server
├── deploy/                 # Configuration Docker Compose
└── sonarqube-compose.yml   # Configuration SonarQube
```

## Prérequis
- JDK 17
- Maven
- Git
- Docker + Docker Compose
- Jenkins
- Ngrok (compte + authtoken)
- Compte GitHub

## Démarrage rapide

### 1. Cloner le projet
```bash
git clone <repository-url>
cd <project-directory>
```

### 2. Démarrer SonarQube
```bash
docker compose -f sonarqube-compose.yml up -d
```

### 3. Déployer les microservices
```bash
cd deploy
docker-compose up -d --build
```

## Services et ports
- **SonarQube** : http://localhost:9999
- **Eureka Server** : http://localhost:8761
- **Gateway** : http://localhost:8888
- **Client Service** : http://localhost:8088
- **Car Service** : http://localhost:8095
- **phpMyAdmin** : http://localhost:8081

## Pipeline CI/CD
Le pipeline Jenkins exécute les étapes suivantes :
1. Clonage du dépôt GitHub
2. Build Maven des microservices
3. Analyse SonarQube (car et client)
4. Déploiement Docker Compose