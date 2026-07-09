# Product Explorer - TP11 final

Code final indicatif apres evolution **TP10 -> TP11**.

## Evolution du projet

- **TP4** : ecran de detail produit.
- **TP5** : ecran d'accueil.
- **TP6** : catalogue local avec listes modernes.
- **TP7** : personnalisation Material Design 3.
- **TP8** : catalogue interactif avec etats Compose.
- **TP9** : refactor avec **State Hoisting**.
- **TP10** : gestion de l'etat de l'ecran avec un **ViewModel**.
- **TP11** : etat UI observable avec **StateFlow**.

## Ce que fait le TP11

Le comportement reste proche du TP10 :

- recherche de produits ;
- filtre "produits en stock" ;
- favoris locaux ;
- mise a jour automatique de l'interface.

La difference principale est l'organisation de l'etat dans le ViewModel.

Dans le TP10, le ViewModel exposait plusieurs proprietes separees.

Dans le TP11, l'etat complet de l'ecran est regroupe dans un seul objet :

```kotlin
ProductCatalogUiState
```

Cet etat est expose avec un `StateFlow`.

Les donnees restent locales.

## Notions utilisees

- `Flow`
- `StateFlow`
- `MutableStateFlow`
- `asStateFlow`
- `collectAsState`
- etat UI
- `data class`
- `copy`
- `update`
- UI reactive

## Nouvel etat UI

`ProductCatalogUiState` represente l'etat complet du catalogue.

Il contient :

- les produits affiches ;
- les categories ;
- le texte de recherche ;
- l'etat du filtre stock ;
- les identifiants des produits favoris.

Exemple :

```kotlin
data class ProductCatalogUiState(
    val products: List<ProductUi> = emptyList(),
    val categories: List<String> = emptyList(),
    val searchQuery: String = "",
    val showOnlyInStock: Boolean = false,
    val favoriteProductIds: List<Int> = emptyList()
)
```

Cette classe contient les valeurs que l'ecran doit afficher.

Elle ne contient pas la logique de modification de l'etat.

## StateFlow dans le ViewModel

Le ViewModel possede maintenant un `MutableStateFlow` prive :

```kotlin
private val _uiState = MutableStateFlow(
    ProductCatalogUiState(
        products = allProducts,
        categories = allCategories
    )
)
```

L'interface observe une version publique en lecture :

```kotlin
val uiState: StateFlow<ProductCatalogUiState> =
    _uiState.asStateFlow()
```

## Principe important

```text
_uiState est modifie par le ViewModel.
uiState est observe par l'interface.
```

L'UI ne modifie pas directement l'etat interne du ViewModel.

## Mise a jour de l'etat

Pour modifier l'etat, le ViewModel utilise `update`.

Exemple avec la recherche :

```kotlin
fun onSearchQueryChange(newValue: String) {
    _uiState.update { currentState ->
        currentState.copy(
            searchQuery = newValue,
            products = filterProducts(
                searchQuery = newValue,
                showOnlyInStock = currentState.showOnlyInStock
            )
        )
    }
}
```

`update` fournit l'etat actuel.

`copy` permet de creer une nouvelle version de cet etat en modifiant seulement les valeurs necessaires.

## Filtre stock

Le filtre stock met aussi a jour l'etat complet :

```kotlin
fun onToggleStockFilter() {
    _uiState.update { currentState ->
        val newShowOnlyInStock = !currentState.showOnlyInStock

        currentState.copy(
            showOnlyInStock = newShowOnlyInStock,
            products = filterProducts(
                searchQuery = currentState.searchQuery,
                showOnlyInStock = newShowOnlyInStock
            )
        )
    }
}
```

La liste affichee est recalculee avec la nouvelle valeur du filtre.

## Favoris

Les favoris font egalement partie de `ProductCatalogUiState`.

```kotlin
fun onFavoriteClick(productId: Int) {
    _uiState.update { currentState ->
        val newFavoriteIds =
            if (currentState.favoriteProductIds.contains(productId)) {
                currentState.favoriteProductIds - productId
            } else {
                currentState.favoriteProductIds + productId
            }

        currentState.copy(
            favoriteProductIds = newFavoriteIds
        )
    }
}
```

On ne modifie pas directement la liste existante.

On cree une nouvelle liste, puis un nouvel etat.

## Nouveaux roles

`ProductCatalogViewModel` :

- possede `_uiState` ;
- expose `uiState` ;
- modifie l'etat avec `update` ;
- cree de nouvelles versions de l'etat avec `copy` ;
- gere les actions de l'utilisateur.

`ProductCatalogContainer` :

- recupere le ViewModel ;
- collecte `uiState` avec `collectAsState()` ;
- transmet les valeurs et callbacks a l'ecran.

