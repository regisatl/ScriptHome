```js
AlbumComponent.ts

import { Component } from '@angular/core';
import { AlbumService, Album } from 'chemin-vers-le-fichier-album.service'; // Replace 'chemin-vers-le-fichier-album.service' with the correct path

@Component({
  selector: 'app-albums',
  templateUrl: './albums.component.html',
  styleUrls: ['./albums.component.css'],
})
export class AlbumsComponent {
  private start = 0;
  private end = 10;
  paginatedAlbums: Album[];
  totalPages: number;
  currentPage: number;

  constructor(private albumService: AlbumService) {
    this.paginatedAlbums = this.albumService.paginate(this.start, this.end);
    this.totalPages = this.getTotalPages();
    this.currentPage = 1;
  }

  onPageChange(pageNumber: number) {
    if (pageNumber >= 1 && pageNumber <= this.totalPages) {
      this.currentPage = pageNumber;
      this.start = (this.currentPage - 1) * 2; // Change 2 to the number of albums per page
      this.end = this.start + 2; // Change 2 to the number of albums per page
      this.paginatedAlbums = this.albumService.paginate(this.start, this.end);
    }
  }

  getTotalPages(): number {
    return Math.ceil(this.albumService.getAlbumsCount() / 2); // Change 2 to the number of albums per page
  }

  getPageNumbers(): number[] {
    return Array.from({ length: this.totalPages }, (_, i) => i + 1);
  }
}

AlbumService.ts

import { Injectable } from '@angular/core';

export interface Album {
  // Define the properties of an album here
}

@Injectable({
  providedIn: 'root',
})
export class AlbumService {
  private albums: Album[] = []; // Add your album data here

  paginate(start: number, end: number): Album[] {
    return this.albums.slice(start, end);
  }

  getAlbumsCount(): number {
    return this.albums.length;
  }

  // ... Other methods for managing albums
}

AlbumsComponent.html
``` html
<!-- Add your existing HTML content for displaying the paginated albums here -->
<!-- For example, display album names -->
<ul>
  <li *ngFor="let album of paginatedAlbums">{{ album.name }}</li>
</ul>

<!-- Pagination -->
<ul class="pagination">
  <li class="page-item" (click)="onPageChange(currentPage - 1)">
    <a class="page-link" href="#" [disabled]="currentPage === 1">Previous</a>
  </li>
  <li class="page-item" *ngFor="let pageNumber of getPageNumbers()">
    <a
      class="page-link"
      href="#"
      [class.active]="pageNumber === currentPage"
      (click)="onPageChange(pageNumber)"
    >
      {{ pageNumber }}
    </a>
  </li>
  <li class="page-item" (click)="onPageChange(currentPage + 1)">
    <a class="page-link" href="#" [disabled]="currentPage === totalPages">Next</a>
  </li>
