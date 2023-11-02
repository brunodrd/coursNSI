**Commentaires**  

Le cas d'une insertion en tête est trivial !  
Dans le cas d'une insertion à un index quelconque $k\gt 0$, on doit d'abord créer
deux *pointeurs* vers les cellules concernées par l'insertion. Ensuite,
dans une boucle, on décale ces pointeurs jusqu'à ce l'on trouve la position 
où sera inséré la nouvelle cellule.
