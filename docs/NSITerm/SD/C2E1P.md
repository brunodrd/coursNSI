## Exercice 1

Reprendre la structure de pile du cours implémentée avec des `list` *de python* et ajouter à l'interface deux opérations:  

* `vider(p)` qui vide une pile `p`;
* `taille(p)` qui renvoie la taille de la pile `p`.


```python
def creer_pile():
    return []

def est_pilevide(p):
    return len(p) == 0

def depiler(p):
    # A compléter
    assert not est_pilevide(p), 'Erreur pile vide'
    v = p.pop()
    return v
    
def empiler(p, val):
    # A compléter
    p.append(val)
    
def sommet(p):
    # A compléter
    assert not est_pilevide(p), 'Erreur pile vide'
    return p[-1]

def vider(p):
    p.clear()
    
def taille(p):
    return len(p)
```


```python
p1 = creer_pile()
empiler(p1, 10)
empiler(p1, 1)
empiler(p1, 5)
empiler(p1, 4)
est_pilevide(p1)
```




    False




```python
print(sommet(p1))
vider(p1)
print(est_pilevide(p1))
```

    4
    True


## Exercice 2 - Notation polonaise inverse

L'*écriture polonaise inverse* des expressions arithmétiques place l'opérateur **après** ses opérandes.  
Cette notation ne nécessite aucune parenthèse, ni aucune règle de priorité. Ainsi l'expression polonaise inverse décrite par la chaîne de caractères  

`1 2 3 * + 4 *`  

désigne l’expression traditionnellement notée $(1+ 2 \times 3)\times 4$.  
La valeur d’une telle expression peut être calculée facilement en utilisant une pile pour stocker les résultats intermédiaires. Pour cela, on parcours un à un les éléments de l'expression et on effectue les actions suivantes:  

* si on trouve un nombre, on l'empile;
* si on trouve un opérateur binaire, on dépile le dernier et l'avant dernier nombre, on leur applique l’opérateur, et on empile résultat.

Si l'expression était bien écrite, il y a bien toujours deux nombres sur la pile lorsque l’on voit un opérateur, et à la fin du processus il **reste exactement un nombre sur la pile, qui est le résultat**.  

Écrire une fonction prenant en paramètre une chaîne de caractères représentant une expression en notation polonaise inverse composée d’additions et de de multiplications de nombres entiers et renvoyant la valeur de cette expression.  

*On supposera que les éléments de l'expression sont séparés par des espaces. Attention, cette fonction ne doit pas renvoyer de résultat si l'expression est mal écrite*.


```python
def npi(c):
    exp = c.split()
    p = creer_pile()
    for elt in exp:
        print(f'----Examen de {elt}')
        if elt.isdigit():
            empiler(p, int(elt))
            print(f"J'ai empilé {int(elt)}")
            print(p)
        elif elt in ('+', '*'):
            a = depiler(p)
            print(f"J'ai dépilé {a}")
            b = depiler(p)
            print(f"J'ai dépilé {b}")
            r = a + b if elt == '+' else a * b
            empiler(p, r)
            print(f"J'ai empilé {r}")
        else:
            raise ValueError
        print()
    assert taille(p) == 1, 'Mal écrit'
    return sommet(p)
            
```


```python
npi('1 2 + 3 * 2 + 4 *')
```

    ----Examen de 1
    J'ai empilé 1
    [1]
    
    ----Examen de 2
    J'ai empilé 2
    [1, 2]
    
    ----Examen de +
    J'ai dépilé 2
    J'ai dépilé 1
    J'ai empilé 3
    
    ----Examen de 3
    J'ai empilé 3
    [3, 3]
    
    ----Examen de *
    J'ai dépilé 3
    J'ai dépilé 3
    J'ai empilé 9
    
    ----Examen de 2
    J'ai empilé 2
    [9, 2]
    
    ----Examen de +
    J'ai dépilé 2
    J'ai dépilé 9
    J'ai empilé 11
    
    ----Examen de 4
    J'ai empilé 4
    [11, 4]
    
    ----Examen de *
    J'ai dépilé 4
    J'ai dépilé 11
    J'ai empilé 44
    





    44



## Exercice 3 - Chaînes bien parenthésées

On considère considère une chaîne de caractères incluant à la
la fois fois des parenthèses rondes `(` et `)` et des parenthèses carrées `[` et `]` . La chaîne est bien parenthésée si chaque ouvrante est associée à une unique fermante de même forme, et réciproquement.  

Écrire une fonction prenant en paramètre une chaîne de caractères contenant, entre autres, les parenthèses décrites et qui renvoie `True` si la chaîne est bien parenthésée et `False` sinon.


```python
par = {'(': ')', '[': ']'}

def est_bien_parent(c):
    p = creer_pile()
    for elt in c:
        if elt in par:
            empiler(p, elt)
        elif elt in par.values():
            s = depiler(p)
            if s not in par or par[s] != elt:
                return False
    return taille(p) == 0
            
            
```


