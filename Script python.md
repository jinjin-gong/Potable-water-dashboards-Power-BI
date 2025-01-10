## 1. Nettoyage des données et calcul de nouveaux indicateurs

```
import pandas as pd

# Charger les données
water_data = pd.read_csv('data/raw/water_access_data.csv')

# Nettoyage des données
water_data.dropna(subset=['safe_water_population', 'total_population'], inplace=True)

# Calcul du taux d'accès à l'eau potable
water_data['safe_water_access_rate'] = (water_data['safe_water_population'] / 
                                        water_data['total_population']) * 100

# Sauvegarde des données nettoyées
water_data.to_csv('data/processed/water_access_cleaned.csv', index=False)
```

## 2. Visualisation des taux d'accès à l'eau potable

```
import matplotlib.pyplot as plt

# Charger les données nettoyées
data = pd.read_csv('data/processed/water_access_cleaned.csv')

# Calcul du taux moyen d'accès par région
region_data = data.groupby('region')['safe_water_access_rate'].mean()

# Visualisation
region_data.sort_values().plot(kind='bar', figsize=(10, 6))
plt.title('Taux moyen d\'accès à l\'eau potable par région')
plt.ylabel('Taux (%)')
plt.xlabel('Région')
plt.tight_layout()
plt.savefig('visualizations/safe_water_access_rate_by_region.png')
plt.show()
```

## 3. Analyse de la mortalité liée à l'eau insalubre

```
# Charger les données
mortality_data = pd.read_csv('data/raw/water_health_data.csv')

# Calcul de la mortalité totale par pays
mortality_by_country = mortality_data.groupby('country')['water_related_deaths'].sum()

# Top 10 des pays
top_10_countries = mortality_by_country.sort_values(ascending=False).head(10)

# Visualisation
top_10_countries.plot(kind='bar', figsize=(10, 6))
plt.title('Top 10 des pays par mortalité liée à l\'eau insalubre')
plt.ylabel('Nombre de morts')
plt.xlabel('Pays')
plt.tight_layout()
plt.savefig('visualizations/water_related_mortality_top10.png')
plt.show()
```

## 4. Carte mondiale des infrastructures "safely managed"

```
import geopandas as gpd
import pandas as pd
import folium

# Charger les données des infrastructures
infra_data = pd.read_csv('data/processed/infrastructure_data.csv')

# Charger les données géographiques des pays
world = gpd.read_file(gpd.datasets.get_path('naturalearth_lowres'))

# Fusionner les données
merged = world.merge(infra_data, left_on='name', right_on='country')

# Carte interactive
map = folium.Map(location=[0, 0], zoom_start=2)
for _, row in merged.iterrows():
    folium.CircleMarker(
        location=[row['latitude'], row['longitude']],
        radius=row['safely_managed_infrastructure_rate'] / 10,
        color='blue',
        fill=True,
        fill_opacity=0.6,
        popup=f"{row['name']}: {row['safely_managed_infrastructure_rate']}%"
    ).add_to(map)

# Sauvegarder la carte
map.save('visualizations/infrastructure_map.html')
```
