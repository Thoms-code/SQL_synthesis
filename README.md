# Synthèse MySQL
## Connexion
### Se connecter
```sql
mysql -u root -p
```
>```sql
>mysql
>```
>lance le terminal mysql
>```sql
>-u root
>ou
>-user root
>```
>identifie l'utilisateur (par default l'utilsateur est "root")
>```zsh
>-p
>ou
>-password
>VARIANTE(pas sécurisée)
>-pmotsdepasse
>ou
>-passwordmotdepasse
*puis écrire son mots de passe*
### Se déconnecter
```sql
exit
ou
exit()
```
### Créer un utilisateur
```sql
CREATE USER 'student'@'localhost' IDENTIFIED BY 'mot_de_passe';
```
> ```sql
> CREATE USER 'student'
> ```
> cette commande crée l'utilisateur _student._
>  ```sql
> @'localhost'
> ```
> Définit à partir d'où l'utilisateur peut se connecter. 
> Dans notre cas, 'localhost', donc il devra être connecté à partir de cet ordinateur.
> ```sql
> IDENTIFIED BY 'mot_de_passe'
> ```
> définit le mot de passe de l'utilisateur.

### Donner tout les privilèges a un utilisateur
```sql
GRANT ALL PRIVILEGES ON NOM_BASE.* TO 'student'@'localhost';
```
>```sql
> GRANT ALL PRIVILEGES
> ```
> cette commande permet d'attribuer tous les droits (c'est-à-dire insertions de données, sélections, modifications, suppressions…).
> ```sql
> ON nom_base.*
> ```
> définit les bases de données et les tables sur lesquelles ces droits sont acquis. Donc ici, on donne les droits sur la base "nomdebase" , pour toutes les tables de cette base (grâce à *).
>  ```sql
> TO 'student'@'localhost'
> ```
> définit l'utilisateur (et son hôte) auquel on accorde ces droits.

## DATABASE
### Conventions pour cette synthèse 
**Nommage des données:**
*Ce ne sont pas les conventions universelle, mais les mienne que j'utiliserai dans cette synthèse, il n'y a pas vraiment de convention universelle c'est comme on le souhaite mais il faut rester cohérent et s'y tenir.*
> **Base de données**
> Mettre en majuscule, séparé par un underscore si nécessaire:
> `LARAVEL ou FR_BE`

> **Tables de données:**
> Première lettre de chaque mots en majuscule, séparé par un underscore si nécessaire:
> `Animaux ou Personne_Fr`

> **Colonnes de données:**
> Premiere lettre du premier mots en majuscule, séparé par un underscore si nécessaire:
> `Espece ou Date_naissance`

> **Lignes de données:**
> Mettre en minuscule, séparé par un underscore si nécessaire:
> `chien ou 30_05_2018`

**Synthaxe des commande:**
> **Majuscule pour les requêtes:**
> Les commandes (requêtes) qui ne sont pas des données seront toujours écrite en majuscule:
> ```sql
> CREATE DATABASE NOM_BASE;
> --
> DESCRIBE Animal;
> ```

> **Commande optionelles:**
> Les commande qui ne sont pas necessaire pour le fonctionnement generale d'une commande sont entre `[ ]` crochets, elle servent en general a donner des precisions, mais peuvent etre obligatoire pour certaine manipulations:
> ```sql
>COMMANDE [OPTION] Nom_Table;
>```

> **Les requetes se terminent par un point-virgule:**
> ```sql
> SHOW WARNINGS;
> ```
### Créer un base de données
```sql
CREATE DATABASE NOM_BASE CHARACTER SET 'utf8';
```
>```sql
>CHARACTER SET 'utf8'
>```
>Permet de mettre la base de données au format "utf-8".
### Supprimer une base de données
```sql
DROP DATABASE NOM_BASE;
```
*si la base de donnée n'existe pas, mySQL affichera:*
 `ERROR 1008 (HY000) : Can't drop database 'NOM_BASE'; database doesn't exist`
>### variante
>```sql
>DROP DATABASE IF EXISTS NOM_BASE;
>```
>*dans ce cas si la base de données n'existe pas, mySQL affichera:*
>`Query OK, 0 rows affected, 1 warning (0.00 sec)`
>*pour afficher l'erreur il faut commander:*
>```sql
>SHOW WARNINGS;
>```
>*cela affiche un tableau:*
>| level | code | message|
>|--|--|--
>|Note|1008|Can't drop database 'NOM_BASE'; database doesn't exist
## Utilisation d'une base de données
**Accéder a la database**
```sql
USE NOM_BASE;
```
### Les Tables
**Créer une table**
*(exemple pour un élevage)*
```sql
CREATE TABLE Animal (
    id SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT,
    espece VARCHAR(40) NOT NULL,
    sexe CHAR(1),
    date_naissance DATETIME NOT NULL,
    nom VARCHAR(30),
    commentaires TEXT,
    PRIMARY KEY (id)
)
ENGINE=INNODB;
```
**Description d'une ligne**
```sql
id SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT,
```
>`id` : nom de la colonne.
>`SMALLINT`: type de colonne.
>` UNSIGNED`: colonne toujours positive.
>`NOT NULL`: ne peux pas être vide.
>`AUTO_INCREMENT`: s’incrémente a chaque donnée rajoutée (+1).
>`,`: fin de description de cette colonne.

**Vérification et affichage d'un table**
```sql
SHOW TABLES;
```
*liste les tables de la base de données*
```sql
DESCRIBE Animal;
```
*liste les colonnes de la table avec leurs caractéristiques*

**Modification d'une table**

la commande *ALTER*
```sql
ALTER TABLE Nom_table RENAME TO ... --permet de changer le nom de la table

