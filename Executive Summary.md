
# Brief 7: executive summary

![](https://i.imgur.com/9rFM7io.png)

---

## Objectifs

----

- utiliser kubernetes et aks
- déployer application de vote
- BDD redis
- stockage persistant pour la BDD
- zone DNS
- application gateway via ingress

----

<section data-visibility="hidden">
## installation de kubctl

- sudo `apt-get install -y kubect`
- création d'un groupe de ressource azure
- création d'un cluster aks `az aks create -g DMRG -n DMAKS --ssh-key-value /chemin/de/la/clé --node-count 2 --enable-managed-identity -a ingress-appgw --appgw-name myApplicationGateway --appgw-subnet-cidr "10.225.0.0/16"`
- connexion de kubectl et aks `az aks get-credentials --resource-group DMRG --name DMAKS`
</section>

---

## ressources azure
- azure kubernetes service
    - 2 nodes
    - ingress application gateway

---

## création des templates k8s

----

- storage
    - storageclass
    - PersistentVolumeClaim
- redis
    - deployment redis
    - clusterIP redis
- ingress
    - ingress azure application gateway
- voting app
    - deployment voteapp
    - clusterIP voteapp
    - HorizontalPodAutoscaler

---

## déploiement du cluster

Déploiement depuis microsoft PipeLine. toutes les heure un job est éxécuté, il vérifie si une nouvelle mise à jours a été publié sur docker hub et si c'est le cas, le manifest kubernetes est redéployé.

---

## explication k8s

- open-source
- orchestration de conteneurs
- configuration déclarative
- gestion d'applications à grandes échelles
- déploiement de pods et de services sur des nodes
- yaml/Json -> contrôleur -> nodes
