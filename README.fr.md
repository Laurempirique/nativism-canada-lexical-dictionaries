# Dictionnaires lexicaux du nativisme — Canada (FR / EN)

*[English version](README.md)*

Ce dépôt contient deux dictionnaires d'expressions régulières (français et anglais) qui servent à repérer des **phrases nativistes** dans des programmes électoraux de partis politiques provinciaux canadiens. Ils ont été construits pour le mémoire de maîtrise *Le nativisme comme réponse au populisme : proposition conceptuelle et théorique mise à l'épreuve en contexte canadien* (UQAM, sociologie, 2026). Ils adaptent et prolongent le dictionnaire du nativisme développé par Habersack et Werner (2023) pour des programmes électoraux autrichiens, allemands et suisses.

---

## 1. Ce que les dictionnaires mesurent

### Définition du nativisme

Les dictionnaires opérationnalisent la définition du nativisme proposée dans le mémoire :

> Le nativisme est la perception que la diversité culturelle d'une société est trop grande et que l'État devrait entreprendre des politiques pour resserrer la culture autour d'un mythe national spécifique.

Dans cette perspective, la diversité culturelle d'une société se situe sur un **spectre allant du nativisme au multiculturalisme**. Ces deux pôles correspondent à des mouvements qui poussent chacun la société vers leur côté du spectre : le nativisme vers la convergence autour d'un mythe national, le multiculturalisme vers la reconnaissance et le maintien de la pluralité.

Ce qui situe une position du côté nativiste, c'est **l'exigence d'homogénéité culturelle autour de ce qui est censé distinguer la nation des autres nations**. Le contenu de cette distinction varie (langue, religion, histoire, voire valeurs libérales lorsqu'elles sont présentées comme ce qui distingue la nation). Le critère est la définition de la nation, non le contenu de la culture. Cette définition fait elle-même l'objet de débats entre les membres de la communauté politique; les dictionnaires mesurent les positions prises dans ce débat, pas son issue.

La définition est descriptive. Elle ne fixe aucun seuil de diversité « acceptable ». Elle sert à mesurer où les acteurs politiques se situent sur le spectre, la question de ce qui est souhaitable étant tranchée par le débat démocratique.

