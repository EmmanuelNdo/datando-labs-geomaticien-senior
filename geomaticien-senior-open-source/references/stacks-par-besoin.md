# Stacks open source recommandées par besoin

Sommaire :
1. Cartographie web
2. Base de données spatiale
3. Traitements et ETL
4. Raster et télédétection
5. LiDAR et nuages de points
6. 3D
7. Collecte terrain
8. Routage et analyse de réseau
9. Géocodage
10. Catalogue et métadonnées
11. Versionnage et travail collaboratif
12. GeoAI et IA appliquée
13. Déploiement et exploitation

Pour chaque besoin : trois niveaux (léger, standard, industriel) et le signal qui fait passer au niveau suivant. Ces listes sont un point de départ, pas un dogme : adaptez au contexte. Vérifiez les versions et la maintenance active des projets avant une recommandation engageante.

---

## 1. Cartographie web

**Léger** (données peu nombreuses, peu mises à jour, pas de serveur)
- Données : GeoJSON pour quelques centaines d'objets, FlatGeobuf ou PMTiles au-delà
- Génération de tuiles : Tippecanoe, ou export depuis QGIS
- Affichage : MapLibre GL JS (tuiles vectorielles, style fin), Leaflet (simple, très répandu), OpenLayers (le plus complet côté SIG, projections, OGC)
- Hébergement : un simple hébergement de fichiers statiques
- Bascule : dès que les données changent souvent ou que plusieurs personnes les éditent

**Standard** (référentiel vivant, édition dans QGIS, publication de services)
- PostGIS comme source unique
- QGIS Server + Lizmap : le géomaticien publie directement son projet QGIS en carte web, avec le style QGIS. Très adapté aux collectivités et aux équipes QGIS
- Ou GeoServer : très complet sur les standards OGC, adapté si la DSI le maîtrise déjà ou s'il faut beaucoup de services WMS/WFS/WMTS
- Tuiles vectorielles depuis PostGIS : Martin ou pg_tileserv ; API d'objets : pg_featureserv
- Bascule : grand public, forte charge, besoin d'une infrastructure de données partagée entre structures

**Industriel** (infrastructure de données géographiques, grand public, forte charge)
- PostGIS répliqué, GeoServer ou QGIS Server derrière un cache de tuiles (GeoWebCache, MapProxy)
- Catalogue GeoNetwork, portail de type geOrchestra ou Mviewer
- Tuiles pré-générées sur stockage objet et CDN pour les fonds de plan

Piège fréquent : servir en WFS des milliers d'objets complexes au navigateur. Préférez les tuiles vectorielles pour l'affichage et le WFS pour le téléchargement ou l'édition.

## 2. Base de données spatiale

- **Mono-utilisateur, échange** : GeoPackage (un fichier, standard OGC, lu partout)
- **Analyse sur fichiers volumineux** : DuckDB avec l'extension spatial, format GeoParquet
- **Multi-utilisateurs, référentiel** : PostgreSQL + PostGIS. Extensions utiles selon le besoin : pgRouting (réseaux), h3-pg (grilles hexagonales), pgpointcloud (nuages de points), pg_cron (tâches planifiées)
- Bonnes pratiques : un schéma par thématique, des rôles distincts lecture et écriture, contraintes de type et de SRC sur les géométries, index GiST, `VACUUM ANALYZE` après gros imports, sauvegarde pg_dump planifiée et testée
- Projets QGIS : les stocker dans PostgreSQL permet de partager un projet unique à toute l'équipe
- Connexions : fichier de service PostgreSQL (`pg_service.conf`) plutôt que des identifiants dans les projets

## 3. Traitements et ETL

- **Ponctuel, visuel** : boîte à outils de traitements QGIS
- **Répétitif, sans code** : modeleur graphique QGIS, exécutable en ligne de commande avec `qgis_process`
- **Scripté** : GDAL/OGR (`ogr2ogr`, `gdalwarp`, `gdal_translate`), PyQGIS pour piloter QGIS, Python avec GeoPandas, Shapely, pyogrio, Rasterio
- **Dans la base** : SQL PostGIS, souvent le plus rapide pour les jointures et découpages sur gros volumes
- **R** : sf et terra, pertinents pour les profils statistiques
- **Planification** : cron ou planificateur de tâches pour commencer, un orchestrateur seulement si les chaînes se multiplient
- Alternative open source aux ETL graphiques propriétaires : combiner modèles QGIS, scripts Python et SQL versionnés. Le senior ne promet pas un équivalent clé en main de FME

## 4. Raster et télédétection