`ProductCatalogScreen` reste **stateless** :

- ne connait pas `StateFlow` ;
- recoit les valeurs a afficher ;
- recoit les callbacks ;
- affiche l'interface.

`ProductListItem` reste aussi **stateless**.

## ProductCatalogContainer

Le container observe maintenant le `StateFlow` :

```kotlin
@Composable
fun ProductCatalogContainer(
    onProductClick: (ProductUi) -> Unit,
    modifier: Modifier = Modifier,
    viewModel: ProductCatalogViewModel = viewModel()
) {
    val uiState by viewModel.uiState.collectAsState()

    ProductCatalogScreen(
        products = uiState.products,
        categories = uiState.categories,
        searchQuery = uiState.searchQuery,
        onSearchQueryChange = viewModel::onSearchQueryChange,
        showOnlyInStock = uiState.showOnlyInStock,
        onToggleStockFilter = viewModel::onToggleStockFilter,
        favoriteProductIds = uiState.favoriteProductIds,
        onFavoriteClick = viewModel::onFavoriteClick,
        onProductClick = onProductClick,
        modifier = modifier
    )
}
```

`collectAsState()` transforme le `StateFlow` en etat Compose observable.

Quand le ViewModel publie un nouvel etat, Compose peut mettre a jour l'interface.

## Point d'entree

Dans `MainActivity`, le code ne change pas.

L'application appelle toujours :

```kotlin
ProductCatalogContainer(
    onProductClick = {
        // Plus tard : ouvrir le detail du produit
    },
    modifier = Modifier.padding(innerPadding)
)
```

`MainActivity` ne gere pas l'etat du catalogue.

## Principe de l'UI reactive

```text
Le ViewModel publie un etat.
Compose observe cet etat.
L'interface se met a jour quand l'etat change.
```

Les actions de l'utilisateur remontent toujours vers le ViewModel :

```text
Etat
    ↓
Interface
    ↑
Actions utilisateur
```

## Previews

Les previews continuent a tester directement le composable stateless :

- `ProductCatalogScreenLightPreview`
- `ProductCatalogScreenDarkPreview`

Les valeurs sont fournies directement a `ProductCatalogScreen`.

Les previews n'ont pas besoin de ViewModel ni de `StateFlow`.

## Imports utiles

```kotlin
import androidx.compose.runtime.collectAsState
import androidx.compose.runtime.getValue
import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewmodel.compose.viewModel
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.flow.asStateFlow
import kotlinx.coroutines.flow.update
```

Les anciens imports lies a `mutableStateOf`, `setValue` ou `rememberSaveable` peuvent etre supprimes de ce fichier s'ils ne sont plus utilises.

## A tester

- lancer l'application ;
- verifier que le catalogue s'affiche ;
- saisir un texte de recherche ;
- verifier que la liste est filtree ;
- activer / desactiver le filtre stock ;
- ajouter / retirer un favori ;
- verifier que le nombre de produits affiches se met a jour ;
- verifier que les previews claire et sombre compilent.

Le comportement doit rester proche du TP10.

La difference principale est que le ViewModel expose maintenant un seul etat observable.

## Organisation finale

```text
ProductCatalogViewModel
    |
    | StateFlow<ProductCatalogUiState>
    ↓
ProductCatalogContainer
    |
    | collectAsState()
    ↓
ProductCatalogScreen
    ↓
ProductListItem
```

Plus precisement :

- `ProductCatalogViewModel` publie l'etat ;
- `ProductCatalogContainer` collecte l'etat ;
- `ProductCatalogScreen` affiche les valeurs ;
- `ProductListItem` affiche un produit ;
- les actions utilisateur remontent vers le ViewModel.

## Ce que StateFlow apporte ici

Avant, le ViewModel exposait plusieurs proprietes separees.

Maintenant, il expose :

```kotlin
val uiState: StateFlow<ProductCatalogUiState>
```

`uiState` contient tout ce que l'ecran doit afficher.

Cela rend la communication entre le ViewModel et l'interface plus claire.

## A retenir

```text
Le ViewModel publie un etat.
Compose observe cet etat.
L'interface se met a jour quand l'etat change.
```

A ce stade, les donnees restent locales.

## Telecharger une version precise

Chaque fin de TP est disponible avec un tag Git :

- `tp4-final`
- `tp5-final`
- `tp6-final`
- `tp7-final`
- `tp8-final`
- `tp9-final`
- `tp10-final`
- `tp11-final`

Pour telecharger une version sans cloner le depot :

1. ouvrir la page GitHub du projet ;
2. aller dans **Tags** ;
3. choisir le tag voulu ;
4. cliquer sur **Download ZIP**.

Pour le code final de ce TP, choisir :

```text
tp11-final
```
