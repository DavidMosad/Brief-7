# DAT Brief 7
## architecture

![](https://i.imgur.com/9rFM7io.png)


## Objectifs

- Déploiement automatique d'une application de vote pour correspondre aux dernières MAJ de l'application sur Dockerhub
- Utilisation de Kubernetes et Azure pour le déploiement de l'application
- BDD Redis avec stockage persistant
- Création d'un DNS qui passe par une Application Gateway pour se connecter a Kubernetes
- Cluster Kubernetes relié par un ingress à l'application Gateway
- Mise en place d'une pipeline en utilisant Azure DevOps

 
## Eléments déployés par Kurbernetes


- storage
    - storageclass
        - azure disk
    - PersistentVolumeClaim
        - 8Gi
        - read write once
- redis
    - deployment redis
        - replicas: 1
        - volume
        - port 6379
        - env:
            - REDIS_PWD
            - ALLOW_EMPTY_PASSWORD:no
    - clusterIP redis
        - port: 6379
- ingress
    - ingress azure application gateway
        - tls
        - azure apllication gateway
- voting app
    - deployment voteapp
        - replicas: 2
        - port: 80
        - resources:
            - request, cpu: 250m
            - limits, cpu: 500m
        - env:
            - REDIS
            - STRESS-SECS: 2
            - REDIS_PWD
    - clusterIP voteapp
        - port 80
    - HorizontalPodAutoscaler
        - max replicas: 8
        - min replicas: 2
        - scale target ref: voteapp
        - CPU utilisation %: 70
## FQDN
aks2.university-of-common-sense.space

## éléments déployés sur azure

- azure kubernetes service
    - 2 nodes
    - ingress application gateway

## PipeLine
 - utilisation de Azure Pipeline
        - simplicité d'utilisation,
        - gratuit,
        - doit être utilisé en entreprise
 
 ## Cost forcast
  - 1 AKS...........................Pay as you go: €69.46
      - 1 nodepool
          - 2 node: Standard B2s
      - 1 SSD premium: 32 Gb
 - blob storage.....................Pay as you go:€0.08
     - blob storage: 1Go
     - 10 000 writing operation
     - 10 000 read operation
 
Total €69.53
    