- Formats : COG (Cloud Optimized GeoTIFF) pour la diffusion, VRT pour assembler sans dupliquer
- Outils : GDAL, Rasterio, xarray et rioxarray pour les séries temporelles, Orfeo ToolBox (CNES) pour la télédétection avancée, extension SCP dans QGIS pour la classification accessible
- Accès aux images : catalogues STAC (pystac-client), Copernicus Data Space Ecosystem pour Sentinel
- Diffusion : TiTiler pour servir des COG en tuiles dynamiques, ou GeoServer
- Piège : télécharger des scènes entières quand une lecture partielle d'un COG suffit

## 5. LiDAR et nuages de points

- Formats : LAZ, COPC pour le streaming et la lecture partielle
- Traitement : PDAL (pipelines JSON reproductibles), visualisation et édition dans QGIS, analyse dans CloudCompare
- Diffusion web : Potree, ou la visualisation de nuages de points de Giro3D
- En France : données LiDAR HD de l'IGN (voir `donnees-france.md`)
- Piège : charger une dalle entière en mémoire au lieu de passer par des pipelines PDAL par tuiles

## 6. 3D

- Standard de diffusion : 3D Tiles (OGC Community Standard)
- Génération : py3dtiles
- Visualisation web : CesiumJS, iTowns (IGN), Giro3D
- Dans QGIS : vue 3D native pour l'exploration et les maquettes simples

## 7. Collecte terrain

- **QField** : le prolongement naturel d'un projet QGIS sur tablette ou smartphone, synchronisation via QFieldCloud ou transfert de fichiers
- **Mergin Maps** : alternative basée sur QGIS avec synchronisation intégrée
- **ODK Central ou KoboToolbox** : formulaires d'enquête avant tout, cartographie secondaire
- Conseil senior : concevoir le modèle de données et les listes de valeurs dans QGIS avant de partir sur le terrain, et prévoir le retour des données vers PostGIS

## 8. Routage et analyse de réseau

- **Dans la base** : pgRouting (isochrones, plus courts chemins, sur données métier)
- **Moteurs de calcul rapides sur OSM** : OSRM, Valhalla, GraphHopper
- **Dans QGIS** : outils d'analyse de réseau natifs pour les besoins ponctuels
- Piège : un réseau non topologique (tronçons non connectés) donne des résultats faux sans message d'erreur. Vérifier la topologie avant tout calcul

## 9. Géocodage

- En France : API Adresse de la Géoplateforme, basée sur la BAN, avec géocodage par fichier CSV
- Monde : Nominatim (sur OSM), Photon, ou addok auto-hébergé pour de gros volumes français
- Conseil senior : pour des volumes importants ou réguliers, auto-héberger plutôt que saturer un service public, et toujours conserver le score de géocodage pour filtrer les résultats douteux

## 10. Catalogue et métadonnées

- GeoNetwork : le catalogue de référence, conforme INSPIRE
- pycsw : service de catalogue léger
- Dans QGIS : l'éditeur de métadonnées de couche, au minimum

## 11. Versionnage et travail collaboratif

- Git pour les scripts, requêtes SQL, modèles QGIS et styles (QML, SLD)
- Kart pour versionner des données géographiques
- Projets QGIS stockés dans PostgreSQL pour l'édition partagée
- Documentation : un README par projet (sources, SRC, chaîne de traitement, qui maintient)

## 12. GeoAI et IA appliquée

- Segmentation et détection : segment-geospatial (samgeo), TorchGeo, extension Deepness dans QGIS
- Bibliothèques Python dédiées au GeoAI, à vérifier au cas par cas (maturité, maintenance)
- Avec un assistant comme Claude : faire générer requêtes PostGIS, scripts PyQGIS et pipelines GDAL en fournissant le contexte (SRC, structure des tables, volumes), puis relire et tester sur un échantillon avant la production
- Conseil senior : un modèle d'IA ne corrige pas une donnée mal structurée. La qualité du référentiel PostGIS reste la base

## 13. Déploiement et exploitation

- Conteneurs Docker pour PostGIS, GeoServer, QGIS Server et Lizmap : installation reproductible et mises à jour maîtrisées
- Reverse proxy (Nginx) devant les services, HTTPS systématique
- Sauvegardes automatisées et restauration testée
- Supervision minimale : espace disque, temps de réponse des services, journaux
- Conseil senior : avant de monter une infrastructure, demander qui l'exploitera dans deux ans. S'il n'y a personne, regarder les offres d'hébergement proposées par les éditeurs et intégrateurs open source
