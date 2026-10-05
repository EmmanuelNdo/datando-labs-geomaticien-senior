# Données et services ouverts de référence en France

Les noms de produits, les adresses et les modalités d'accès évoluent (notamment avec la Géoplateforme de l'IGN). Vérifiez l'accès actuel avant de donner une URL précise ou une procédure de téléchargement.

## Portails

- **cartes.gouv.fr** : portail de la Géoplateforme (IGN et partenaires), accès aux données et aux services de diffusion
- **data.gouv.fr** : plateforme nationale des données ouvertes
- **geo.api.gouv.fr** : API du découpage administratif (communes, départements, régions)

## Référentiels IGN

- **BD TOPO** : description vectorielle du territoire (bâti, routes, hydrographie, végétation...)
- **Admin Express** : limites administratives
- **BD ORTHO** : orthophotographies
- **RGE ALTI** : modèle numérique de terrain
- **LiDAR HD** : nuages de points et modèles dérivés sur le territoire national
- **BD Forêt** : occupation forestière
- **OCS GE** : occupation du sol à grande échelle

Diffusion : services WMTS, WMS et WFS de la Géoplateforme, utilisables directement dans QGIS. Pour les traitements lourds ou répétés, télécharger les données plutôt qu'interroger les flux en boucle.

## Foncier et adresses

- **Cadastre Etalab** : plan cadastral vectorisé en téléchargement (communes, départements)
- **PCI Vecteur** : plan cadastral informatisé de la DGFiP
- **DVF** : demandes de valeurs foncières
- **BAN** : Base Adresse Nationale, avec l'API Adresse pour le géocodage unitaire et par fichier CSV

## Autres sources utiles

- **RPG** : registre parcellaire graphique (parcelles agricoles déclarées)
- **Corine Land Cover** : occupation du sol européenne
- **Géorisques** : risques naturels et technologiques
- **INSEE** : données carroyées et statistiques infracommunales
- **SIRENE géolocalisé** : établissements
- **OpenStreetMap** : extraits régionaux (Geofabrik), requêtes ciblées (Overpass), import en base (osm2pgsql)

## Conseils senior

- Toujours noter la source, le millésime et la licence dans les métadonnées de la couche
- Les référentiels nationaux sont en Lambert-93 (EPSG:2154) en métropole ; les territoires ultramarins ont leurs propres SRC
- Pour un référentiel local vivant, importer la donnée nationale dans PostGIS et documenter la chaîne de mise à jour plutôt que de télécharger à la main à chaque millésime