ALTER TABLE Nom_table ADD ... -- permet d'ajouter quelque chose (une colonne par exemple)

ALTER TABLE Nom_table DROP ... -- permet de retirer quelque chose 

ALTER TABLE Nom_table CHANGE ...
ALTER TABLE Nom_table MODIFY ... -- permettent de modifier une colonne
```
Modification du nom de la table:
```sql
ALTER TABLE Nom_table
RENAME TO nouveau_Nom_table;
```
Ajout d'une colonne:
*(on va ajouter une colonne "date_insertion" de type DATE en exemple)*
```sql
ALTER TABLE Nom_table
ADD COLUMN date_insertion DATE NOT NULL;
```
Suppression d'un colonne:
*(on va supprimer la colonne date_insertion)*
```sql
ALTER TABLE Nom_table
DROP COLUMN date_insertion;
```
Modification d'une colonne:
Avec CHANGE:
(CHANGE peut modifier le nom **et** la description d'une colonne)
```sql
ALTER TABLE Nom_table
CHANGE ancien_nom nouveau_nom nouvelle_description;
```
Avec MODIFY:
(MODIFY ne peut changer **que** la description)
```sql
ALTER TABLE Nom_table 
MODIFY nom_colonne nouvelle_description;
```
Attention les descriptions doivent être complétement réécrite.

### Les Lignes de données
**Via la console**
```sql
INSERT INTO Nom_table (espece, sexe, date_naissance, nom) 
VALUES ('chien', 'F', '2008-12-06 05:18:00', 'Caroline'),
        ('chat', 'M', '2008-09-11 15:38:00', 'Bagherra'),
        ('tortue', NULL, '2010-08-23 05:18:00', NULL);
```
> **En détails:**
> `INSERT INTO Nom_table`: sélectionne la bonne table (littéralement "insérer dans Nom_table").
> `(espece, sexe, date_naissance, nom)`: se sont les nom des colonnes dans lesquels on va insérer nos données.
> `VALUES`: exprime que l'on va insérer des valeurs.
> `('chien', 'F', '2008-12-06 05:18:00', 'Caroline'),`: ce sont les valeurs que l'ont veut rajouter qui doivent correspondre a leurs type.
> > **Attention:**
	> il faut rajouter des données dans TOUTES les colonnes que l'on a mis entre parenthèse, si il y a une colomne ou je neux pas mettre de donnée je mets 'NULL'.

**Exécuter des commandes via un fichier.sql**
```sql
SOURCE monFichier.sql; -- si le fichier se trouve dans le repertoire.
-- ou
SOURCE monchemin\monFichier.sql; --monchemin a remplacer par le chemin vers le fichier .sql
```
**Insérer des données via un fichier .csv**
*le fichier .csv*
```csv
nom;prenom;date_naissance
Charles;Myeur;1994-12-30
Bruno;Debor;1978-05-12
Mireille;Franelli;1990-08-23
```

***Attention** le fichier doit se terminer par un saut de ligne.*

*Commande pour le lire dans mySQL:*
```sql
LOAD DATA [LOCAL] INFILE 'nom_fichier'
INTO TABLE Nom_table
[FIELDS
    [TERMINATED BY '\t']
    [ENCLOSED BY '']
    [ESCAPED BY '\\' ]
]
[LINES 
    [STARTING BY '']    
    [TERMINATED BY '\n']
]
[IGNORE Nombre LINES]
[(nom_colonne,...)];
```
> **En détails :**
> `LOCAL`: Spécifie que le fichier se trouve du coté client.
> `FIELDS`:Spécifie le format des colonnes.
> > **Obligatoire si `FIELDS` est utilisé:**
> > `TERMINATED BY`: Définit le caractère séparant les colonnes, entre guillemets. `'\t'` correspond à une tabulation par défaut.
> `ENCLOSED BY`: définit le caractère entourant les valeurs dans chaque colonne (vide par défaut).
> `ESCAPED BY`: Définit le caractère d'échappement pour les caractères spéciaux. Si par exemple vous définissez vos valeurs comme entourées d'apostrophes, mais que certaines valeurs contiennent des apostrophes, il faut échapper ces apostrophes "internes" afin qu'elles ne soient pas considérées comme un début ou une fin de valeur. Par défaut, il s'agit du \ habituel. Il faut lui-même l'échapper dans la clause.
> 
> `LINES`:Spécifie le format des lignes.
> > **Obligatoire si `LINES` est utilisé:**
> >`STARTING BY`:  Définit le caractère de début de ligne (vide par défaut).
> > `TERMINATED BY`: Définit le caractère de fin de ligne (`'\n'` par défaut).
> 
>`IGNORE nombre LINES`: Ignore un certain nombre de lignes. Par exemple, si la première ligne de votre fichier contient les noms des colonnes, vous ne voulez pas l'insérer dans votre table. Il suffit alors d'utiliser `IGNORE 1 LINES`.

## SÉLECTION DE DONNÉES
### Méthode SELECT
`SELECT` permet d'afficher les données d'un tableau.
*Syntaxe:*
```sql
SELECT  * FROM Nom_table;
```
