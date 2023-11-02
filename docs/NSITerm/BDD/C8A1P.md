Corrigé (partiel) de l'activité de découverte BDD
=======================================

## Question 5


```python
import csv


with open('./BDD/biblio.csv', newline='', encoding='utf8') as fp:
    table = [dict(ligne) for ligne in csv.DictReader(fp, delimiter=',')]

```

### Quels sont les livres édités chez Denoêl?


```python
for ligne in table:
    if ligne['Editeur'] == 'Denoël':
        print(ligne['Titre'])
```

    Espoir-du-cerf
    Les maîtres chanteurs
    Les maîtres chanteurs
    Espoir-du-cerf


#### Remarque

Dans le fichier, on a une occurence écrite différemment et donc n'apparait pas clairement ici: `Denoel` (sans tréma).

### Quels sont les livres écrits par Orson Scott Card?


```python
for ligne in table:
    if 'Card' in ligne['Auteur']:
        print(ligne['Titre'])
```

    Espoir-du-cerf
    Les maîtres chanteurs
    Les maîtres chanteurs
    Les maîtres chanteurs
    Espoir-du-cerf


#### Remarque

On est obligé de se limiter à la recherche de la sous chaîne `Card` pour ne pas louper des occurences.

### Quels sont les livres écrits par Friedrich Nietzsche?


```python
# Pb dû à l'orthographe
for ligne in table:
    if 'Nietzsche' in ligne['Auteur']:
        print(ligne['Titre'])
```

    La volonté de puissance
    Ainsi parlait Zarathoustra


#### Remarque

On loupe ici l'exemplaire n°5 car dans ce dernier le nom est tout en majuscule. On peut quand même s'en sortir, même si cela devient pénible:


```python
for ligne in table:
    if 'Nietzsche'.upper() in ligne['Auteur'].upper():
        print(ligne['Titre'])
```

    La volonté de puissance
    Ainsi parlait Zarathoustra
    Humain  trop humain


### Quels sont les livres écrits par David Brin?


```python
for ligne in table:
    if 'David Brin' in ligne['Auteur']:
        print(ligne['Titre'])
```

On a rien en sortie car dans le fichier on a `D. Brin`. Comme précédemment, on peut trouver une solution, pas très satisfaisante car il faut vérifier que le même orthographe a été utilisée partout. Et, qu'en est-il de l'exemplaire n° 13 ?


```python
for ligne in table:
    if 'D. Brin' in ligne['Auteur']:
        print(ligne['Titre'])
```

    Élévation
    Élévation


## Question 6

Il sera extrêmement difficile de changer `Jean Batiste` en `Jean Batiste Junior`. En effet, il faudrait examiner chaque ligne où ce prénom intervient et décider au cas par cas.  

Le système tel qu'il est présenté ne prévoit pas la gestion de livres avec des auteurs multiples.

## Question 7

Parmi les problèmes rencontrés, on peut citer:  

* la taille du fichier devient très importante avec le temps;
* le temps de traitement va devenir rédhibitoire dans la durée;
* la recherche est difficile;
* la mise à jour d'information peut être impossible;
* des problèmes peuvent être difficiles à résoudre: *Comment insérer un nouvel abonné qui n'a encore rien emprunté?*, ou encore: *Comment supprimer un livre sans risquer de supprimer un abonné?*.

## Question 8

Cette décomposition n'a pas engendré de pertes d'informations. En effet, on peut retrouver l'auteur d'un livre en faisant correspondre `idAu` de la table `EMPRUNTS` avec `idAu` de la table `AUTEURS`.  

Avantage: si on doit faire une modification sur le nom d'un auteur, on ne le fait qu'une fois dans la table `AUTEURS`.

## Question 9

**Exemples de code python associé aux requêtes demandées**


```python
import csv


with open('./BDD/EMPRUNTS_Q8.csv', newline='', encoding='utf8') as fp:
    t_emprunts = [dict(ligne) for ligne in csv.DictReader(fp, delimiter=',')]
with open('./BDD/AUTEURS.csv', newline='', encoding='utf8') as fp:
    t_auteur = [dict(ligne) for ligne in csv.DictReader(fp, delimiter=',')]
```


```python
def trouver_idauteur(auteur):
    prenom, nom = auteur
    for ligne in t_auteur:
        if ligne['Nom'] == nom and ligne['Prénom'] == prenom:
            return ligne['idAu']
```


```python
def chercher_livre(auteur):
    result = ''
    id_auteur = trouver_idauteur(auteur)
    for ligne in t_emprunts:
        if ligne['idAu'] == id_auteur:
            result = result + '\n' + ligne['Titre']
    return result
```


```python
print(chercher_livre(('Orson Scott', 'Card')))
```

    
    Espoir-du-cerf
    Les maîtres chanteurs
    Les maîtres chanteurs
    Les maîtres chanteurs
    Espoir-du-cerf