</ul>
```

texte avec explication

Bien sûr ! Voici une explication détaillée du code que j'ai fourni pour implémenter la pagination des albums dans l'application Angular :

1. **AlbumService** (fichier `album.service.ts`):

   - `AlbumService` est un service Angular qui contient les données des albums et les méthodes associées pour gérer ces données.

   - `Album` est une interface définissant la structure des albums. Vous devez remplacer les propriétés par les informations spécifiques à vos albums, par exemple, `name`, `artist`, etc.

   - `private albums: Album[] = [];`: C'est un tableau privé qui contient les données des albums. Vous devez ajouter vos albums ici ou les récupérer depuis une source de données externe, comme une API HTTP.

   - `paginate(start: number, end: number): Album[]`: Cette méthode retourne une tranche (slice) du tableau `albums` en fonction des indices `start` et `end`. Cela permettra de renvoyer uniquement les albums nécessaires pour afficher une page donnée.

   - `getAlbumsCount(): number`: Cette méthode renvoie le nombre total d'albums dans le tableau `albums`. Elle est utilisée pour déterminer le nombre total de pages.

2. **AlbumsComponent** (fichier `albums.component.ts`):

   - `AlbumsComponent` est un composant Angular qui affiche la liste paginée des albums.

   - `private start = 0; private end = 10;`: Ces variables privées représentent les indices de début et de fin pour la pagination. Elles sont utilisées pour extraire la tranche d'albums à afficher.

   - `paginatedAlbums: Album[];`: C'est un tableau qui contiendra les albums paginés à afficher dans le composant.

   - `totalPages: number; currentPage: number;`: Ces variables permettent de suivre le nombre total de pages et la page actuellement affichée.

   - Le constructeur récupère initialement les albums paginés à partir du service, détermine le nombre total de pages à afficher et initialise la page actuelle à 1.

   - `onPageChange(pageNumber: number)`: Cette méthode est appelée lorsqu'un lien de pagination est cliqué. Elle met à jour les indices de début et de fin en fonction du numéro de page demandé, puis récupère la tranche d'albums correspondante depuis le service.

   - `getTotalPages(): number`: Cette méthode utilise la méthode `getAlbumsCount()` du service pour déterminer le nombre total de pages nécessaires pour afficher tous les albums. Elle utilise `Math.ceil()` pour arrondir vers le haut pour le cas où le nombre d'albums n'est pas divisible par le nombre d'albums par page.

   - `getPageNumbers(): number[]`: Cette méthode retourne un tableau de numéros de page, allant de 1 à `totalPages`. Cela est utilisé dans le template pour afficher les liens de pagination.

3. **Le fichier HTML du composant** (fichier `albums.component.html`):

   - Dans le template HTML, vous devez afficher les albums paginés en utilisant une boucle `*ngFor`, où chaque élément représente un album.

   - La liste de pagination utilise une boucle `*ngFor` également pour afficher les liens de chaque page en utilisant la méthode `getPageNumbers()` du composant.

   - Les liens de pagination sont configurés avec `(click)="onPageChange(pageNumber)"` pour appeler la méthode `onPageChange()` du composant lorsque l'utilisateur clique sur un lien.

   - Les boutons "Previous" et "Next" sont désactivés (`[disabled]`) lorsqu'il n'y a pas de page précédente ou suivante à afficher. Cela évite de naviguer en dehors des limites de pagination.

N'oubliez pas que vous devez remplacer les propriétés de l'interface `Album` par les propriétés spécifiques de vos albums dans le service. Vous pouvez également personnaliser le nombre d'albums affichés par page en ajustant les valeurs `start` et `end` dans le composant, et le nombre dans la méthode `getTotalPages()`.

Ce code permettra de paginer la liste d'albums deux par deux sur la page principale de votre application Angular.

## les routes 

```js
loginComponent.ts

import { Component } from '@angular/core';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {
  // Ajoutez ici la logique de connexion, si nécessaire
}

loginComponent.html

<!-- Le template HTML du composant LoginComponent -->
<h2>Connectez-vous</h2>
<!-- Ajoutez ici les champs de connexion et le bouton de connexion -->


albumDescriptionComponent.ts

import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { Album, AlbumService } from '../album.service';

@Component({
  selector: 'app-album-description',
  templateUrl: './album-description.component.html',
  styleUrls: ['./album-description.component.css']
})
export class AlbumDescriptionComponent implements OnInit {
  album: Album;

  constructor(
    private route: ActivatedRoute,
    private albumService: AlbumService
  ) {}

  ngOnInit() {
    this.route.params.subscribe(params => {
      const albumId = params['id'];
      this.album = this.albumService.getAlbumById(albumId);
    });
  }
}

```html
albumDescriptionComponent.html

Le template du composant AlbumDescriptionComponent 
<div *ngIf="album">
  <h2>{{ album.name }}</h2>
  <p>Artiste: {{ album.artist }}</p>
  <p>Année de sortie: {{ album.year }}</p>
  <!-- Ajoutez d'autres détails de l'album ici -->
</div>

<div *ngIf="!album">
  <p>L'album demandé n'a pas été trouvé.</p>
</div>
```
```js
appModule.ts

const albumsRoutes: Routes = [
  // ...
  { path: 'login', component: LoginComponent },
  { path: 'album/:id', component: AlbumDescriptionComponent },
];


Bien sûr ! La logique de connexion dans le composant `LoginComponent` consistera généralement à gérer les entrées de l'utilisateur, à valider les informations de connexion et à effectuer des actions appropriées en fonction du résultat de la validation. Pour cet exemple, nous allons simuler la logique de connexion en utilisant une variable de connexion factice et une fonction de connexion.

Voici comment vous pouvez ajouter la logique de connexion dans le `LoginComponent` :

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {
  username: string;
  password: string;
  isLoggedIn: boolean = false;

  constructor() {}

  login() {
    // Pour cet exemple, nous supposerons que les informations de connexion correctes sont 'admin' pour le nom d'utilisateur et 'password' pour le mot de passe.
    // En pratique, vous devrez interagir avec un service d'authentification et vérifier les informations de connexion de manière sécurisée.
    if (this.username === 'admin' && this.password === 'password') {
      this.isLoggedIn = true;
      // Vous pouvez également effectuer une redirection vers une autre page après la connexion réussie.
    } else {
      alert('Nom d\'utilisateur ou mot de passe incorrect.');
    }
  }

  logout() {
    this.isLoggedIn = false;
    this.username = '';
    this.password = '';
  }
}
```

