# Product Explorer - TP4 code final

Code final indicatif du **TP4 - Ecran de detail produit**.

L'application affiche un produit avec ses informations principales : image, titre, marque, categorie, prix, remise, note, stock, livraison, description, garantie et bouton d'action.

## Notions vues

- `Text`, `Button`, `Icon`, `Image`
- `Card`, `OutlinedTextField`
- `Column`, `Row`, `Scaffold`
- `Modifier`
- `@Preview`
- callbacks
- `contentDescription`

## Fichier principal

```text
app/src/main/java/com/example/productexplorer/MainActivity.kt
```

Le code est decoupe en plusieurs composables, par exemple :

- `ProductDetailScreen`
- `ProductHeader`
- `ProductPriceCard`
- `ProductRating`
- `ProductAvailabilityCard`
- `ProductDescription`
- `ProductWarrantyField`
- `AddToCartButton`

## Icônes Material

Pour utiliser `Icons.Default.Star`, il faut ajouter la dependance des icones Material.

Dans `app/build.gradle.kts` :

```kotlin
implementation("androidx.compose.material:material-icons-extended")
```

Ou avec le Version Catalog, dans `gradle/libs.versions.toml` :

```toml
androidx-compose-material-icons-extended = { module = "androidx.compose.material:material-icons-extended" }
```

Puis dans `app/build.gradle.kts` :

```kotlin
implementation(libs.androidx.compose.material.icons.extended)
```

Comme le projet utilise le Compose BOM, il n'est generalement pas necessaire de preciser une version.

Apres modification : cliquer sur **Sync Now**.

## Télécharger une version précise

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
tp4-final
```
