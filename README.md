# Géomaticien senior open source

Un skill pour Claude et ChatGPT qui vous donne un **collègue géomaticien senior** à côté de vous : il répond à votre question SIG, puis vous dit comment un expert de l'open source l'aurait abordée pour que ça tienne dans le temps.

Un projet [Datando Labs](https://datando.fr/datando-labs/).

**[⬇ Télécharger le skill (.zip)](https://github.com/EmmanuelNdo/datando-labs-geomaticien-senior/releases/latest/download/geomaticien-senior-open-source.zip)**

---

## Ce que fait le skill

Vous posez une question de géomatique à votre assistant IA, comme d'habitude. Le skill change la façon dont il répond :

- **Il répond d'abord à la demande.** Vous demandez un script PyQGIS, vous repartez avec un script PyQGIS qui fonctionne.
- **Il ajoute l'avis du senior.** Contexte retenu, stack recommandée, pourquoi, pièges à éviter, palier suivant si le projet grossit.
- **Il dimensionne juste.** Trois niveaux (léger, standard, industriel) et le seuil qui fait passer de l'un à l'autre. Pas de GeoServer pour trois couches statiques, pas de GeoPackage partagé pour dix éditeurs.
- **Il signale les anti-patterns.** Calculs de surface en degrés, shapefiles dispersés, traitements mensuels faits à la main, base PostGIS sans index ni sauvegarde.
- **Il reste honnête.** L'open source a un coût en temps et en compétences. Si un outil propriétaire est plus adapté à votre contexte, il le dit.

Il se déclenche tout seul dès que vous parlez SIG, cartographie, PostGIS, QGIS, carto web, LiDAR, télédétection, collecte terrain, géocodage, routage ou migration depuis ArcGIS.

## Exemple

> **Vous :** Chaque mois je télécharge le cadastre, je le découpe sur ma commune et je fais une jointure avec notre fichier de parcelles.
>
> **Claude :** *(vous aide sur la jointure, puis)*
>
> **L'avis du senior**
> Ce que je recommande : automatiser avec ogr2ogr vers PostGIS, une requête SQL pour le découpage et la jointure, une tâche planifiée.
> Pourquoi : une heure gagnée chaque mois et plus d'erreur de manipulation.
> Les pièges à éviter : ...

## Installation

### Dans Claude (web, desktop)

1. [Téléchargez le fichier `geomaticien-senior-open-source.zip`](https://github.com/EmmanuelNdo/datando-labs-geomaticien-senior/releases/latest/download/geomaticien-senior-open-source.zip). Ne le décompressez pas.
2. Ouvrez les paramètres de Claude, rubrique **Skills**.
3. Importez le fichier .zip.
4. Vérifiez que le skill est activé, puis posez votre première question SIG.

Les skills nécessitent que l'exécution de code soit activée dans vos paramètres.

### Dans ChatGPT

Le skill suit le format standard des skills (un dossier avec un `SKILL.md`) : il fonctionne aussi dans ChatGPT.

1. [Téléchargez le fichier `geomaticien-senior-open-source.zip`](https://github.com/EmmanuelNdo/datando-labs-geomaticien-senior/releases/latest/download/geomaticien-senior-open-source.zip). Ne le décompressez pas.
2. Dans ChatGPT, ouvrez **Plugins**, puis l'onglet **Skills**.
3. Cliquez sur **Ajouter**, puis sur **Importer depuis votre ordinateur**.
4. Sélectionnez le fichier .zip téléchargé.
5. Vérifiez que le skill est activé, puis posez votre première question SIG.

### Dans Claude Code

Copiez le dossier du skill dans vos skills personnels :

```bash
git clone https://github.com/EmmanuelNdo/datando-labs-geomaticien-senior.git
cp -r datando-labs-geomaticien-senior/geomaticien-senior-open-source ~/.claude/skills/
```

Pour le réserver à un seul projet, copiez-le plutôt dans `.claude/skills/` à la racine du projet.

## Contenu

```text
geomaticien-senior-open-source/
├── SKILL.md                      # Rôle, méthode de réponse, principes et anti-patterns
└── references/
    ├── stacks-par-besoin.md      # Stacks recommandées par domaine, en trois niveaux
    ├── migration-esri.md         # Correspondance Esri / open source et migration progressive
    └── donnees-france.md         # Sources de données et services ouverts de référence en France
```

Claude ne charge les fichiers de `references/` que lorsqu'il en a besoin : le skill reste léger dans la conversation.

## Personnaliser le skill

Le skill est un simple dossier de fichiers Markdown. Vous pouvez l'adapter à votre structure :

- ajouter votre stack interne dans `references/stacks-par-besoin.md` ;
- ajouter vos référentiels métier ou régionaux dans `references/donnees-france.md` ;
- changer le ton ou le format de réponse dans `SKILL.md`.

Zippez ensuite le dossier `geomaticien-senior-open-source` et réimportez-le.

## Contribuer

Une stack manquante, un piège classique oublié, une source de données ouverte à ajouter ? Ouvrez une issue ou une pull request. Gardez l'esprit du skill : des recommandations argumentées, dimensionnées au contexte, sans numéros de version cités de mémoire.

## Auteur

Conçu par [Emmanuel Ndofunsu](https://datando.fr), formateur en géomatique et IA, auteur de *L'IA générative pour les géomaticiens* (D-BookeR, 2026).

Pour aller plus loin avec l'IA dans votre service SIG : [datando.fr](https://datando.fr).

## Licence

[MIT](LICENSE) : vous pouvez l'utiliser, le modifier et le partager librement.

## Publier une nouvelle version

Modifiez le skill, changez le numéro dans le fichier `VERSION`, puis poussez sur `main`. Le workflow GitHub Actions reconstruit le .zip et publie la release : le lien de téléchargement du README pointe toujours vers la dernière version.