Explications :

1. `username` et `password`: Ce sont les propriétés du composant qui contiendront les valeurs entrées par l'utilisateur dans les champs de connexion du template.

2. `isLoggedIn`: Cette propriété est utilisée pour suivre si l'utilisateur est actuellement connecté ou déconnecté. Nous l'initialisons à `false` car l'utilisateur n'est pas connecté au début.

3. `login()`: C'est la méthode de connexion. Nous supposons ici que les informations de connexion correctes sont un nom d'utilisateur `'admin'` et un mot de passe `'password'`. En pratique, vous devrez interagir avec un service d'authentification pour valider les informations de connexion de manière sécurisée. Si les informations de connexion sont correctes, nous mettons `isLoggedIn` à `true` pour indiquer que l'utilisateur est connecté. Vous pouvez également effectuer une redirection vers une autre page après une connexion réussie.

4. `logout()`: C'est la méthode de déconnexion. Lorsqu'elle est appelée, nous mettons `isLoggedIn` à `false` pour indiquer que l'utilisateur est déconnecté, et nous effaçons également les valeurs des propriétés `username` et `password`.

Vous pouvez ensuite utiliser ces méthodes dans le template `login.component.html` pour gérer les actions de connexion et de déconnexion :

```html
<h2>Connectez-vous</h2>
<!-- Utilisez ngIf pour afficher le formulaire de connexion uniquement si l'utilisateur n'est pas connecté -->
<div *ngIf="!isLoggedIn">
  <form (ngSubmit)="login()">
    <label for="username">Nom d'utilisateur:</label>
    <input type="text" name="username" [(ngModel)]="username">

    <label for="password">Mot de passe:</label>
    <input type="password" name="password" [(ngModel)]="password">

    <button type="submit">Se connecter</button>
  </form>
</div>

<!-- Utilisez ngIf pour afficher un message de bienvenue et le bouton de déconnexion si l'utilisateur est connecté -->
<div *ngIf="isLoggedIn">
  <p>Bienvenue, {{ username }} !</p>
  <button (click)="logout()">Se déconnecter</button>
</div>
```

En utilisant cette logique de connexion simulée, le formulaire de connexion s'affichera lorsqu'un utilisateur n'est pas connecté. S'il entre les informations de connexion correctes et clique sur "Se connecter", il sera accueilli par un message de bienvenue et un bouton pour se déconnecter. Si les informations de connexion sont incorrectes, une alerte s'affiche pour informer l'utilisateur que le nom d'utilisateur ou le mot de passe est incorrect.

Notez que cette implémentation est pour une démonstration basique et n'est pas sécurisée pour une utilisation réelle. Dans une application réelle, vous devrez implémenter une logique d'authentification plus robuste et sécurisée.


Pour créer une méthode qui permet de rendre aléatoire le tableau de chaînes "albumsLists", vous pouvez ajouter une fonction appelée "shuffleAlbumsLists()" dans le fichier "album-details.component.ts". Cette fonction prendra le tableau "albumsLists" et le mélange de manière aléatoire. Ensuite, vous pouvez lier cette fonction à un bouton dans le fichier "album-details.component.html" pour l'exécuter lorsque le bouton est cliqué.

Voici comment vous pouvez procéder :

1. Dans "album-details.component.ts":

