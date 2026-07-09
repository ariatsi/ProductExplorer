# Product Explorer - TP10 final

Code final indicatif apres evolution **TP9 -> TP10**.

## Evolution du projet

- **TP4** : ecran de detail produit.
- **TP5** : ecran d'accueil.
- **TP6** : catalogue local avec listes modernes.
- **TP7** : personnalisation Material Design 3.
- **TP8** : catalogue interactif avec etats Compose.
- **TP9** : refactor avec **State Hoisting**.
- **TP10** : gestion de l'etat de l'ecran avec un **ViewModel**.

## Ce que fait le TP10

Le comportement reste le meme que dans le TP9 :

- recherche de produits ;
- filtre "produits en stock" ;
- favoris locaux ;
- mise a jour automatique de l'interface.

La difference principale est l'organisation de l'etat.

L'etat principal du catalogue n'est plus stocke dans `ProductCatalogContainer`.
Il est maintenant gere par `ProductCatalogViewModel`.

Les donnees restent locales.

## Notions utilisees

- `ViewModel`
- etat UI
- cycle de vie
- `mutableStateOf`
- `private set`
- fonctions d'evenement
- composable stateless
- `viewModel()`
- callbacks

## Nouveau ViewModel

`ProductCatalogViewModel` gere maintenant :

- la liste complete des produits ;
- les categories ;
- `searchQuery` ;
- `showOnlyInStock` ;
- `favoriteProductIds` ;
- le calcul de `filteredProducts` ;
- les actions de l'utilisateur.

Exemple :

```kotlin
class ProductCatalogViewModel : ViewModel() {

    private val allProducts: List<ProductUi> = sampleProducts()

    val categories: List<String> = sampleCategories()

    var searchQuery by mutableStateOf("")
        private set

    var showOnlyInStock by mutableStateOf(false)
        private set

    var favoriteProductIds by mutableStateOf(listOf<Int>())
        private set

    // Etat et actions du catalogue
}
```

## Actions utilisateur

L'interface ne modifie plus directement l'etat.

Elle appelle des fonctions du ViewModel :

```kotlin
fun onSearchQueryChange(newValue: String) {
    searchQuery = newValue
}

fun onToggleStockFilter() {
    showOnlyInStock = !showOnlyInStock
}

fun onFavoriteClick(productId: Int) {
    favoriteProductIds = if (favoriteProductIds.contains(productId)) {
        favoriteProductIds - productId
    } else {
        favoriteProductIds + productId
    }
}
```

## Nouveaux roles

`ProductCatalogViewModel` :

- possede l'etat de l'ecran ;
- modifie cet etat ;
- calcule la liste filtree ;
- reagit aux actions de l'utilisateur.

`ProductCatalogContainer` :

- recupere le ViewModel avec `viewModel()` ;
- lit les valeurs du ViewModel ;
- transmet les valeurs et callbacks a l'ecran.

`ProductCatalogScreen` reste **stateless** :

- recoit les valeurs a afficher ;
- recoit les callbacks ;
- affiche l'interface ;
- ne gere pas directement l'etat du catalogue.

`ProductListItem` reste aussi **stateless**.

## ProductCatalogContainer

Le container recupere maintenant le ViewModel :

```kotlin
@Composable
fun ProductCatalogContainer(
    onProductClick: (ProductUi) -> Unit,
    modifier: Modifier = Modifier,
    viewModel: ProductCatalogViewModel = viewModel()
) {
    ProductCatalogScreen(
        products = viewModel.filteredProducts,
        categories = viewModel.categories,
        searchQuery = viewModel.searchQuery,
        onSearchQueryChange = viewModel::onSearchQueryChange,
        showOnlyInStock = viewModel.showOnlyInStock,
        onToggleStockFilter = viewModel::onToggleStockFilter,
        favoriteProductIds = viewModel.favoriteProductIds,
        onFavoriteClick = viewModel::onFavoriteClick,
        onProductClick = onProductClick,
        modifier = modifier
    )
}
```

## Point d'entree

Dans `MainActivity`, l'application appelle maintenant :

```kotlin
ProductCatalogContainer(
    onProductClick = {
        // Plus tard : ouvrir le detail du produit
    },
    modifier = Modifier.padding(innerPadding)
)
```

Les produits et les categories ne sont plus transmis depuis `MainActivity`.

## Principe important

```text
Le composable affiche l'etat.
Le ViewModel gere l'etat.
```

Le principe du State Hoisting reste valable :

```text
Les donnees descendent.
Les evenements remontent.
```

## Previews

Les previews peuvent maintenant tester directement le composable stateless :

- `ProductCatalogScreenLightPreview`
- `ProductCatalogScreenDarkPreview`

Les valeurs sont fournies directement a `ProductCatalogScreen`.

La preview teste l'affichage et n'a pas besoin de creer un ViewModel.

## Imports utiles

```kotlin
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.setValue
import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewmodel.compose.viewModel
```

Pour utiliser `viewModel()`, le projet doit disposer de la dependance ViewModel Compose.

Ne pas ajouter une version de dependance au hasard : utilisez une version coherente avec la configuration du projet.

## A tester

- saisir un texte de recherche ;
- effacer la recherche ;
- activer / desactiver le filtre stock ;
- ajouter / retirer un favori ;
- verifier que la liste se met a jour ;
- tourner l'ecran ;
- verifier que les previews compilent.

Le comportement doit rester identique au TP9.

La difference est que l'etat du catalogue est maintenant gere par le ViewModel.

## Organisation finale

```text
MainActivity
    ↓
ProductCatalogContainer
    ↓
ProductCatalogViewModel
    ↓
ProductCatalogScreen
    ↓
ProductListItem
```

Plus precisement :

- `MainActivity` affiche le catalogue ;
- `ProductCatalogContainer` relie le ViewModel a Compose ;
- `ProductCatalogViewModel` gere l'etat de l'ecran ;
- `ProductCatalogScreen` affiche l'interface ;
- `ProductListItem` affiche un produit.

## Telecharger une version precise

Chaque fin de TP est disponible avec un tag Git :

- `tp4-final`
- `tp5-final`
- `tp6-final`
- `tp7-final`
- `tp8-final`
- `tp9-final`
- `tp10-final`

Pour telecharger une version sans cloner le depot :

1. ouvrir la page GitHub du projet ;
2. aller dans **Tags** ;
3. choisir le tag voulu ;
4. cliquer sur **Download ZIP**.

Pour le code final de ce TP, choisir :

```text
tp10-final
```
