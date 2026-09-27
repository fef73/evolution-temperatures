# Évolution des températures — Historique météo d'une ville

Application météo mono-fichier (HTML/CSS/JS, sans backend) qui retrace l'évolution des températures d'une ville, jour après jour, sur la période de ton choix, à partir des archives climatiques [Open-Meteo](https://open-meteo.com/) (ERA5, gratuites, sans clé API).

Site : https://fef73.github.io/evolution-temperatures/ — accessible aussi depuis le lanceur [comparateur-meteo.fr](https://comparateur-meteo.fr/).

## Ville

- Recherche de n'importe quelle ville au monde (géocodage Open-Meteo).
- Altitude précise optionnelle (bouton **⛰ Altitude**) : station de ski, sommet, vallée encaissée — la température est recalculée pour ce point.
- **Population** affichée sous le nom de la ville :
  - en France : population de la **commune** et de la **zone urbaine** (intercommunalité / EPCI), données INSEE via `geo.api.gouv.fr` ;
  - hors France : population de la ville (Open-Meteo / GeoNames) ;
  - chiffres exacts et sources au survol.

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
- Nombre de jours de canicule (≥35 °C), gel (≤0 °C), pluie (≥1 mm), neige, vent fort (rafales ≥60 km/h), qualité de l'air dégradée (AQI >40) et beau temps.
- Bulletin résumant la période, avec alertes canicule, gel, pollution et vent.

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
- Le résumé des fonctionnalités est aussi affiché dans le site, dans un panneau repliable juste avant le pied de page.

## Sources de données

- Archives climatiques journalières et horaires : `archive-api.open-meteo.com` (ERA5)
- Qualité de l'air : `air-quality-api.open-meteo.com`
- Géocodage : `geocoding-api.open-meteo.com`
- Population (France) : `geo.api.gouv.fr` (INSEE)

## Notes techniques

- Fichier unique, aucune dépendance serveur — Chart.js chargé depuis un CDN pour le graphique.
- Statistiques de visite anonymes et sans cookie avec GoatCounter.