```typescript
import { Component, OnInit, Input, OnChanges, Output, EventEmitter } from '@angular/core';
import { Album, List } from '../album';
import { AlbumService } from '../album.service';

@Component({
  selector: 'app-album-details',
  templateUrl: './album-details.component.html',
  styleUrls: ['./album-details.component.css']
})
export class AlbumDetailsComponent implements OnInit, OnChanges {
  // ... Autres propriétés et méthodes existantes ...

  shuffleAlbumsLists() {
    if (this.albumsLists && this.albumsLists.length > 1) {
      // Utilisation de l'algorithme de Fisher-Yates pour mélanger le tableau
      for (let i = this.albumsLists.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [this.albumsLists[i], this.albumsLists[j]] = [this.albumsLists[j], this.albumsLists[i]];
      }
    }
  }

  // ... Autres propriétés et méthodes existantes ...
}
```

2. Dans "album-details.component.html", ajoutez le bouton pour déclencher la méthode de mélange :

```html
<!-- Le reste du code HTML existant -->
<button class="w3-button w3-blue border-0 rounded fw-bold px-4 py-3 text-uppercase" (click)="shuffleAlbumsLists()">
  Mélanger les détails
</button>
```

Explication : La méthode "shuffleAlbumsLists()" utilise l'algorithme de Fisher-Yates pour mélanger de manière aléatoire les éléments du tableau "albumsLists". Lorsque le bouton est cliqué, la fonction est déclenchée et les détails sont mélangés de manière aléatoire. Assurez-vous que le bouton est placé à l'endroit approprié dans le HTML pour que l'utilisateur puisse cliquer dessus et déclencher le mélange des détails.

Bien sûr, je vais vous expliquer chaque ligne de la méthode "shuffleAlbumsLists()" en détail :

```typescript
shuffleAlbumsLists() {
  if (this.albumsLists && this.albumsLists.length > 1) {
```
Cette ligne définit la fonction "shuffleAlbumsLists()". Elle vérifie d'abord si "albumsLists" existe (c'est-à-dire s'il n'est pas nul ou indéfini) et s'il a plus d'un élément (longueur supérieure à 1). Si ces conditions sont satisfaites, le mélange peut être effectué. Si le tableau "albumsLists" est vide ou ne contient qu'un seul élément, il n'est pas nécessaire de le mélanger, donc la fonction s'arrête ici.

```typescript
  for (let i = this.albumsLists.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [this.albumsLists[i], this.albumsLists[j]] = [this.albumsLists[j], this.albumsLists[i]];
  }
```
Ceci est la partie principale du mélange utilisant l'algorithme de Fisher-Yates (ou Knuth Shuffle). L'idée est de parcourir le tableau de droite à gauche, en partant du dernier élément (index "length - 1") jusqu'au deuxième élément (index 1). Pour chaque élément, nous choisissons un index aléatoire "j" entre 0 et l'index actuel "i" (inclus). Cela garantit que chaque élément a une chance égale d'être échangé avec n'importe quel autre élément du tableau.

En utilisant la destructuration d'array, les éléments à l'index "i" et "j" sont échangés. Cela signifie que le contenu des deux positions est échangé dans le tableau "albumsLists", créant ainsi un mélange aléatoire.

Notez que l'index "0" est exclu du mélange car il n'y a pas besoin d'échanger le premier élément avec lui-même.

C'est ainsi que la méthode "shuffleAlbumsLists()" fonctionne pour mélanger aléatoirement le tableau "albumsLists". Une fois cette méthode appelée, l'ordre des éléments dans le tableau sera aléatoire, ce qui donne un ordre de détails aléatoire lorsque le composant est rendu dans le template.

Pour implémenter la méthode `logout()` dans le composant HTML et gérer l style de navigation actif et non actif, nous aurons besoin de quelques étapes. D'abord, nous allons modifier le modèle HTML du composant `login.component.html` pour inclure la fonctionnalité de déconnexion (`logout()`), puis nous allons ajouter une logique pour gérer la classe CSS pour la navigation active/non active.

Voici comment vous pouvez le faire :

1. Modifier le modèle HTML (`login.component.html`) pour inclure la déconnexion et gérer le style de navigation active/non active :

