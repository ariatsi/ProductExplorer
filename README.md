# Product Explorer - TP7 final

Code final indicatif apres evolution **TP6 -> TP7**.

## Evolution du projet

- **TP4** : ecran de detail produit.
- **TP5** : ecran d'accueil.
- **TP6** : catalogue local avec `LazyColumn` et `LazyRow`.
- **TP7** : personnalisation graphique avec Material Design 3.

## Ce que fait le TP7

Le projet garde le catalogue du TP6, mais ameliore son apparence avec le theme Compose.

On personnalise :

- les couleurs avec `ColorScheme` ;
- les textes avec `Typography` ;
- les formes avec `Shapes` ;
- le theme clair ;
- le theme sombre.

## Fichiers principaux

- `MainActivity.kt`
- `ui/theme/Theme.kt`
- `ui/theme/Type.kt`

Selon votre projet, les noms exacts peuvent varier.

## Notions utilisees

- `MaterialTheme`
- `MaterialTheme.colorScheme`
- `MaterialTheme.typography`
- `MaterialTheme.shapes`
- `lightColorScheme`
- `darkColorScheme`
- `Shapes`
- `Typography`
- previews claire et sombre

## Changements principaux

Le theme centralise maintenant l'apparence de l'application :

```kotlin
MaterialTheme(
    colorScheme = colorScheme,
    typography = ProductTypography,
    shapes = ProductShapes,
    content = content
)
```

Les composants utilisent le theme au lieu de couleurs ou formes fixes :

```kotlin
color = MaterialTheme.colorScheme.primary
shape = MaterialTheme.shapes.large
style = MaterialTheme.typography.titleLarge
```

## Previews

Le TP ajoute deux previews pour comparer le rendu :

- `ProductCatalogScreenLightPreview`
- `ProductCatalogScreenDarkPreview`

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
tp7-final
```