```python
est_bien_parent('1*3+(2-[2/5])')
```




    True



## Exercice 4 - File bornée


On propose ici une autre réalisation possible d'une file bornée à partir d'un tableau.  La file sera repérée par deux index `tete` et `queue`. On enfile par la queue et défile par la tête.  

Soit $N$ la capacité de la file. On prévoit un tableau de taille $N+3$ car on souhaite stocker également:

* la position de la tête (index 0);  
* la position de la queue (index 1);  
* la longueur effective de la file (index 3)  

La position de la queue indique l'endroit où sera enfilée la prochaine donnée.  
La figure (a) ci-dessous montre une file `f` de ce type et de capacité maximale 7, dans laquelle figurent déjà 4 éléments. A partir de cette situation et après les opérations:  

```python
defiler(f)
defiler(f)
enfiler(f, 4)
enfiler(f, 17)
enfiler(f, 10)
```
on se trouve dans la situation (b).

![file](img/file_tab.png)



```python
MAXFILE = 8 # Capacité de la pile


def creer_file(n=MAXFILE):
    return [3, 3, 0] + [None] * n

def enfiler(f, x):
    assert not est_pleine(f), 'Erreur: file pleine'
    
    queue, taille = f[1], f[2] # Pour la lisibilité
    
    f[queue] = x
    f[2] = taille + 1 # la file comporte un elt supplémentaire    
    # Déplacement de la queue - modèle circulaire
    if queue == len(f) -1:
            queue = 3
    else:
        queue += 1
    f[1] = queue

def est_pleine(f):
    return f[2] == len(f) - 3
                                   
def est_vide(f):
    return f[2] == 0

def defiler(f):
    assert not est_vide(f), 'Erreur: file vide'
    
    tete = f[0]
    v = f[tete]
    f[tete] = None # Optionnel
    f[2] -= 1 # la file a un elt de moins
    # Déplacement de la tete - modèle circulaire
    if tete == len(f) - 1:
        tete = 3
    else:
        tete += 1
    f[0] = tete
    return v
```


```python
f = creer_file()
#f[0], f[1], f[2] = 4, 8, 4
#f[4], f[5], f[6], f[7] = 15, 6, 9, 8
print(f)
enfiler(f, 3)
print(f)
enfiler(f, -1)
print(f)
enfiler(f, 100)
print(f)
enfiler(f, 8)
```

    [3, 3, 0, None, None, None, None, None, None, None, None]
    [3, 4, 1, 3, None, None, None, None, None, None, None]
    [3, 5, 2, 3, -1, None, None, None, None, None, None]
    [3, 6, 3, 3, -1, 100, None, None, None, None, None]


1. A partir des informations disponibles:  
  * dire combien d'opérations sont autorisées sur cette structure;
  * rédiger leur spécification.
  
2. Prévoir les réponses de l'interpréteur python après les séquences de commandes suivantes (*qui se suivent!*):  

Séquence 1
```python
f1 = creer_file()
est_filevide(f1)
```  

Séquence 2
```python
enfiler(f1, 5)
est_filevide(f1)
```

Séquence 3
```python
enfiler(f1, -10)
enfiler(f1, 25)
enfiler(f1, -35)
while not est_filevide(f1):
    print(defiler(f1), end=' ')
```

Séquence 4
```python
f2 = creer_file(3)
enfiler(f2, 10)
enfiler(f2, 100)
enfiler(f2, 1000)
enfiler(f2, 50)
```

3. Proposer une implémentation pour `enfiler` et `defiler`.


```python
f = creer_file(5)
print(f)
enfiler(f, 5)
#print(f)
enfiler(f, 10)
#print(f)
enfiler(f, -1)
#print(f)
print(est_pleine(f))
defiler(f)
#print(f)
defiler(f)
#print(f)
defiler(f)
print(f)
```

    [3, 3, 0, None, None, None, None, None]
    False
    [6, 6, 0, None, None, None, None, None]



```python
f1 = creer_file()
est_filevide(f1)
```




    True




```python
enfiler(f1, 5)
est_filevide(f1)
```




    False




```python
enfiler(f1, -10)
enfiler(f1, 25)
enfiler(f1, -35)
while not est_filevide(f1):
    print(defiler(f1), end=' ')
```

    5 -10 25 -35 


```python
f2 = creer_file(3)
enfiler(f2, 10)
enfiler(f2, 100)
enfiler(f2, 1000)
enfiler(f2, 50)
```


    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-15-c43f6bea7b7e> in <module>
          3 enfiler(f2, 100)
          4 enfiler(f2, 1000)
    ----> 5 enfiler(f2, 50)
    

    <ipython-input-11-a90013406bf0> in enfiler(f, x)
          6 
          7 def enfiler(f, x):
    ----> 8     assert not est_pleine(f), 'Erreur: file pleine'
          9 
         10     queue, taille = f[1], f[2] # Pour la lisibilité


    AssertionError: Erreur: file pleine



```python

```
