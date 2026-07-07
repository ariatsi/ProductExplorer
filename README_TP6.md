# Product Explorer - TP6 final

Code final indicatif apres evolution **TP5 -> TP6**.

## Evolution du projet

- **TP4** : ecran de detail produit.
- **TP5** : ecran d'accueil `ProductHomeScreen`.
- **TP6** : ajout d'un catalogue local avec `ProductCatalogScreen`.
- Les ecrans precedents restent dans le code, mais l'application affiche maintenant le catalogue au lancement.

## Ce que fait le TP6

L'ecran catalogue contient :

- un titre ;
- une courte description ;
- une ligne horizontale de categories ;
- une liste verticale de produits ;
- une carte reutilisable pour chaque produit ;
- un bouton "Voir le produit" sans vraie navigation pour l'instant.

Les donnees restent locales.

## Notions utilisees

- `LazyColumn`
- `LazyRow`
- `items`
- `item`
- `PaddingValues`
- `Arrangement`
- donnees locales Kotlin
- carte produit reutilisable
- callbacks
- `@Preview`

## Composables / fonctions principaux

- `ProductCatalogScreen`
- `ProductListItem`
- `CategoryRow`
- `sampleProducts()`
- `sampleCategories()`
- `ProductCatalogScreenPreview`

Elements deja presents depuis les TP precedents :

- `ProductUi`
- `sampleProduct()`
- `ProductHomeScreen`
- `ProductDetailScreen`
- `CategoryChip`

## Changement important

`ProductUi` possede maintenant un identifiant :

```kotlin
val id: Int
```

Il est utilise comme cle stable dans la liste :

```kotlin
items(
    items = products,
    key = { product -> product.id }
)
```

## Point d'entree

Dans `MainActivity`, l'application affiche maintenant :

```kotlin
ProductCatalogScreen(
    products = sampleProducts(),
    onProductClick = {
        // Plus tard : ouvrir le detail du produit
    },
    modifier = Modifier.padding(innerPadding)
)
```

## Imports utiles

```kotlin
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.PaddingValues
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.LazyRow
import androidx.compose.foundation.lazy.items
```

## Telecharger une version precise

Chaque fin de TP est disponible avec un tag Git :

- `tp4-final`
- `tp5-final`
- `tp6-final`
- `tp7-final`

Pour telecharger une version sans cloner le depot :

1. ouvrir la page GitHub du projet ;
2. aller dans **Tags** ;
3. choisir le tag voulu ;
4. cliquer sur **Download ZIP**.

Pour le code final de ce TP, choisir :

```text
tp6-final
```
