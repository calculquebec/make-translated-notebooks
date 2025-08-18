# Compilation de notebooks traduits

## Fichiers sources

* Les fichiers à éditer sont dans `src/`.

### Métadonnées des cellules

Dans Jupyter Lab, à la droite d'un notebook, il y a un bouton
de roues dentées pour faire afficher le "Property Inspector".
C'est dans "ADVANCED TOOLS" qu'il faut éditer les métadonnées.

* (Obligatoire) Langue(s) de la cellule (de code ou Markdown) :
  * Anglais :  `"lang": "en"`
  * Français : `"lang": "fr"`
  * Les deux : `"lang": "en,fr"`
* Différencier la version à remplir et le solutionnaire :
  * Exercice : `"tags": ["exer"]`
  * Solution : `"tags": ["soln"]`
  * Les deux versions : par défaut, donc ne rien mettre

## Fichiers compilés

Les fichiers compilés et à utiliser pendant un atelier sont dans :

* `en` et `fr` pour les versions à remplir;
* `solution-en` et `solution-fr` pour les solutionnaires.

### Compilation

* Compilation des dernières modifications : `python make.py`
* Recompilation complète (rebuild) : `python make.py -r`
