# DevOps

## Module DevOps S8 - TP2 - Travis CI 

* Création de .travis.yml
```
language: java

# services:
#    - docker
   
# before_install:
#     - docker pull postgres:9.6
#     - docker run -d -p 5432:5432 postgres
   
script: 
    - mvn clean verify
```
* Après avoir setup Travis et mis en place le repo Git, on observe dans Travis CI qu'un job a été lancé, directement à partir de notre fichier .travis.yml 

![40% center](travis.png)

* Le log nous montre que le mvn-clean-verify a été correctement terminé et que les tests ont été réalisés avec succès

![40% center](mvn-clean-verify.png)

![40% center](tests-travis.png)

* Dans travis CI, lorsqu'on saisit notre docker_username et notre password, il est enregistré en "sécurisé" et en masqué car ces informations risqueraient d'être affichées dans le log

* Le contenu du fichier .travis.yml est modifié puisque l'on souhaite utiliser docker : 
```
   language: java

   services:
      - docker

   #env:
   #    - DOCKER_COMPOSE_VERSION=1.4.2

   script: 
       - mvn clean verify
       - mvn package -DskipTests
       #build
       - docker build -t bilge98/sample-application-db-changelog-job sample-application-db-changelog-job/
       - docker build -t bilge98/sample-application-http-api-server/http sample-application-http-api-server/
       #connexion
       - docker login --username=$DOCKER_USERNAME --password=$PASSWORD
       #push
       - docker push bilge98/sample-application-db-changelog-job
       - docker push bilge98/sample-application-http-api-server 
```
