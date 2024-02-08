NOTE DE COURS
    docker composer -> plus efficace que docker run car pas besoin de supprimer le contenaire quand on veut modifier qqc avec docker compose
    différence entre RUN et CMD dans le dockerfile -> Run se lance au début alors que CMD se lance lorsqu'on build  
    CI -> Continuous Integration -> verification backend et les dev n'ont plus qu'a faire des push régulierement pou maintenir le projet pret
    CD -> C'est la pratique de garantir que le code est toujours prêt à être déployé vers un environnement de production. Cela implique généralement des pipelines d'intégration continue et des tests automatisés

    declarative ->  on décrit ce que l'ont veut 
    impérative -> on dit ce qu'il doit faire (language c par exemple)

    superuser -> become: yes