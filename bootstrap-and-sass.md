# Utiliser Bootstrap avec Sass

---

Premièrement, il faut récupérer les fichiers à l'adresse [Source files](https://getbootstrap.com/docs/5.3/getting-started/download/)

## Structure de fichiers

Pour pouvoir utiliser correctement sass et bootstrap, Bootstrap préconise de créer la strucutre de fichier suivante afin de travailler correctement avec sass

Pour ne pas dénaturer les fichiers scss bootstrap, on sépare les sources venant de bootstrap dans un répertoire à part, `node-modules`, et on crée son fichier scss customisé dans un répertoire à la racine `scss`.

On reproduit l'arborescence que l'on aurait si on utilisait un gestionnaire de paquet tel que npm. 
Si vous avez installé bootstrap avec npm sur votre projet, la structure sera comme cela par défaut :

```
votre-projet/
|── css/
├── scss/
│   |── custom-01.scss
│   └── custom-02.scss
└── node_modules/
│   └── bootstrap/
│       ├── js/
│       └── scss/
└── index.html
```

## Customiser Bootstrap

Il existe deux méthodes préconisées pour faire ceci, 
- soit on part du boostrap dans sont entièreté et on ne fait qu'ajouter du css à la fin
- Soit on importe les fichiers nécessaire et on surcharge les variables et les map à des endroits précis, puis on ajoute son css customisé

### Partir de boostrap

La marche à suivre et les imports nécéssaires sont indiqués dans le fichier `custom-01.scss`

### Customisation totale

La marche à suivre et les imports nécéssaires sont indiqués dans le fichier ```custom-02.scss```

## Compiler le css

