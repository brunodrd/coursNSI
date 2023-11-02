# Description du projet
Il s'agit de modéliser une partie de *bataille* aux cartes.  
Les règles du jeu de bataille sont les suivantes (voir [wikipedia](https://fr.wikipedia.org/wiki/Bataille_(jeu))): il s'agit d'un jeu à deux joueurs où l'on commence par distribuer l'ensemble d'un jeu de cartes (*52 cartes*) aux joueurs de manière à ce que chaque joueur possède le même nombre de cartes, c'est-à-dire 26.  

Les joueurs **ne regardent pas les cartes** et à chaque tour, ils retournent la carte se trouvant sur le **dessus** du paquet. Le joueur possédant la carte la plus forte récupère les deux cartes et les place **sous** son tas (*cela doit rappeler une structure de donnée déjà étudiée* :cold_sweat:).  
Si les deux cartes sont de valeur égale, on dit qu'il y a **bataille** : **chaque joueur place la carte du dessus de son tas à l'envers sur sa première carte, puis place une nouvelle carte par dessus**. Le joueur ayant la carte la plus forte récupère l'ensemble des cartes (*en cas de nouvelle égalité, on recommence*).  

La partie se termine lorsque l'un des deux joueurs n'a plus de carte (et ce dernier a perdu). 
# Modélisation
Plusieurs modélisations sont possibles. En voici une:  

[![](https://mermaid.ink/img/pako:eNqVkk1Pg0AQhv8KmZNG2vBREYg3PTUxMfFmuExhaNcAW5ddY8X-dxe2rUupB5cDk2c-3pdhO8h5QZBCXmHbPjJcC6yzxtFnSeoBhaTWuf-ezZxnfFckTcrEhi-5IiUutpj42CIko2nLiB_bTWqwdCjuDOrPrJXCaXhtEdtbf24kEySuri2CZcnyzS_c2xrGhK3xNqj6ExLYhJRjKRRUoziXLZh2y1ZqjGcrlMiqikawZI0xctHjaTm2zdxa16BXU4XN-szEv7fRb7Ozy_XHyz-qp6Y-sDr9XuOSqzGZ6oMLNYkaWaHv4jAsA7mhmjJIdVhQiaqSGWRNX4pK8pddk0MqhSIX1LZASYfbC2mJVaspFUxy8XS43_3LhS02kHbwCWkQh_MoWsSxF0bRnRcGLuw0DeZR7Me-vwgC30tu470LX5zrod488YYnjPw4TIJkMUx7HZKD4v4HGan0Gg?type=png)](https://mermaid.live/edit#pako:eNqVkk1Pg0AQhv8KmZNG2vBREYg3PTUxMfFmuExhaNcAW5ddY8X-dxe2rUupB5cDk2c-3pdhO8h5QZBCXmHbPjJcC6yzxtFnSeoBhaTWuf-ezZxnfFckTcrEhi-5IiUutpj42CIko2nLiB_bTWqwdCjuDOrPrJXCaXhtEdtbf24kEySuri2CZcnyzS_c2xrGhK3xNqj6ExLYhJRjKRRUoziXLZh2y1ZqjGcrlMiqikawZI0xctHjaTm2zdxa16BXU4XN-szEv7fRb7Ozy_XHyz-qp6Y-sDr9XuOSqzGZ6oMLNYkaWaHv4jAsA7mhmjJIdVhQiaqSGWRNX4pK8pddk0MqhSIX1LZASYfbC2mJVaspFUxy8XS43_3LhS02kHbwCWkQh_MoWsSxF0bRnRcGLuw0DeZR7Me-vwgC30tu470LX5zrod488YYnjPw4TIJkMUx7HZKD4v4HGan0Gg)

# Remarques concernant les classes
La classe `Cartes` ne présente pas de difficultés particulières avec ses deux attributs `couleur` et `valeur`. L'As est particulier, c'est la plus forte carte, on peut lui attribuer la valeur 14.  

La classe `JeuCartes` doit permettre de constituer les 52 cartes d'un jeu. Son attribut `cartes` étant une instance de `Cartes`. 

Pour pouvoir jouer, deux joueurs ont besoin d'un paquet de cartes issus d'un jeu de cartes. D'où la présence d'une classe `Paquet`.
# Roadmap 
[![](https://mermaid.ink/img/pako:eNqVkTtOA0EMQK9iTR0J8Wu2BgqkSFFCuY13xpsYzWfl8RQoymWoyDn2YswGEpQgClyNPE9-_myNTY5MY5QDeY7URqihrJ5gIemVFJ6pgCOwKEr5639FASsM19DAU5KAyilWKMNaUhmO2DEamCc37j3nb7DAIKnz40egS3I57odq4lNFHd_t5lJ8M4kxsGeUY9VchiGJTr067vuSp-QV2OQ9dkkO0KXt0QprETqYrMecf_f-QllzLeSoK-vzNm7_KFHpOWcCilUfQolsf1aUurrV_1juziwFBA-DnkP3FVrIuM8U9TSqmZlA9Tzs6om3U6Y1uqG6ddPUp6Mei9fWtHFXUSyaVm_Rmkal0MyUwaHSA-NaMJimR59p9wkY9bgI?type=png)](https://mermaid.live/edit#pako:eNqVkTtOA0EMQK9iTR0J8Wu2BgqkSFFCuY13xpsYzWfl8RQoymWoyDn2YswGEpQgClyNPE9-_myNTY5MY5QDeY7URqihrJ5gIemVFJ6pgCOwKEr5639FASsM19DAU5KAyilWKMNaUhmO2DEamCc37j3nb7DAIKnz40egS3I57odq4lNFHd_t5lJ8M4kxsGeUY9VchiGJTr067vuSp-QV2OQ9dkkO0KXt0QprETqYrMecf_f-QllzLeSoK-vzNm7_KFHpOWcCilUfQolsf1aUurrV_1juziwFBA-DnkP3FVrIuM8U9TSqmZlA9Tzs6om3U6Y1uqG6ddPUp6Mei9fWtHFXUSyaVm_Rmkal0MyUwaHSA-NaMJimR59p9wkY9bgI)

# Modalités d'évaluation
* Présentation du travail
* Qualité du travail collaboratif et recherche

# Pistes pour les plus rapides
Envisager une partie `Joueur humain` vs `Programme`.