# Database Tuto

Après installation, accéder au _shell_ de postgres :

```zsh
sudo service postgresql start
sudo su - postgres
```

Acceder au _prompt_ :

```zsh
psql

// ou bien //

psql -d postgres -U postgres
```

Ceci nous connecte avec l'utilisateur _admin_ (**postgres**)

Les commandes utiles pour le prompt :

- `\du` : Liste les utilisateurs et leur rôles
- `\conninfo` : Affiche les informations de connexion
- `\q` : Quitte le prompt
- `\c` : Connexion avec une nouvelle base de données
- `\dt` : Liste toutes les tables
- `\list` : Liste toutes les bases de données

## Enregistrer un nouvel utilisateur

```sql
postgres=# CREATE ROLE db_user WITH LOGIN PASSWORD 'strongpassword123';
postgres=# ALTER ROLE db_user CREATEDB;
```

**Pour toute commande dans le prompt il faut ajout un `;` à la fin de la ligne.**

Se connecter avec un autre utilisateurs :

```zsh
psql -d postgres -U db_user
```

## Créer une base de données

Avec l'utilisateur définit plus haut :

```sql
CREATE DATABASE first_db;
```

On peut se connecter directement à cette base de données :

```sql
psql -d first_db -U db_user
```

## Créer une table

```sql
 CREATE TABLE users(id SERIAL PRIMARY KEY, name VARCHAR(30)
, email VARCHAR(30));
```

Pour voir les noms des colonnes de la table :

```sql
\d+ users;
```

## Insérer des données dans la table

On peut ajouter autant de lignes que l'on veut en une commande :

```sql
INSERT INTO users(name,email) VALUES ('alice', 'alice@mail.com'), ('bob','bob@mail.com');
```

## Faire des requêtes sur cette table

Afficher toute la table :

```sql
SELECT * FROM users;
```

Afficher seulement un seul champ de la table :

```sql
SELECT name FROM users;
```

Ajouter des filtres sur les requêtes :

```sql
SELECT * FROM users;
```

## Modifier la table

Ajouter une colonne :

```sql
ALTER TABLE users ADD COLUMN age SMALLINT CHECK (age >= 0) DEFAULT 0;
```

Modifier les valeures de la table :

```sql
UPDATE users SET age = 20;
UPDATE users SET age = 25, email = 'bob@newmail.com' WHERE
 users.id = 2;
```

Supprimer des lignes :

```sql
DELETE FROM users WHERE users.email = 'proton@mail.com';
```

## Effacer une table ou la base de données

```sql
DROP TABLE users;

\c postgres
DROP DATABASE db_tuto;
```

## Commande utiles

Le _format string_ :

```sql
UPDATE users SET bio = FORMAT('My name is %s and my password is %s',users.name, users.password);
```

La longueur :

```sql
SELECT * FROM users WHERE LENGTH(users.password) > 3;
```

Afficher dans une certains ordre et limiter le nombre d'entrer :

```sql
SELECT * FROM users ORDER BY users.id DESC LIMIT 2;
```

---

## correction exo

```zsh
LENGTH(users.password) > 3;

FORMAT('Hello I am %s!', users.name)
ORDER BY -users.id;
ORDER BY users.id DESC;
ORDER BY users.id DESC LIMIT 2;
```