Pour pouvoir compiler sass, il fait qu'il soit installé sur votre système.
Le moyen le plus simple est d'utiliser npm.
Pour cela, il faut télcharger [nodejs](https://nodejs.org/en/download/) et l'installer, sous windows je préfère utiliser le .msi

Une fois que c'est fait, dans votre projet il faut ouvrir un terminal.
vous pouvez vérifier les version de npm et nodejs en tapane les commandes suivantes : 
```
# vérifier la version de npm
npm -v #devrait retourner (à a date du 24/01/2025) v22.13.1
 "vérifier la version de nodejs"
node -v #devrait retourner (à a date du 24/01/2025) v10.9.2
```

Une fois que c'est OK, il faut installer sass, si vous travaillez régulièrement sur des projet qui utilisent sass, il est conseillé de l'installer globalement.

Si sass est installé globalement, à chaque nouveau projet il sera utilisable directement sans avoir à refaire l'installation via le terminal pour chaque projet.

Dans le terminal on écrit la commande suivante :
```
# installer sass globalement
npm install -g sass
```
Enfin, pour compiler les fichiers sass en un fichier css, il faut utiliser des commandes.

Vous pouvez :
- soit lancer la commande au besoin dans le terminal
- soit demander à sass de surveiller le fichier scss sur lequel vous travaillez, de fait, à chaque sauvegarde du fichier, sass lancera la compilation et surtout vous retournera les erreurs de compilation  qu'il pourrait apparaître, vous aidant ainsi a rapidement débugger le script.

### Compiler à la demande

Dans le cas par exemple du fichier custom-01.scss
`sass ./scss/custom-01.scss ./css/custom-01.css`

Dans le cas par exemple du fichier custom-02.scss
`sass ./scss/custom-02.scss ./css/custom-02.css`

### Surveiller le fichier et compilation à la sauvegarde

il suffit d'ajouter --watch entre sass et le fichier scss

Dans le cas par exemple du fichier custom-01.scss
`sass --watch ./scss/custom-01.scss ./css/custom-01.css`

Dans le cas par exemple du fichier custom-02.scss
`sass --watch ./scss/custom-02.scss ./css/custom-02.css`

## Sass

Sass est donc un langage qui ressemble à du CSS mais qui va être compilé afin de générer le CSS final d'un site.

Plutôt que d'écrire toutes les parties du CSS dans un seul fichier, on va séparer les différents éléments du CSS.

Grace à la compilation, il sera possible d'écrire une boucle qui permettra de compiler toutes les itérations ou toutes les dérivation d'une classe.

Par exemple, les classe `alert` et `alert-*` de bootstrap sont créées à l'aide d'une boucle

sass utilise la map suivante (_exempl.scss) : 
```scss
// scss-docs-start theme-colors-map
$theme-colors: (
  "primary":    $primary,
  "secondary":  $secondary,
  "success":    $success,
  "info":       $info,
  "warning":    $warning,
  "danger":     $danger,
  "light":      $light,
  "dark":       $dark
) !default;
```

et compile la suite des différents étét des alert à l'aide de cette boucle (_alert.scss) :

```scss
// scss-docs-start alert-modifiers
// Generate contextual modifier classes for colorizing the alert
@each $state in map-keys($theme-colors) {
  .alert-#{$state} {
    --#{$prefix}alert-color: var(--#{$prefix}#{$state}-text-emphasis);
    --#{$prefix}alert-bg: var(--#{$prefix}#{$state}-bg-subtle);
    --#{$prefix}alert-border-color: var(--#{$prefix}#{$state}-border-subtle);
    --#{$prefix}alert-link-color: var(--#{$prefix}#{$state}-text-emphasis);
  }
}
// scss-docs-end alert-modifiers
```

et cela le css suivant (css-exemple.css) :
```css
.alert-primary {
  --bs-alert-color: var(--bs-primary-text-emphasis);
  --bs-alert-bg: var(--bs-primary-bg-subtle);
  --bs-alert-border-color: var(--bs-primary-border-subtle);
  --bs-alert-link-color: var(--bs-primary-text-emphasis);
}

.alert-secondary {
  --bs-alert-color: var(--bs-secondary-text-emphasis);
  --bs-alert-bg: var(--bs-secondary-bg-subtle);
  --bs-alert-border-color: var(--bs-secondary-border-subtle);
  --bs-alert-link-color: var(--bs-secondary-text-emphasis);
}

.alert-success {
  --bs-alert-color: var(--bs-success-text-emphasis);
  --bs-alert-bg: var(--bs-success-bg-subtle);
  --bs-alert-border-color: var(--bs-success-border-subtle);
  --bs-alert-link-color: var(--bs-success-text-emphasis);
}

.alert-info {
  --bs-alert-color: var(--bs-info-text-emphasis);
  --bs-alert-bg: var(--bs-info-bg-subtle);
  --bs-alert-border-color: var(--bs-info-border-subtle);
  --bs-alert-link-color: var(--bs-info-text-emphasis);
}

.alert-warning {
  --bs-alert-color: var(--bs-warning-text-emphasis);
  --bs-alert-bg: var(--bs-warning-bg-subtle);
  --bs-alert-border-color: var(--bs-warning-border-subtle);
  --bs-alert-link-color: var(--bs-warning-text-emphasis);
}

.alert-danger {
  --bs-alert-color: var(--bs-danger-text-emphasis);
  --bs-alert-bg: var(--bs-danger-bg-subtle);
  --bs-alert-border-color: var(--bs-danger-border-subtle);
  --bs-alert-link-color: var(--bs-danger-text-emphasis);
}

.alert-light {
  --bs-alert-color: var(--bs-light-text-emphasis);
  --bs-alert-bg: var(--bs-light-bg-subtle);
  --bs-alert-border-color: var(--bs-light-border-subtle);
  --bs-alert-link-color: var(--bs-light-text-emphasis);
}

.alert-dark {
  --bs-alert-color: var(--bs-dark-text-emphasis);
  --bs-alert-bg: var(--bs-dark-bg-subtle);
  --bs-alert-border-color: var(--bs-dark-border-subtle);
  --bs-alert-link-color: var(--bs-dark-text-emphasis);
}
```

Voir l'exemple dans `css-exemple.scss`, qu'on compile grâce à la commande 
`sass ./scss/css-exemple.scss ./css/css-exemple.css`
Attention, dans cet exemple sont repris toutes les variables nécessaires pour que la compilation se passe bien.