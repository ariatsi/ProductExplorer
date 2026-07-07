# Product Explorer - TP5 final

Code final indicatif apres evolution **TP4 -> TP5**.

## Evolution du projet

- **TP4** : creation de l'ecran de detail produit.
- **TP5** : ajout d'un ecran d'accueil `ProductHomeScreen`.
- L'ecran detail reste dans le code, mais l'application affiche maintenant l'accueil au lancement.

## Ce que fait le TP5

L'ecran d'accueil contient :

- un en-tete ;
- une fausse zone de recherche ;
- un produit mis en avant ;
- des categories ;
- une offre du jour avec badge ;
- un bouton "Voir le produit" sans vraie navigation pour l'instant.

Les donnees restent locales.

## Notions utilisees

- `Column`
- `Row`
- `Box`
- `Spacer`
- `Surface`
- `Button`
- `Modifier`
- callbacks
- `@Preview`

## Composables principaux

- `ProductHomeScreen`
- `HomeHeader`
- `SearchPreviewBar`
- `FeaturedProductSection`
- `ProductQuickInfoRow`
- `CategoryChip`
- `CategoriesSection`
- `DailyOfferBox`
- `ProductHomeScreenPreview`

Les composables du TP4 restent disponibles, notamment :

- `ProductDetailScreen`
- `ProductHeader`
- `ProductPriceCard`
- `ProductRating`
- `ProductAvailabilityCard`

## Point d'entree

Dans `MainActivity`, l'application affiche maintenant :

```kotlin
ProductHomeScreen(
    featuredProduct = sampleProduct(),
    onFeaturedProductClick = {
        // Plus tard : ouvrir le detail du produit
    },
    modifier = Modifier.padding(innerPadding)
)
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
tp5-final
```
