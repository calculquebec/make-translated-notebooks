# Compilation de notebooks traduits

## Fichiers sources

Vos notebooks bilingues doivent être dans un répertoire `src` à la racine du
dépôt Git.

### Métadonnées des cellules

Dans Jupyter Lab, à la droite d'un notebook, il y a un bouton
de roues dentées pour faire afficher le "Property Inspector".
C'est dans "ADVANCED TOOLS" qu'il faut éditer les métadonnées d'une cellule.

* (Obligatoire*) Langue(s) de la cellule (de code ou de Markdown) :
  * Anglais :  `"lang": "en"`
  * Français : `"lang": "fr"`
  * Les deux : `"lang": "en,fr"`
  * *Note : une cellule vide est automatiquement dans toutes les langues.
* Identifier les cellules à éditer (exercice) et les cellules de la solution :
  * Version à éditer seulement : `"tags": ["exer"]`
  * Version solution seulement : `"tags": ["soln"]`
  * Les deux versions (par défaut) : ne rien mettre.

Dans une section d'exercices de votre atelier,
un même bout de code peut se répéter en 2*2 cellules :

* Cellule de solution en français
* Cellule à éditer en français (avec des `###` ou tout vide)
* Cellule de solution en anglais
* Cellule à éditer en anglais (avec des `###` ou tout vide)

## Fichiers compilés

Les fichiers compilés et à utiliser pendant un atelier seront dans :

* `en` et `fr` pour les versions à éditer en atelier;
* `solution-en` et `solution-fr` pour les solutionnaires.

### Ajouter ce dépôt comme sous-module

Pour ajouter ce dépôt comme sous-module à votre matériel :

```
git submodule add https://github.com/calculquebec/make-translated-notebooks.git translation
git commit -m "Ajout du sous-module de traduction"
```

### Compilation des notebooks

À partir de votre matériel :

* Compilation des dernières modifications : `python translation/make.py`
* Recompilation complète (rebuild) : `python translation/make.py -r`

Dans un workflow GitHub (fichier `.github/workflows/make.yml`) :

```
name: Make exercise and solution notebooks

on:
  push:
    branches:
    - main
    paths:
    - 'src/**'

jobs:
  make_notebooks:
    runs-on: ubuntu-latest
    steps:
    # https://github.com/actions/checkout
    - name: Git checkout latest revision
      uses: actions/checkout@v4
      with:
        submodules: recursive

    # https://github.com/actions/setup-python
    - name: Set up Python 3.11
      uses: actions/setup-python@v5
      with:
        python-version: '3.11'

    - name: Rebuild all final notebooks
      run: |
        python translation/make.py -r

    - name: Commit any change
      run: |
        git config user.name "github-actions[bot]"
        git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
        git add en fr solution-{en,fr}
        MSG="Compilation de la révision ${{github.sha}}"
        ! git diff --cached --quiet && git commit -m "$MSG" && git push || git status
```
