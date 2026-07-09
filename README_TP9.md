# Product Explorer - TP9 final

Code final indicatif apres evolution **TP8 -> TP9**.

## Evolution du projet

- **TP4** : ecran de detail produit.
- **TP5** : ecran d'accueil.
- **TP6** : catalogue local avec listes modernes.
- **TP7** : personnalisation Material Design 3.
- **TP8** : catalogue interactif avec etats Compose.
- **TP9** : refactor avec **State Hoisting**.

## Ce que fait le TP9

Le comportement reste le meme que dans le TP8 :

- recherche de produits ;
- filtre "produits en stock" ;
- favoris locaux ;
- mise a jour automatique de l'interface.

La difference principale est l'organisation du code.

## Notions utilisees

- State Hoisting
- composable stateful
- composable stateless
- valeurs descendantes
- evenements remontants
- callbacks
- `rememberSaveable`
- `remember`
- `mutableStateOf`

## Nouveaux roles des composables

`ProductCatalogContainer` devient le composable **stateful** :

- possede `searchQuery` ;
- possede `showOnlyInStock` ;
- possede `favoriteProductIds` ;
- calcule `filteredProducts` ;
- contient `toggleFavorite()`.

`ProductCatalogScreen` devient le composable **stateless** :

- recoit les valeurs a afficher ;
- recoit les callbacks ;
- affiche la `LazyColumn` ;
- ne contient plus de `remember`.

`ProductListItem` reste aussi **stateless** :

- recoit `isFavorite` ;
- recoit `onFavoriteClick` ;
- recoit `onClick`.

## Point d'entree

Dans `MainActivity`, l'application appelle maintenant :

```kotlin
ProductCatalogContainer(
    products = sampleProducts(),
    categories = sampleCategories(),
    onProductClick = {
        // Plus tard : ouvrir le detail du produit
    },
    modifier = Modifier.padding(innerPadding)
)
```

## Principe important

```text
Les donnees descendent.
Les evenements remontent.
```

## Previews

Les previews du catalogue doivent maintenant appeler plutot :

- `ProductCatalogContainerLightPreview`
- `ProductCatalogContainerDarkPreview`

Une preview stateless peut aussi etre gardee pour tester `ProductCatalogScreen` avec des valeurs fournies directement.

## Imports utiles

```kotlin
import androidx.compose.runtime.getValue
import androidx.compose.runtime.setValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.saveable.rememberSaveable
import androidx.compose.foundation.lazy.items
```

## A tester

- saisir un texte de recherche ;
- activer / desactiver le filtre stock ;
- ajouter / retirer un favori ;
- verifier que la liste se met a jour ;
- verifier que les previews compilent.

## Telecharger une version precise

Chaque fin de TP est disponible avec un tag Git :

- `tp4-final`
- `tp5-final`
- `tp6-final`
- `tp7-final`
- `tp8-final`
- `tp9-final`

Pour telecharger une version sans cloner le depot :

1. ouvrir la page GitHub du projet ;
2. aller dans **Tags** ;
3. choisir le tag voulu ;
4. cliquer sur **Download ZIP**.

Pour le code final de ce TP, choisir :

```text
tp9-final
```
