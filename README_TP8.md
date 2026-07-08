# Product Explorer - TP8 final

Code final indicatif apres evolution **TP7 -> TP8**.

## Evolution du projet

- **TP4** : ecran de detail produit.
- **TP5** : ecran d'accueil.
- **TP6** : catalogue local avec listes modernes.
- **TP7** : personnalisation Material Design 3.
- **TP8** : catalogue interactif avec etats Compose.

## Ce que fait le TP8

Le catalogue devient interactif localement :

- recherche de produits ;
- filtrage par titre, marque ou categorie ;
- filtre "produits en stock uniquement" ;
- ajout / retrait de favoris ;
- mise a jour automatique de l'interface.

Les donnees restent locales.

## Notions utilisees

- `mutableStateOf`
- `remember`
- `rememberSaveable`
- `OutlinedTextField`
- filtre local
- favoris locaux
- recomposition Compose

## Composables modifies

- `ProductCatalogScreen`
- `ProductListItem`

## Changements principaux

Etat de recherche :

```kotlin
var searchQuery by rememberSaveable {
    mutableStateOf("")
}
```

Filtrage des produits :

```kotlin
val filteredProducts = remember(products, searchQuery, showOnlyInStock) {
    products.filter { product ->
        val matchesSearch =
            product.title.contains(searchQuery, ignoreCase = true) ||
            product.brand.contains(searchQuery, ignoreCase = true) ||
            product.category.contains(searchQuery, ignoreCase = true)

        val matchesStock = !showOnlyInStock || product.stock > 0

        matchesSearch && matchesStock
    }
}
```

Favoris locaux :

```kotlin
var favoriteProductIds by rememberSaveable {
    mutableStateOf(listOf<Int>())
}
```

## A tester

- rechercher un produit ;
- filtrer les produits en stock ;
- ajouter un favori ;
- retirer un favori ;
- tourner l'ecran pour verifier `rememberSaveable`.

## Telecharger une version precise

Chaque fin de TP est disponible avec un tag Git :

- `tp4-final`
- `tp5-final`
- `tp6-final`
- `tp7-final`
- `tp8-final`

Pour telecharger une version sans cloner le depot :

1. ouvrir la page GitHub du projet ;
2. aller dans **Tags** ;
3. choisir le tag voulu ;
4. cliquer sur **Download ZIP**.

Pour le code final de ce TP, choisir :

```text
tp8-final
```
