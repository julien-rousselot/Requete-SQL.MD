# Challenge : Découverte SQL

### Auteurs

```sql
    SELECT name
    FROM author;

    SELECT name
    FROM author
    WHERE id = 2;
```

### Articles

```sql
    SELECT name
    FROM category;

    SELECT name
    FROM category
    WHERE id = 1;
```

## Bonus SQL

### Auteurs

##### Lister les auteurs par ordre alphabétique (sur le nom)
```sql
    SELECT name
    FROM author
    ORDER BY name;
```
##### Lister les auteurs dont l'e-mail contient @gmail.com 
```sql
    SELECT *
    FROM author
    WHERE email LIKE "%@gmail.com";
```
##### Lister les auteurs qui n'ont pas d'image de profil 
```sql
    SELECT name
    FROM author
    WHERE image IS NULL;
```

### Articles

##### Lister le titre et la date de publication de tous les articles 
```sql
    SELECT title, published_date 
    FROM post
```
##### Afficher le titre d'un article en particulier (en passant l'id de l'article)
```sql
    SELECT title 
    FROM post
    WHERE id = 2;
```
##### Lister les 2 derniers articles publiés 
```sql
    SELECT id, title, resume, content, published_date
    FROM post
    ORDER BY published_date DESC
    LIMIT 2;
```
##### Compter le nombre d'articles (doc) et renommer la colonne en nbArticles
```sql
    SELECT COUNT(id) AS "nbArticles"
    FROM post;
```

# MCD
## Cardinalités et Relations
Sur un MCD, on ne trouve pas de clé étrangères => à la place, on a des bulles de relations avec des cardinalités.

Le MCD sert de shéma pour nous aider à concevoir la BDD.

A terme, il va falloir la "traduire" en BDD, et donc créer des liaisons entre certaines tables.

Pour savoir où mettre les clé étrangères, il faut regarder les cardinalités => elles donnent le détail des relations.

## 3 types de relations
On ne regarde que le 2eme chiffres de chaque cardinalité (le chiffre correspondant au max).

### One To Many (1:n)
Règle : la clé étrangère sera obligatoirement du coté du One.

### One To One (1:1)
Règle : A priori on peut fusionner les 2 tables. Au pire du pire, on peut décider de vraiment séparer les 2 tables, et dans ce cas, on a le choix pour mettre la clé étrangère dans l'une, ou l'autre, ou les deux.

### Many To Many (n:n)
Règle : On crée une table de liaison qui contiendra une clé étrangère référant aux 2 tables => permet de faire toutes les combinaisons possibles.

## signification 1,N 
=> 1 est le minimum et N le maximum



# Requete SQL sur PHP 



1. on se connecte à la base de données SQL existante
2. en utilisant PDO, un outil proposé par PHP
3. ca nous permettra par la suite de lancer des requêtes SQL
4. on en récupérer les résultats

```
$pdo = new PDO (
    'mysql:dbname=videogame;host=127.0.0.1',
    'explorateur',
    '6q595XmCKm'
    $options = [PDO::ATTR_ERRMODE => PDO::ERRMODE_WARNING];
); 
```

#### $dataSourceName doit contenir :
*   le nom du pilote/driver (mysql, sqlite, oracle, etc)
*  suivi de deux points :
*  suivi du nom de la base de données (dbname=nom de la bdd)
*  suivi d'un point virgule ;
*  suivi de l'adresse du serveur SQL (host=url de la bdd)

#### $user contient le login de l'utilisateur SQL
* il faut donc que cet utilisateur SQL ait les droits
* d'accès sur la base de donnée cible

#### $password contient le mot de passe de l'utilisateur SQL

#### $options contient des .. options .. optionnelles (sic)
 ici on active l'affichage des erreurs liées aux requêtes
