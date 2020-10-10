# Command–query separation (CQS)

## Problème

Un système a souvent besoin d'effectuer des effets de bord pour fonctionner (par exemple modifier une valeur en base de données)

Dans certains langages (JavaScript par exemple), il est impossible de savoir si une fonction aura un effet de bord ou non

Il faut un système pour identifier les fonctions qui auront un effet de bord

## Solution

Il faut impérativement séparer les fonctions en deux catégories:

- les *commandes* qui changent l'état observable du système mais ne renvoient pas de données
- les *queries* qui renvoient des données et qui ne changent pas l'état observable du système

Une fonction est soit une commande, soit une query, mais jamais les deux

## Exemple

```ts
class GetUserPreferences implements Query<boolean> {
    public async execute({ userId: string }): Result<boolean> {
        return this.source.getUserPreferencesByUserId(id);
    }
}
```

La classe `CheckUserExistence` est une query qui va me dire si oui ou non un utilisateur existe

```ts
type RegisterUserInput {
    id: string;
    email: string;
    firstName: string;
    lastName: string;
}

class RegisterUser implements Command {
    public async execute(input: RegisterUserInput): Result<void> {
        const userAlreadyExists = await this.doesUserAlreadyExist(input.emailAddress);

        return userAlreadyExists
            ? UserAlreadyExistsError
            : createUser(input);
    }

    // ...
}
```

La classe `RegisterUser` est une commande qui va vérifier si un utilisateur existe et si ce n'est pas le cas, va créer un nouvel utilisateur

Elle ne renvoie pas de données, elle indique seulement si l'exécution s'est déroulée correctement ou non à travers le `Result`

## Gestion de l'identifiant de l'utilisateur

Mon use case `RegisterUser` attend un identifiant, ce dernier n'est pas généré par mon back ou par ma base de données, c'est mon utilisateur qui me l'envoie

Si vous utilisez une solution de génération d'identifiants suffisamment solide, alors vos chances de collision pourraient être de l'ordre de 1% sur une période de 149 milliard d'années pour 1000 générations à la seconde

## Code de bas niveau

Le CQS s'applique plus rarement au code de bas niveau

```ts
const element = stack.pop();
```

Cette action va récupérer le dernier élément de la pile et passer au suivant
