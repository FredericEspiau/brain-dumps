# Polymorphisme

Capacité à fournir une interface (pas au sens objet du terme) à plusieurs entités de types différents

Il existe trois types de polymorphisme

# Ad hoc polymorphism

Fonction ou opérateyr qui a plusieurs implémentations en fonction du type d'arguments qu'elle accepte

## Exemple

```
1 + 2 = 3
"bab" + "oon" = "baboon"
```

L'opérateur `+` possède plusieurs implémentations en fonction du type de ses arguments, qu'ils soient deux entiers ou deux strings

La bonne implémentation à appliquer sera déterminée en fonction du type des arguments

# Parametric polymorphism

Les génériques

Capacité à définir des fonctions ou types qui nécessitent au moins un type en paramètre

Afin de pouvoir appliquer la fonction ou le type, il faudra d'abord fournir le ou les types

```ts
type Value<T> = { value: T };

// on ne peut utiliser Value qu'à condition de lui donner un type en paramètre
const aNumber: Value<number> = { value: 3 };
```

Permet d'avoir des comportements sans dépendre d'un type précis

```ts
// Pas besoin de savoir ce qu'est T
const isOver = <T>(a: Value<T>, b: Value<T>): boolean => a.value > b.value;
```

# Subtyping

L'héritage
