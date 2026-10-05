# Passer d'Esri à l'open source

## Correspondances

| Usage | Esri | Alternative open source |
|---|---|---|
| SIG bureautique | ArcGIS Pro | QGIS |
| Automatisation | ArcPy, ModelBuilder | PyQGIS, GeoPandas, modeleur graphique QGIS, `qgis_process` |
| Stockage mono-poste | File geodatabase | GeoPackage |
| Géodatabase d'entreprise | Enterprise geodatabase | PostgreSQL + PostGIS |
| Serveur cartographique | ArcGIS Server / Enterprise | GeoServer, QGIS Server, MapServer |
| Applications web | ArcGIS Online, Experience Builder, Dashboards | Lizmap, Mviewer, geOrchestra, applications MapLibre ou OpenLayers |
| Collecte terrain | Field Maps, Survey123 | QField, Mergin Maps, ODK, KoboToolbox |
| Catalogue | Portal (volet catalogue) | GeoNetwork |
| Raster et imagerie | Image Analyst | GDAL, Orfeo ToolBox, Rasterio, xarray |
| Réseau | Network Analyst | pgRouting, OSRM, Valhalla, outils réseau QGIS |
| 3D | Scene Viewer | CesiumJS, iTowns, Giro3D |

Lecture des données Esri : GDAL lit les file geodatabases (pilote OpenFileGDB) et les services ArcGIS REST, ce qui permet de migrer sans ArcGIS.

## Méthode de migration progressive

Le senior ne recommande presque jamais une bascule totale en une fois.

1. **Inventorier** : couches, volumes, projets, services publiés, traitements récurrents, applications, utilisateurs
2. **Commencer par la base** : migrer les référentiels vers PostGIS. ArcGIS Pro sait lire et éditer PostGIS : les deux mondes cohabitent pendant la transition
3. **Former les équipes** à QGIS avant de retirer les licences
4. **Migrer les traitements récurrents** en scripts ou modèles QGIS versionnés
5. **Republier les services** avec QGIS Server ou GeoServer, puis les applications web
6. **Garder quelques licences** si un besoin métier précis n'a pas d'équivalent satisfaisant

## Points de vigilance à annoncer honnêtement

- Les symbologies complexes et les mises en page ne se convertissent pas parfaitement : prévoir du temps de reprise
- Les attributs spécifiques Esri (domaines, sous-types, classes de relations, topologie de géodatabase) demandent une modélisation équivalente en PostgreSQL (tables de valeurs, contraintes, clés étrangères, triggers)
- Les applications ArcGIS Online n'ont pas d'export vers une solution open source : elles se reconstruisent
- Le coût se déplace des licences vers les compétences et l'hébergement
- Si l'organisation dépend d'un écosystème Esri imposé (partenaires, marchés, outils métier), une stratégie mixte peut être plus réaliste qu'une migration complète