```html
<!-- Affichez le nom d'utilisateur connecté s'il est connecté -->
<div *ngIf="isLoggedIn">
  <p>Bienvenue, {{ username }} !</p>
  <button (click)="logout()">Déconnexion</button>
</div>

<!-- Afficher le formulaire de connexion s'il n'est pas connecté -->
<div *ngIf="!isLoggedIn">
  <label>Nom d'utilisateur:</label>
  <input type="text" [(ngModel)]="username">
  <label>Mot de passe:</label>
  <input type="password" [(ngModel)]="password">
  <button (click)="login()">Se connecter</button>
</div>

<!-- Navigation -->
<ul>
  <li [class.active]="isLoggedIn"><a routerLink="/">Accueil</a></li>
  <li [class.active]="!isLoggedIn"><a routerLink="/login">Connexion</a></li>
  <!-- Ajoutez d'autres liens de navigation ici -->
</ul>
```

2. Assurez-vous que vous avez inclus le module `FormsModule` dans le module de ce composant pour que la liaison de données (ngModel) fonctionne correctement. Vous pouvez l'ajouter dans le fichier du module associé au composant (`login.module.ts`) :

```typescript
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { LoginComponent } from './login.component';

@NgModule({
  declarations: [LoginComponent],
  imports: [FormsModule],
})
export class LoginModule {}
```

3. Dans votre fichier CSS (`login.component.css`) ou tout autre fichier de style, vous pouvez ajouter les styles pour la classe `active` afin de mettre en évidence la navigation active :

```css
/* Styles pour la navigation active */
ul li.active a {
  color: blue;
  font-weight: bold;
}
```

Assurez-vous que vous avez configuré le routage approprié dans votre application Angular pour que le composant `LoginComponent` soit affiché lorsque l'utilisateur accède à `/login`.

Avec cette implémentation, lorsque l'utilisateur est connecté, il verra un message de bienvenue avec un bouton de déconnexion. Sinon, il verra le formulaire de connexion. La classe CSS `active` sera appliquée au lien de navigation approprié en fonction de l'état de connexion de l'utilisateur.

N'oubliez pas que pour une application réelle, vous devrez implémenter une authentification sécurisée avec un service d'authentification approprié plutôt que de simplement comparer le nom d'utilisateur et le mot de passe dans le code comme dans cet exemple de code.

Pour implémenter une authentification sécurisée avec un service d'authentification approprié, nous allons suivre quelques étapes essentielles. Dans cet exemple, nous allons utiliser un service factice pour simuler l'authentification, mais en pratique, vous devez utiliser un service d'authentification réel qui communique avec un backend sécurisé pour gérer l'authentification.

1. Créez un service d'authentification (`auth.service.ts`) pour gérer l'authentification :

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class AuthService {
  private isLoggedIn: boolean = false;

  // Méthode de connexion. Vérifiez les informations d'identification et retournez une Promise (ou Observable) résolue si les informations sont valides.
  login(username: string, password: string): Promise<boolean> {
    return new Promise((resolve, reject) => {
      // Ici, vous devez effectuer une requête HTTP ou une vérification côté serveur pour valider les informations d'identification.
      // Pour cet exemple, nous utiliserons des informations d'identification factices.
      if (username === 'admin' && password === 'password') {
        this.isLoggedIn = true;
        resolve(true);
      } else {
        resolve(false);
      }
    });
  }

  // Méthode de déconnexion
  logout(): void {
    this.isLoggedIn = false;
  }

  // Méthode pour vérifier si l'utilisateur est connecté
  isAuthenticated(): boolean {
    return this.isLoggedIn;
  }
}
```

2. Dans votre composant `LoginComponent`, injectez le service d'authentification et utilisez-le pour gérer l'authentification et la déconnexion :

```typescript
import { Component } from '@angular/core';
import { AuthService } from './auth.service';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {
  username: string;
  password: string;

  constructor(private authService: AuthService) {}

  login() {
    this.authService.login(this.username, this.password)
      .then((isLoggedIn: boolean) => {
        if (isLoggedIn) {
          // Redirection vers une autre page après la connexion réussie (par exemple, la page d'accueil).
          // Vous devez configurer le routage dans votre application pour cela.
        } else {
          alert('Nom d\'utilisateur ou mot de passe incorrect.');
        }
      });
  }

  logout() {
    this.authService.logout();
  }

  // Méthode pour vérifier si l'utilisateur est connecté
  isAuthenticated(): boolean {
    return this.authService.isAuthenticated();
  }
}
```

3. Modifiez le modèle HTML (`login.component.html`) pour afficher les éléments en fonction de l'état de connexion :

```html
<div *ngIf="isAuthenticated(); else loginForm">
  <p>Bienvenue, {{ username }} !</p>
  <button (click)="logout()">Déconnexion</button>
