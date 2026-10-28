# Hp Printer Card (FR)

Carte Home Assistant pour imprimante : **feuille animée** pendant l'impression, **niveaux d'encre ou de toner**, **alerte de cartouche faible**, message d'état de l'imprimante et compteurs de pages. Style verre, **thème clair/sombre automatique**, éditeur visuel, textes en français (et 12 autres langues).

> Dérivée de [ha-printer-card](https://github.com/ADNPolymerase/ha-printer-card) d'**ADNPolymerase** (licence MIT). La logique de la carte (détection des cartouches, états, traductions, illustrations) vient du projet d'origine ; cette version change le nom (`hp-printer-card`), l'apparence (verre, thème clair/sombre) et l'éditeur.

## Fonctionnalités

- **Feuille animée** : pendant l'impression, une page sort en boucle et s'estompe, la LED clignote. En cas de bourrage, la feuille reste coincée avec un triangle d'avertissement. `prefers-reduced-motion` est respecté.
- **Niveaux d'encre ou de toner** en cartouches, en barres ou dessinés dans l'imprimante, avec les bacs de maintenance et les pièces d'usure (tambour, fusible…).
- **Alerte de cartouche faible** (20 % par défaut, ou le seuil de l'imprimante s'il est plus élevé), séparée pour les encres et les pièces d'usure.
- **Message d'état** normalisé (prêt, impression, veille, avertissement, arrêt, hors ligne), avec les motifs de l'imprimante traduits (manque de papier, encre faible…).
- **Compteurs** : pages imprimées, numérisées, copiées et fax, avec le détail noir et blanc / couleur.
- **Thème** : suit le thème clair/sombre de Home Assistant, ou forcé avec `theme`.

## Installation via HACS (dépôt personnalisé)

1. HACS → ⋮ (en haut à droite) → **Dépôts personnalisés**.
2. Collez l'URL de ce dépôt, choisissez la catégorie **Tableau de bord** (Dashboard), puis **Ajouter**.
3. Recherchez « Hp Printer Card (FR) » et cliquez sur **Télécharger**.
4. Rechargez le navigateur (Ctrl+Maj+R).

Installation manuelle : copiez `hp-printer-card.js` dans `/config/www/` et ajoutez `/local/hp-printer-card.js` comme ressource de type **Module JavaScript**.

## Utilisation

Ajoutez une carte, cherchez « Hp Printer Card », puis choisissez l'imprimante dans l'éditeur visuel. En YAML, le minimum est :

```yaml
type: custom:hp-printer-card
entity: sensor.hp_color_laserjet_mfp_m277dw
```

Exemple plus complet :

```yaml
type: custom:hp-printer-card
entity: sensor.hp_color_laserjet_mfp_m277dw
name: Imprimante du bureau
printer_type: laser
plug_entity: switch.prise_imprimante
power_entity: sensor.prise_imprimante_puissance
printing_watts: 30
theme: auto
```

`entity` est le capteur d'état de l'imprimante (`sensor.<imprimante>` pour l'intégration IPP, `sensor.<imprimante>_status` pour Brother). Les cartouches et les compteurs sont détectés automatiquement sur le même appareil.

## Options

| Option | Rôle |
|---|---|
| `entity` | Capteur d'état de l'imprimante (**obligatoire**) |
| `name` | Titre de la carte |
| `theme` | `auto` (suit Home Assistant, par défaut), `light` ou `dark` |
| `cartridges`, `counters` | Consommables et compteurs : identifiants ou objets (`entity`, `name`, `color`, `kind`). Vide = détection automatique |
| `extra_entities` | Entités supplémentaires affichées sous l'état |
| `plug_entity` | Prise (`switch` ou `input_boolean`). Éteinte = imprimante hors ligne |
| `power_entity`, `printing_watts` | Capteur de puissance (W) et seuil au-delà duquel l'imprimante est considérée en impression |
| `paper_entity` | Capteur de bac à papier |
| `print_entity` | Bouton de test d'impression (`button`, `input_button`, `script`, `switch`) |
| `web_url` | Lien vers l'interface web de l'imprimante (`auto` possible) |
| `shop_url` | Lien d'achat des cartouches, `{ref}` est remplacé par le modèle |
| `low_threshold` | Seuil de niveau bas en % (20 par défaut) |
| `full_threshold` | Seuil de plein d'un bac de déchets (`kind: waste_fill`), 90 par défaut |
| `image` | Photo de votre imprimante à la place du dessin (ex. `/local/imprimante.jpg`) |
| `printer_type` | `mfp` (défaut), `inkjet`, `laser` ou `office` |
| `cartridge_style` | `cartridges` (défaut), `bars` ou `inside` (dans l'imprimante) |
| `cartridge_order` | `color` (défaut) ou `level` (les plus vides d'abord) |
| `cartridge_rows` | `auto` (défaut), `1`, `2` ou `3` |
| `counters_align` | `left` (défaut) ou `center` |
| `name_position` | `below` (défaut) ou `top` |
| `state_map` | Correspondance personnalisée entre l'état brut et l'état affiché |
| `compact` | Mode compact avec une icône colorée |
| `show_supplies`, `show_parts`, `show_counters`, `show_message`, `show_power` | Masquer ou afficher chaque section (tout est affiché par défaut) |
| `more_info` | Ouvrir la fiche de l'entité au clic (activé par défaut) |
| `language` | Forcer la langue : `en`, `fr`, `de`, `es`, `it`, `nl`, `pt`, `sv`, `no`, `da`, `pl`, `ru`, `zh` |

## Apparence : nuance de couleur

Dans l'éditeur visuel, à côté du thème :

| Option | Valeurs |
|---|---|
| `nuance` | `theme` (par défaut), `braise`, `ocean`, `lagon`, `foret`, `lavande`, `framboise`, `ambre`, `ardoise`, `perso` |
| `nuance_perso` | Couleur au choix (sélecteur de couleur), utilisée avec `nuance: perso` |
| `intensite` | De `0` (neutre) à `2` (très coloré), `1` par défaut |

La nuance teinte le dégradé de fond (et la couleur d'accent quand la carte en a une). Les couleurs d'état (vert, orange, rouge…) ne changent pas.

**Une seule nuance pour toutes les cartes Johan854** : laisse `nuance: theme` et ajoute à ton thème Home Assistant :

```yaml
carte-nuance: "#2f8fd8"
```

Sans cette ligne, `nuance: theme` garde les couleurs d'origine de la carte.

## Historique des versions

- **1.1.0** : nuance de couleur au choix (8 nuances, couleur personnalisée, intensité, ou nuance commune via `carte-nuance` dans le thème).
- **1.0.0** : première version de cette carte, dérivée de `ha-printer-card` 1.1.2 : nouveau nom (`hp-printer-card`), style verre, thème clair/sombre automatique avec option `theme`, éditeur visuel complété.

## Licence

MIT. Projet d'origine : [ADNPolymerase/ha-printer-card](https://github.com/ADNPolymerase/ha-printer-card), également sous licence MIT. Voir le fichier `LICENSE`.