```python
print(chercher_livre(('David', 'Brin')))
```

    
    Élévation
    Rédemption
    Élévation



```python
print(chercher_livre(('Friedrich', 'Nietzsche')))
```

    
    La volonté de puissance
    Ainsi parlait Zarathoustra
    Humain  trop humain


## Question 10

Introduisons, comme suggéré, une table `LIVRES`:  

| idLi | Titre                        | 
|------|------------------------------| 
| 1    | La volonté de puissance      | 
| 2    | Espoir-du-cerf               | 
| 3    | Vendredi ou la vie sauvage   | 
| 4    | Élévation                    | 
| 5    | Ainsi parlait Zarathoustra   | 
| 6    | Humain  trop humain          | 
| 7    | Les maîtres chanteurs        | 
| 8    | St-Exupery  Terre des hommes | 
| 9    | Rédemption                   | 

La table `EMPRUNTS` sera modifiée comme suit:  

| id Ex | idLi | idAu | Editeur   | Nom      | Prenom     | tel | Date-emp | Date-ret | 
|-------|------|------|-----------|----------|------------|-----|----------|----------| 
| 1     | 1    | 1    | Gallimard |          |            |     |          |          | 
| 2     | 2    | 2    | Denoël    | Michel   | Tom        |     | 20/10/08 | 07/11/08 | 
| 3     | 3    | 3    | Poche     | Moreau   | J. Batiste |     | 02/10/09 |          | 
| 4     | 4    | 4    | J’AI LU   | Laurent  | Camille    |     | 02/04/06 | 20/04/06 | 
| 5     | 3    | 3    | Poche     | Moreau   | J. Batiste |     | 02/10/09 |          | 
| 6     | 3    | 3    | Poche     |          |            |     |          |          | 
| 7     | 5    | 1    | Poche     |          |            |     |          |          | 
| 8     | 6    | 1    | POCHE     |          |            |     |          |          | 
| 9     | 7    | 2    | Denoël    | Moreau   | J. Batiste |     | 08/10/09 |          | 
| 10    | 7    | 2    | Denoël    |          |            |     |          |          | 
| 11    | 7    | 2    | Denoël    |          |            |     |          |          | 
| 12    | 8    | 5    | Broché    |          |            |     |          |          | 
| 13    | 9    | 4    | J’ai lu   |          |            |     |          |          | 
| 2     | 2    | 2    | Denoël    | Roux     | Sarah      |     | 20/11/09 |          | 
| 4     | 2    | 4    | J’AI LU   | Dubois   | Mathis     |     | 25/11/09 |          | 


## Question 11

On peut adopter la décomposition suivante:

### Table EMPRUNTS

| idEx | idLi | idAu | idEd | idAb | Date-emp | Date-ret | 
|-------|------|------|------|------|----------|----------| 
| 1     | 1    | 1    | 1    |      |          |          | 
| 2     | 2    | 2    | 2    | 1    | 20/10/08 | 07/11/08 | 
| 3     | 3    | 3    | 3    | 2    | 02/10/09 |          | 
| 4     | 4    | 4    | 4    | 3    | 02/04/06 | 20/04/06 | 
| 5     | 3    | 3    | 3    | 4    | 02/10/09 |          | 
| 6     | 3    | 3    | 3    |      |          |          | 
| 7     | 5    | 1    | 3    |      |          |          | 
| 8     | 6    | 1    | 3    |      |          |          | 
| 9     | 7    | 2    | 2    | 2    | 08/10/09 |          | 
| 10    | 7    | 2    | 2    |      |          |          | 
| 11    | 7    | 2    | 2    |      |          |          | 
| 12    | 8    | 5    | 5    |      |          |          | 
| 13    | 9    | 4    | 4    |      |          |          | 
| 2     | 2    | 2    | 2    | 5    | 20/11/09 |          | 
| 4     | 2    | 4    | 4    | 6    | 25/11/09 |          | 


### Table EDITEURS

| idEd | Nom       | 
|------|-----------| 
| 1    | Gallimard | 
| 2    | Denoël    | 
| 3    | Poche     | 
| 4    | J’ai lu   | 
| 5    | Broché    | 


### Table ABONNES

| idAb | Nom     | Prenom       | Tel | Ad | 
|------|---------|--------------|-----|----| 
| 1    | Michel  | Tom          |     |    | 
| 2    | Moreau  | Jean Batiste |     |    | 
| 3    | Laurent | Camille      |     |    | 
| 4    | Moreau  | Jean Batiste |     |    | 
| 5    | Roux    | Sarah        |     |    | 
| 6    | Dubois  | Mathis       |     |    | 


Les tables `LIVRES` et `AUTEURS`restent inchangées par rapport à la question 10.

## Question 12

On remarque que les informations liées à un livre peuvent apparaître plusieurs fois (lorsque le livre est emprunté à nouveau). Cela se produit par exemple pour l'exemplaire `idEx = 2`.