</div>

<ng-template #loginForm>
  <label>Nom d'utilisateur:</label>
  <input type="text" [(ngModel)]="username">
  <label>Mot de passe:</label>
  <input type="password" [(ngModel)]="password">
  <button (click)="login()">Se connecter</button>
</ng-template>

<!-- Navigation -->
<ul>
  <li [class.active]="isAuthenticated()"><a routerLink="/">Accueil</a></li>
  <li [class.active]="!isAuthenticated()"><a routerLink="/login">Connexion</a></li>
  <!-- Ajoutez d'autres liens de navigation ici -->
</ul>
```

Avec cette implémentation, l'authentification est gérée par le service `AuthService`, qui utilise une méthode de connexion pour vérifier les informations d'identification. Si l'utilisateur est connecté, il verra un message de bienvenue et un bouton de déconnexion. Sinon, il verra le formulaire de connexion.

Encore une fois, rappelez-vous que ceci est un exemple simplifié pour expliquer le concept. En production, vous devrez mettre en œuvre une véritable authentification sécurisée avec des pratiques appropriées pour sécuriser les informations d'identification et les échanges de données entre le client et le serveur.

Pour implémenter la méthode "logout()" dans le fichier du composant HTML ("login.component.html"), ainsi que la gestion de la classe CSS pour indiquer la navigation active ou non active, vous pouvez utiliser les directives Angular comme suit :

1. Dans "login.component.html", ajoutez le bouton de déconnexion et gérez la navigation active/non active :

```html
<!-- Afficher le formulaire de connexion lorsque l'utilisateur n'est pas connecté -->
<div *ngIf="!isLoggedIn">
  <form>
    <!-- Vos champs d'entrée pour le nom d'utilisateur et le mot de passe ici -->

    <button class="w3-button w3-blue" (click)="login()">Se connecter</button>
  </form>
</div>

<!-- Afficher le message de bienvenue et le bouton de déconnexion lorsque l'utilisateur est connecté -->
<div *ngIf="isLoggedIn">
  <p>Bienvenue, {{ username }} !</p>
  <button class="w3-button w3-red" (click)="logout()">Se déconnecter</button>
