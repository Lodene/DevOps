Pointez sur le document/rapport.

1-1 Documentez les éléments essentiels de votre conteneur de base de données : commandes et Dockerfile.
    Créer un container et associer une image :
        docker run --name nom_du_contenaire nom_de_image

    Créer une image :
        docker build -t nom_image . 

    Créer un network :
        `docker network create app-network`

    Associer un contenaire a un network :
        quand on crée un contenaire, il faut préciser cela :
        -network nom_network

        exemple :
        docker run --network app-network -p 5454:5432 -e POSTGRES_PASSWORD=pwd --name nom_contenaire nom_image_associé
        -> -p sert à préciser le port

    Créer un contenaire adminer avec le meme net-work que celui du contenaire de ta bdd :
        docker run --network app-network -p 8080:8080 --name nom_contenaire adminer
        -> ce n'est pas à nous de créer l'image adminer

    Créer un volume :
        docker volume create nom_du_volume
    
    Associer un volume a un contenaire :
        Cela se passe quand on crée le contenaire :
            docker run --network app-network -p 5454:5432 -e POSTGRES_PASSWORD=pwd --name TP1_container -v nom_du_volume:/var/lib/postgresql/data lodene/test


    Passons au Dockerfile :
    
    FROM postgres:14.1-alpine -> précise la biblioteque que l'on va utiliser

    ENV POSTGRES_DB=mydatabase -> nom de la table dans la bdd
    ENV POSTGRES_USER=myuser -> nom du user

    COPY SQL/createandinsert.sql /docker-entrypoint-initdb.d/ -> on copy le fichier SQL/createandinsert.sql vers le contenaire pour qu'il l'execute



1-2 Pourquoi avons-nous besoin d'une construction en plusieurs étapes ? Et expliquez chaque étape de ce fichier docker.
    La construction en plusieurs étapes dans un Dockerfile est un moyen astucieux d'obtenir une image Docker plus petite. Chaque étape de construction crée une sorte de "couche" dans l'image, et seulement les fichiers vraiment utiles à cette étape sont ajoutés à cette couche. Cela a pour effet de rendre l'image finale plus petite, car elle ne contient que ce dont elle a vraiment besoin, évitant ainsi d'emporter des fichiers et des dépendances inutiles.

    # Étape de construction
    FROM maven:3.8.6-amazoncorretto-17 AS myapp-build
    ENV MYAPP_HOME /opt/myapp
    WORKDIR $MYAPP_HOME

    # Copie de la configuration Maven et du code source
    COPY pom.xml .
    COPY src ./src

    # Construction de l'application en sautant les tests
    RUN mvn package -DskipTests

    # Étape d'exécution
    FROM amazoncorretto:17
    ENV MYAPP_HOME /opt/myapp
    WORKDIR $MYAPP_HOME

    # Copie du fichier JAR depuis la construction vers l'exécution
    COPY --from=myapp-build $MYAPP_HOME/target/*.jar $MYAPP_HOME/myapp.jar

    # Définition du point d'entrée pour exécuter le fichier JAR
    ENTRYPOINT java -jar myapp.jar


1-3 Document docker-compose les commandes les plus importantes. 
    docker login -> pour se login, a faire en premier
    docker tag my-database USERNAME/my-database:1.0 -> l'equivalent du commit sur github, le faire pour chaque image modifié
    docker push USERNAME/my-database:1.0  on push toutes les images qu'on a commit

1-4 Documentez votre fichier docker-compose.
    fait

1-5 Documentez vos commandes de publication et vos images publiées dans Dockerhub.
    PS C:\laragon\www\CPE\devOps\TP1\serv> docker tag lodene/bddd lodene/bdd:1.0
    PS C:\laragon\www\CPE\devOps\TP1\serv> docker push lodene/bdd:1.0
    faire ça pour chaque image (sans ou blier de se login au début)


Pourquoi mettons-nous nos images dans un dépôt en ligne ?
    Pour pouvoir le récupérer de n'importe ou si on a les codes d'authentification

2-1 Que sont les conteneurs de test ?
    bibliotheque java qui execute des conteneurs DOCKER pendant les tests


2-2 Documentez vos configurations d'actions Github.
    fait



NOTE DE COURS
    docker composer -> plus efficace que docker run car pas besoin de supprimer le contenaire quand on veut modifier qqc avec docker compose
    différence entre RUN et CMD dans le dockerfile -> Run se lance au début alors que CMD se lance lorsqu'on build  
    CI -> ... intégration
    CD -> ... ...


    declarative ->  on décrit ce que l'ont veut 
    impérative -> on dit ce qu'il doit faire (language c par exemple)

    superuser -> become: yes
