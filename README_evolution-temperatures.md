# Historique météo Ville — évolution des températures d'une ville

Application météo mono-fichier (HTML/CSS/JS, sans backend) qui retrace l'évolution des températures d'une ville, jour après jour, sur la période de ton choix, à partir des archives climatiques [Open-Meteo](https://open-meteo.com/) (ERA5, gratuites, sans clé API).

Site : https://fef73.github.io/evolution-temperatures/ — accessible aussi depuis le lanceur [comparateur-meteo.fr](https://comparateur-meteo.fr/).

## Ville

- Recherche de n'importe quelle ville au monde (géocodage Open-Meteo).
- Altitude précise optionnelle (bouton **⛰ Altitude**) : station de ski, sommet, vallée encaissée — la température est recalculée pour ce point.
- **Population** affichée sous le nom de la ville :
  - en France : population de la **commune** et de la **zone urbaine** (intercommunalité / EPCI), données INSEE via `geo.api.gouv.fr` ;
  - hors France : population de la ville (Open-Meteo / GeoNames) ;
  - chiffres exacts et sources au survol (ou au toucher sur mobile).

## Période

- 7 jours, 30 jours, 3 mois, 1 an, ou dates personnalisées depuis 1940 (début des archives ERA5).
- Les derniers jours sont consolidés avec environ 5 jours de délai.

## Graphique

- Températures maximale, moyenne et minimale, jour après jour (la moyenne est celle des 24 relevés horaires, pas (max + min) / 2).
- Lignes de seuil canicule (35 °C) et gel (0 °C).
- Pression atmosphérique et qualité de l'air (AQI européen) avec bandes de couleur.
- Isotherme 0 °C estimée, masquée par défaut (à activer dans la légende).
- Clic sur la légende pour masquer/afficher une courbe.
- Tooltip au survol avec icône météo et phase de lune.
- Jauge d'amplitude thermique : minimum, moyenne et maximum de la période.

## Cartes et bulletin

- Journée la plus chaude et la plus froide, avec heure du pic, humidité et vent.
- Moyenne et écart thermique sur la période.
- Nombre de jours de canicule (≥35 °C), gel (≤0 °C), pluie (≥1 mm, hors jours de neige), vent fort (rafales ≥60 km/h), qualité de l'air dégradée (AQI >40) et beau temps.
- **Jours de neige** : cumul de neige quotidien des archives Open-Meteo (`snowfall_sum`, ERA5), un jour comptant dès 0,5 cm ; le total de neige tombée sur la période est affiché en cm.
  - Si une altitude est précisée, la neige des archives reste celle de la maille du modèle : un jour compte donc aussi comme neigeux s'il a précipité (≥1 mm) alors que la température moyenne corrigée pour l'altitude était ≤1,3 °C (point situé au-dessus de l'isotherme 0 °C, ou moins de 200 m en dessous).
- Bulletin résumant la période, avec alertes canicule, gel, pollution, vent et neige.

## Confort d'usage

- Interface bilingue FR/EN et unité °C/°F (préférences mémorisées) — graphique, cartes, jauge et bulletin sont convertis.
- Lien de partage qui conserve la ville et la période.
- Accès direct par URL, pour un raccourci ou une appli mobile :
  ```
  ?lat=45.9237&lon=6.8694&ville=Chamonix&alt=1035&periode=30
  ```
  `periode` : `7`, `30`, `90` ou `365` — ou une période précise :
  ```
  ?lat=45.9237&lon=6.8694&ville=Chamonix&debut=2024-01-01&fin=2024-01-31
  ```
- Position GPS sans nom (appli mobile, raccourci) : la **commune, la région et le pays** sont retrouvés automatiquement par géocodage inverse (Nominatim / OpenStreetMap). Si le service ne répond pas en 4 secondes, « Position GPS » est affiché.
- Depuis le lanceur [comparateur-meteo.fr](https://comparateur-meteo.fr/), le bouton **📍 Ma position** transmet la position du téléphone et le nom choisi.
- Le résumé des fonctionnalités est aussi affiché dans le site, dans un panneau repliable juste avant le pied de page.

## Cache local et hors connexion

- **Cache des archives** : chaque jour consulté est gardé dans le téléphone (IndexedDB). Les périodes déjà vues s'affichent aussitôt, et seuls les jours manquants sont demandés à Open-Meteo.
  - Un enregistrement par lieu (coordonnées) et par altitude.
  - Les 8 derniers jours ne sont jamais mis en cache, car ERA5 les consolide encore.
  - Le nombre de jours lus dans le cache est indiqué à côté de l'heure de mise à jour (« 💾 27 j en cache »).
- **⬇ Enregistrer / ⬆ Restaurer** (bas de page) : sauvegarde du cache dans un fichier `meteo-cache-AAAA-MM-JJ.json`, puis restauration sur le même appareil ou un autre. Un seul fichier couvre l'historique ville et l'historique neige, qui partagent la même base.
- **Vider le cache** : n'efface que les données de ce site.
- **Hors connexion** : après une première visite, la page s'ouvre sans réseau (service worker `sw.js`). Les jours en cache s'affichent, avec « 📴 hors connexion — N jour(s) récent(s) indisponible(s) ».

## Sources de données

- Archives climatiques journalières et horaires (températures, précipitations, cumul de neige, vent, pression) : `archive-api.open-meteo.com` (ERA5)
- Qualité de l'air : `air-quality-api.open-meteo.com`
- Géocodage : `geocoding-api.open-meteo.com`
- Population (France) : `geo.api.gouv.fr` (INSEE)
- Géocodage inverse (position GPS → commune) : `nominatim.openstreetmap.org`

## Notes techniques

- Fichier unique, aucune dépendance serveur — Chart.js chargé depuis un CDN pour le graphique.
- `sw.js` : service worker (page, Chart.js et polices gardés sur l'appareil pour l'usage hors connexion).
- Statistiques de visite anonymes et sans cookie avec GoatCounter.

## Licence

© 2026 Fernand (fef73) — tous droits réservés. Voir le fichier [LICENSE](LICENSE). Les données météo restent soumises aux licences de leurs fournisseurs (Open-Meteo CC BY 4.0, INSEE / Etalab, OpenStreetMap ODbL).