</div>
```

2. Dans "login.component.css", vous pouvez ajouter une classe CSS pour mettre en évidence le bouton actif et non actif :

```css
/* Style pour les boutons */
.w3-button {
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

/* Style pour le bouton actif */
.w3-button.active {
  background-color: #4CAF50; /* Couleur verte */
  color: white;
}

/* Style pour le bouton non actif */
.w3-button:not(.active) {
  background-color: #f1f1f1; /* Couleur grise */
  color: black;
}
```

3. Enfin, dans "login.component.ts", implémentez la méthode "logout()" comme suit :

```typescript
logout() {
  this.isLoggedIn = false;
  this.username = '';
  this.password = '';
}
```

Explication : Dans le fichier HTML, nous utilisons les directives Angular `*ngIf` pour afficher les parties appropriées du contenu en fonction de l'état de connexion (`isLoggedIn`). Lorsque l'utilisateur n'est pas connecté, nous montrons le formulaire de connexion avec un bouton "Se connecter". Lorsqu'il est connecté, nous montrons un message de bienvenue avec un bouton "Se déconnecter".

Nous utilisons également les directives Angular `(click)` pour appeler les méthodes `login()` et `logout()` lorsque les boutons sont cliqués.

Dans le fichier CSS, nous avons défini des styles pour les boutons, ainsi qu'une classe `.active` pour mettre en évidence le bouton actif. Cette classe est appliquée au bouton "Se déconnecter" lorsque l'utilisateur est connecté, et le style appliqué en fonction de la classe active montre un fond vert pour le bouton actif et un fond gris pour le bouton non actif.

Assurez-vous que le fichier CSS est correctement lié au composant pour que les styles soient appliqués. Avec ces modifications, vous aurez une gestion simple de l'authentification avec une interface utilisateur de connexion et de déconnexion dans votre composant de connexion.

Pour rendre aléatoire le tableau de strings `albumsLists`, nous pouvons ajouter deux buttons dans le HTML de l'enfant `album-details.component.html`. Un bouton sera utilisé pour générer le mélange aléatoire du tableau, tandis que l'autre bouton permettra de cacher ou afficher la liste aléatoire.

1. Ajoutons les deux boutons dans le HTML de `album-details.component.html` :

```html
<div class="card" *ngIf="album">
    <!-- Le reste du contenu... -->

    <!-- Bouton pour générer un mélange aléatoire -->
    <button class="w3-button w3-blue border-0 rounded fw-bold px-4 py-3 text-uppercase"
            (click)="shuffleAlbumsLists()">
        Mélanger la liste
    </button>

    <!-- Bouton pour cacher/afficher la liste aléatoire -->
    <button class="w3-button w3-blue border-0 rounded fw-bold px-4 py-3 text-uppercase"
            (click)="toggleRandomList()">
        {{ showRandomList ? 'Cacher la liste aléatoire' : 'Afficher la liste aléatoire' }}
    </button>

    <!-- Liste aléatoire -->
    <ul class="list-group rounded-bottom m-0 border-0" *ngIf="showRandomList && randomAlbumsLists; else noRandomAlbumsLists">
        <li class="list-group-item list-group-item-action w3-blue border-top-0 border-bottom-1 fw-semibold"
            *ngFor="let detailsList of randomAlbumsLists"> {{ detailsList }} 
        </li>
    </ul>
    <!-- Au cas où nous n'avons pas de détails lists aléatoires -->
    <ng-template #noRandomAlbumsLists>
        <ul class="list-group rounded-bottom m-0 border-0">
            <li class="list-group-item list-group-item-action w3-blue border-top-0 border-bottom-1 fw-semibold">
                Pas de détails Lists aléatoires</li>
        </ul>
    </ng-template>
</div>
```

2. Maintenant, nous devons ajouter la logique dans le fichier `album-details.component.ts` pour gérer les actions des boutons et générer le mélange aléatoire :

```typescript
import { Component, OnInit, Input, OnChanges, Output, EventEmitter } from '@angular/core';
import { Album, List } from '../album';
import { AlbumService } from '../album.service';

@Component({
    selector: 'app-album-details',
    templateUrl: './album-details.component.html',
    styleUrls: ['./album-details.component.css']
})
export class AlbumDetailsComponent implements OnInit, OnChanges {

    // Classe Input permet de récupérer les données de l'enfant
    @Input() album!: Album;

    lists: List[] = [];

    albumsLists!: string[] | undefined;

    isPlaying: boolean = false;
    showRandomList: boolean = false; // Propriété pour afficher/cacher la liste aléatoire
    randomAlbumsLists: string[] = [];

    constructor(private albumService: AlbumService) { }

    ngOnInit() {
        // this.album;
    }

    ngOnChanges(): void {
        if (this.album !== undefined) {
            this.albumsLists = this.albumService.getAlbumList(this.album.id)?.list;
        }
    }

    @Output() onPlay: EventEmitter<Album> = new EventEmitter();

    play(album: Album) {
        this.onPlay.emit(album);
        this.isPlaying = !this.isPlaying;
    }

    // Méthode pour générer le mélange aléatoire de la liste
    shuffleAlbumsLists() {
        if (this.albumsLists) {
            this.randomAlbumsLists = this.shuffleArray([...this.albumsLists]);
        }
    }

    // Méthode pour afficher/cacher la liste aléatoire
    toggleRandomList() {
        this.showRandomList = !this.showRandomList;
    }

    // Méthode pour mélanger le tableau de façon aléatoire
    private shuffleArray(array: any[]) {
        for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
        }
        return array;
    }
}
```

La méthode `shuffleArray()` utilise l'algorithme de mélange de Fisher-Yates pour générer un mélange aléatoire de la liste `albumsLists`. Lorsque l'utilisateur clique sur le bouton "Mélanger la liste", la méthode `shuffleAlbumsLists()` est appelée, ce qui génère une nouvelle liste aléatoire. Le bouton "Afficher la liste aléatoire" permet de basculer l'affichage de la liste aléatoire générée. Si l'utilisateur clique dessus, la liste sera affichée, sinon, elle sera cachée.

```