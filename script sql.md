## Script SQL pour calculer le taux d'accès à l'eau potable

### Objectif
Ce script calcule le taux d'accès à l'eau potable par pays et par année à partir des données disponibles dans la table `BasicAndSafelyManagedDrinkingWaterServices`.

### Description des colonnes :
- `Population using at least basic drinking-water services` : Nombre de personnes ayant accès à de l'eau potable.
- `total_population` : Population totale d'un pays pour une année donnée.

### Code SQL :
```sql
SELECT
    country,
    year,
    SUM(Population using at least basic drinking-water services) / SUM(total_population) * 100 AS Population using at least basic drinking-water services_rate
FROM BasicAndSafelyManagedDrinkingWaterServices
GROUP BY country, year
ORDER BY Population using at least basic drinking-water services_rate ASC;

```
## Extraction des taux d'accès à l'eau potable par région
```
SELECT 
    region,
    year,
    SUM(Population using safely drinking-water) / SUM(total_population) * 100 AS safe_water_access_rate
FROM BasicAndSafelyManagedDrinkingWaterServices
GROUP BY region, year
ORDER BY year, safe_water_access_rate DESC;
```

## Pays avec la plus forte mortalité liée à l'eau insalubre
```
SELECT 
    country,
    year,
    SUM(WASH deaths) AS total_deaths
FROM Mortability rate attributed to exposure to unsafe WAH services
GROUP BY country, year
ORDER BY total_deaths DESC
LIMIT 10;
```