Une phrase est codée comme nativiste lorsqu'elle exprime l'une ou l'autre de ces idées : la diversité est excessive ou menaçante, la culture nationale doit être défendue ou affirmée, ou l'État doit agir pour réduire la diversité (par la politique culturelle, linguistique ou religieuse, ou par le contrôle de l'immigration).

<!-- À COMPLÉTER : ajouter le numéro de section ou de page du mémoire où la définition est développée. -->

### Dimensions (à titre indicatif)

Chaque motif est rattaché à une dimension (colonne `category`). **Ces dimensions ne servent qu'à organiser les motifs et n'ont pas été utilisées dans l'analyse du mémoire.** Seul le score global compte (voir la section 4).

| Dimension | Contenu |
|---|---|
| `cultural_dim` | Attachement à la culture, à l'identité, aux traditions et à l'histoire nationales; hostilité envers les « ennemis » de la nation; immigration perçue comme excessive; patriotisme; grand remplacement; déportation |
| `political_rights_dim` | Restriction de l'immigration, plafonds, révocation de statut, migrants « illégaux », refus d'intégration, culture nationale dominante, auto-ségrégation |
| `economic_dim` | Immigrants qui « prennent les emplois », fardeau fiscal, préférence nationale (« X first ») |
| `religious_dim` | Islam associé à l'immigration ou au danger, voile, charia, fondamentalisme, abattage rituel, etc. |
| `law_and_order_dim` | Groupes étrangers ou ethniques associés au crime; ordres juridiques parallèles |

---

## 2. Une mise en garde importante : ces dictionnaires ne sont pas universels

**Les dictionnaires doivent être adaptés au corpus sur lequel on les applique.** Ils ne fonctionnent pas de manière universelle.

Une approche lexicale repère des mots et des combinaisons de mots, pas du sens. Le même terme peut être nativiste dans un contexte et anodin dans un autre (« patrimoine » dans « notre patrimoine national » ou dans « patrimoine naturel »; « tradition » dans un discours identitaire ou dans « industries traditionnelles »). Pour obtenir des résultats valides, chaque motif a été ajusté au corpus étudié afin de :

- **réduire les faux positifs** : phrases repérées à tort. On les gère par des exclusions (*vetoes*), par exemple les références aux Premières Nations, aux industries culturelles, au patrimoine naturel ou au développement durable;
- **réduire les faux négatifs** : phrases nativistes manquées. On les gère en élargissant les termes ou en ajoutant des motifs propres aux débats du corpus.

Appliqués tels quels à un autre corpus (autre pays, autre époque, discours parlementaires, médias sociaux, presse), les dictionnaires produiront presque certainement des erreurs dans les deux sens. Toute réutilisation devrait inclure une **nouvelle validation manuelle** sur un échantillon du nouveau corpus (voir la section 5).

### Corpus d'origine

Les dictionnaires ont été adaptés aux **programmes électoraux de l'ensemble des partis politiques provinciaux, dans toutes les provinces, lors des élections ayant précédé l'élection fédérale canadienne du 21 octobre 2019**. Cette élection correspond à l'Étude électorale canadienne (CES) 2019 utilisée dans le mémoire.

Les programmes proviennent de la banque de documents politiques **Poltext** du Centre d'analyse des politiques publiques (CAPP) de l'Université Laval (https://www.poltext.org/). Les programmes eux-mêmes ne sont pas redistribués dans ce dépôt.

### Les dictionnaires français et anglais ne sont pas des traductions

Les deux dictionnaires ont été construits séparément et reflètent les débats propres à chaque espace linguistique. Le dictionnaire français, appliqué surtout aux programmes québécois, comprend des motifs liés à la langue française, à la francisation, à l'intégration, à la laïcité et aux accommodements raisonnables. Ces motifs n'ont pas d'équivalent direct dans le dictionnaire anglais. À l'inverse, le dictionnaire anglais contient des motifs propres au Canada anglais (« Keep Alberta strong », références à « the West »).

**Les scores produits par les deux dictionnaires ne sont donc pas directement comparables** sans une validation croisée.

---

## 3. Contenu du dépôt

| Fichier | Description |
|---|---|
| `nativism_dict_FR_regex.csv` | Dictionnaire français : `category`, `Regex`, `Concept` |
| `nativism_dict_ENG_regex.csv` | Dictionnaire anglais : `category`, `Regex`, `Concept`, `Rule` (description en clair de chaque motif et de ses exclusions) |

Notes :

- Dans le fichier français, certaines lignes n'ont pas de valeur dans `category`. Comme les dimensions ne servent pas au calcul du score, ces lignes comptent normalement dans le score global.
- Les motifs utilisent des drapeaux en ligne (`(?i)` insensible à la casse, `(?x)` mode étendu), des *lookaheads* positifs et négatifs, et des limites de mots (`\b`). <!-- À COMPLÉTER : moteur utilisé (ex. stringr / ICU dans R, ou PCRE avec perl = TRUE). -->

---

## 4. Utilisation et calcul du score

### Unité d'analyse : la phrase

Plusieurs motifs exigent que deux termes apparaissent « dans la même phrase ». Le texte doit donc être segmenté en phrases avant l'application des dictionnaires. Le dictionnaire de Habersack et Werner (2023) était appliqué aux quasi-phrases du MARPOR (Manifesto Project); ce dépôt utilise des phrases. <!-- À COMPLÉTER : méthode de segmentation. -->

### Score

- Une phrase repérée par au moins un motif compte pour **un seul point**. Si plusieurs motifs repèrent la même phrase, on n'ajoute pas de point.
- Le score d'un acteur politique est le **pourcentage de phrases nativistes** sur l'ensemble des phrases du corpus qui lui est associé :

  *score = (nombre de phrases nativistes / nombre total de phrases) × 100*

### Comparabilité des corpus

Le score est une proportion. Il n'est comparable entre acteurs que si leurs corpus sont équivalents :

- **Un document par acteur** (un programme par parti, par exemple) convient, à condition que les documents soient de taille comparable.
- **Les documents courts biaisent les résultats** : quelques phrases repérées suffisent à produire un pourcentage élevé, ou nul. Il peut être préférable d'écarter les programmes très courts ou de signaler leur taille.
- **Pour des corpus volumineux ou hétérogènes** (publications sur les réseaux sociaux, par exemple), il faut tirer un échantillon aléatoire de taille équivalente pour chaque acteur.

### Exemple minimal en R

```r
library(readr)
library(stringr)
library(purrr)

dict <- read_csv("nativism_dict_FR_regex.csv")

phrases <- c(
  "Nous protégerons notre identité et notre patrimoine.",
  "Le patrimoine naturel du Québec sera protégé.",
  "Nous réduirons les délais d'attente en santé."
)

# Une colonne logique par motif
hits <- map(dict$Regex, ~ str_detect(phrases, regex(.x)))
hits <- do.call(cbind, hits)

# Une phrase est nativiste si au moins un motif la repère (un seul point par phrase)
est_nativiste <- rowSums(hits) > 0

# Score : pourcentage de phrases nativistes
score <- mean(est_nativiste) * 100
```

---

## 5. Construction et validation

- **Point de départ.** Les dictionnaires adaptent au contexte canadien, et à la langue française, le dictionnaire du nativisme de Habersack et Werner (2023), conçu pour les programmes électoraux autrichiens, allemands et suisses.
- **Procédure.** Les motifs ont été raffinés par itération pendant environ deux mois. Chaque version a été appliquée au corpus, les phrases repérées et non repérées ont été lues, puis les motifs ont été corrigés jusqu'à ce qu'il ne reste plus de faux positifs ni de phrases nativistes manquées dans le corpus.
- **Limite.** Cette validation a été faite sur le même corpus que celui qui a servi à construire les dictionnaires. La performance mesurée est donc une performance *dans l'échantillon*, et elle surestime probablement la performance sur des textes nouveaux. Pour toute réutilisation, il est recommandé de coder manuellement un échantillon aléatoire du nouveau corpus et de mesurer la précision et le rappel.

---

## 6. Limites

- **Négation, ironie et citation.** Une phrase qui rapporte ou dénonce un discours nativiste (« nous refusons la rhétorique du grand remplacement ») peut être repérée. Certaines exclusions corrigent ce problème, mais pas toutes.
- **Choix de classification.** Certains motifs, surtout en français (langue française, francisation, cohésion sociale, valeurs communes), reposent sur l'idée que l'affirmation d'une culture nationale commune relève du pôle nativiste du spectre. Ce choix découle de la définition du mémoire. Un chercheur qui sépare nationalisme civique, interculturalisme et nativisme devrait retirer ou isoler ces motifs.
- **Peuples autochtones.** Plusieurs motifs excluent les références aux Premières Nations, aux Métis et aux Inuits. Le vocabulaire de la tradition, de l'appartenance et du territoire y a un sens différent, qui ne relève pas du nativisme au sens défini ici.
- **Contenu sensible.** Les dictionnaires contiennent, par construction, des termes offensants ou stigmatisants. Leur présence ne reflète pas les positions de l'auteur.

---

## 7. Citer

Si vous utilisez ces dictionnaires, veuillez citer le mémoire et l'article dont ils s'inspirent :

> <!-- À COMPLÉTER : Nom, Prénom --> (2026). *Le nativisme comme réponse au populisme : proposition conceptuelle et théorique mise à l'épreuve en contexte canadien*. Mémoire de maîtrise, Université du Québec à Montréal. <!-- lien Archipel -->

> Habersack, F. et Werner, A. (2023). How non-radical right parties strategically use nativist language: evidence from an automated content analysis of Austrian, German, and Swiss election manifestos. *Party Politics*, 29(5), 865–877. https://doi.org/10.1177/13540688221103930

Source du corpus : Poltext, Centre d'analyse des politiques publiques (CAPP), Université Laval. https://www.poltext.org/

---

## Licence

<!-- À COMPLÉTER : par exemple CC BY 4.0 pour les dictionnaires. -->

## Contact

<!-- À COMPLÉTER -->