* grâce à PDO::ERRMODE_WARNING
* sur un environnement de Prod c'est plutôt : PDO::ERRMODE_SILENT
* qui permet de masquer les erreurs liées aux requêtes => faut pas donner trop d'info sur la DB à n'importe qui

 ``` 
 try {
    // tout le code contenu dans le try { } sera exécuté par PHP mais si jamais une erreur survient
    // PHP stoppe l'exécution du code contenu dans le try { }, à la ligne qui provoque l'ereur,
    // et se met alors à exécuter le code contenu dans le catch { }


    // on se connecte à SQL en instanciant un objet, à partir de la classe PDO
    $pdo = new PDO($dataSourceName, $user, $password, $options);

    // si la connexion est réussie, on obtient un objet PDO
    var_dump($pdo);

    // Par exemple,
    // pour lister tous les authors de la bdd oblog, on créé la requête via Adminer 
    // et on la copie/colle comme simple chaîne de caractères dans PHP :

    $sql = "SELECT * FROM author";

    $pdoStatement = $pdo->query($sql);

    if ($pdoStatement !== false) {
        echo "La requête a bien été exécutée !";

        // Maintenant on veut récupérer nos données

        // par défaut, fetchAll() doublonne volontairement
        // les données issus de la requête
        //
        // le format est le suivant :
        //
        // | id | 0 | name    | 1       | image    | 2        | email             | 3                 |
        // |----|---|---------|---------|----------|----------|-------------------|-------------------|
        // | 1  | 1 | Vincent | Vincent | vinz.jpg | vinz.jpg | vincent@yahoo.com | vincent@yahoo.com |
        // | 2  | 2 | Julie   | Julie   | NULL     | NULL     | julie@gmail.com   | julie@gmail.com   |
        //
        // un format pas hyper pratique, faut le dire
        // mais heureusement on peut changer le mode de fetchAll()
        // https://www.php.net/manual/fr/pdostatement.fetchall.php

        $result = $pdoStatement->fetchAll(PDO::FETCH_ASSOC);
        var_dump($result);
    } else {
        echo "La requête de récupération des auteurs n'a pas pu être exécutée : <br>";
        // on peut alors récupérer et afficher
        // la dernière erreur survenue
        print_r($pdo->errorInfo());
    }

    $pdoStatement = $pdo->query("SELECT * FROM `author` WHERE `id` = 4");
    if ($pdoStatement !== false) {
        echo "La requête a bien été exécutée !";

        $result = $pdoStatement->fetch(PDO::FETCH_ASSOC);
        var_dump($result);
    } else {
        echo "La requête n'a pas pu être exécutée : <br>";
        // on peut alors récupérer et afficher
        // la dernière erreur survenue
        print_r($pdo->errorInfo());
    }

    // on veut insérer 2 nouveaux authors
    // on peut créer et tester la requête d'abord dans Adminer
    // puis on la copie/colle ici
    $sql = "INSERT INTO author (name, image, email) VALUES
        ('Albert', 'albert.jpg', 'albert@camarail.com'),
        ('Robert', 'robert.jpg', 'robert@camarail.com')";
    $nbInsertedRows = $pdo->exec($sql);
    var_dump($nbInsertedRows);

    // On veut modifier l'avatar de tous les Albert en mettant albert-a-la-plage.jpg
    $sql = "UPDATE author
            SET image = 'albert-a-la-plage.jpg',
                email = 'albertALP@camarail.com'
            WHERE name = 'Albert'";
    $nbUpdatedRows = $pdo->exec($sql);
    var_dump("Nombre de lignes modifiées : " . $nbUpdatedRows);

    // On veut supprimer les Albert et Robert
    $sql = "DELETE FROM author WHERE name = 'Albert' OR name = 'Robert'";
    $nbDeletedRows = $pdo->exec($sql);
    var_dump("Nombre de lignes supprimées : " . $nbDeletedRows);
} catch (PDOException $exception) {
    // une erreur est survenue dans le try { }
    // donc PHP exécute dorénavant le code contenu dans ce catch { }

    // debug
    exit("La connexion à SQL via PDO a échoué : <br>".$exception->getMessage());
}
