# TDD

Kernel: test le plus simple auquel on peut penser

Dans notre cas, 0 + 0 = 0

Ecrire les tests en commençant par l'assertion, ça aide à éviter le copy pasting

```ts
const sum = add(Fraction(0), Fraction(0));
expect(sum).toStrictEqual(0);
```

Mais on peut pas comparer une `Fraction` et un `number`, du coup deux solutions en Java
- générer la fonction `Equal` pour comparer une `Fraction` et un `number`
- créer du code spécifiquement pour le test qui va permettre de récupérer la valeur du nombre dans une `Fraction``

On va faire ça parce que on doit se concentrer sur une seule chose à la fois, la fonction `Add`, du coup on va faire du scafolding pour les tests

La deuxième solution est moins préférable à la première en général

`Fraction` est ce qu'on appelle une `whole value`

## `Whole value`

Quantité utilisée pour décrire des choses dans un domaine

Ne représente pas une chose mais une mesure d'une chose

Ils n'ont pas d'identité propre

Ex: une entreprise vaut 10 000 euros

Ce sont ses actions ou ses biens qui sont mesurés, mais ça ne représente pas l'entreprise

Ce qui est important c'est que c'est toute la propriété qui est mesurée, pas juste une partie d'elle

La valeur de l'entreprise est 10 000 euros et non juste 10 000 par exemple

Une valeur doit toujours être stockée avec son unité, ce qui forme une whole value

Ex:

Argent -> 5 000 dollars
Timestamp -> préciser si on est en milli ou nanosecondes et préciser la timezone
Coordonnées -> pour pas mélanger la longitude et latitude

## Comment ça marche en général ?

On a une classe qui contient la valeur et dedans des méthodes pour combiner des objets de cette classe et retourne une nouvelle instance de la classe

----

Always watch a test fail first !

-> gives a target to aim for. Aim small
-> make you fail until you success

---

A chaque fois que tous les tests passent, faire un commit

---

Avant refactoring, rendre le code symétrique pour voir plus facilement ce qu'il faut refactorer

---

une fois qu'on pense que notre implem fait passer tous nos cas de tests, est pas obligé d'écrire les autres tests qu'on avait prévu

on écrit que des testq ui nous font changer la conception de notre code

---

dans les tests, on peut copier-coller quelques lignes de code mais un gros setup dans chaque test non par contre

---

Defect injection: intentionally writing incorrect code to ensure a test fails

Si en changeant le code les tests failent, alors on sait que le test fonctionne

---

Test until fear becomes boredom -> une fois qu'on est sûr de notre code, pas besoin d'écrire les tests supplémentaires

