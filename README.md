# Airports ✈️

> Fait avec ❤️ en 2026 pour aider les développeurs de portails d'aéroports, de tourisme et bien plus encore !

Une collection JSON de ~29 000 entrées avec des informations de base sur presque tous les aéroports et pistes d'atterrissage du monde.

## 🎯 Pourquoi ce projet ?

Ce projet a été créé avec passion pour faciliter le travail des développeurs qui construisent :

- 🛫 **Portails d'aéroports** : Intégrez facilement des données d'aéroports dans vos applications
- 🌍 **Sites de tourisme** : Enrichissez vos plateformes avec des informations géographiques précises
- 🗺️ **Applications de voyage** : Offrez à vos utilisateurs des données complètes sur les destinations
- 📊 **Outils d'analyse** : Exploitez des données structurées pour vos analyses
- 🚁 **Applications d'aviation** : Accédez rapidement aux informations essentielles

## 📋 Description

Cette bibliothèque fournit des données complètes sur les aéroports du monde entier, utilisant les codes ICAO comme clés. Chaque entrée contient des informations essentielles telles que le code IATA, le nom de l'aéroport, la ville, le code pays ISO à deux lettres, l'altitude en pieds, les coordonnées géographiques et le fuseau horaire.

## 📦 Format des données

Les données sont structurées au format JSON avec les codes ICAO comme identifiants uniques :

```json
"HBBA": {
    "icao": "HBBA",
    "iata": "BJM",
    "name": "Bujumbura International Airport",
    "city": "Bujumbura",
    "country": "BI",
    "elevation": 2582,
    "lat": -3.3240199089,
    "lon": 29.3185005188,
    "tz": "Africa/Bujumbura"
}
```

## 🔑 Champs disponibles

- **icao**: Code ICAO de l'aéroport (4 lettres)
- **iata**: Code IATA de l'aéroport (3 lettres)
- **name**: Nom complet de l'aéroport
- **city**: Ville où se situe l'aéroport
- **state**: État/Province (si applicable)
- **country**: Code pays ISO à deux lettres
- **elevation**: Altitude au-dessus du niveau de la mer (en pieds)
- **lat**: Latitude en degrés décimaux
- **lon**: Longitude en degrés décimaux
- **tz**: Fuseau horaire (format IANA)

## 🚀 Installation

Via Composer :

```bash
composer require irakozemasenge/airports
```

Via npm :

```bash
npm install @irakozemasenge/airports
```

Via pip (Python) :

```bash
pip install airports-data
```

## 💻 Utilisation

### En JavaScript / Node.js

```javascript
// Charger les données
const airports = require('./airports.json');

// Rechercher un aéroport par code ICAO
const bujumburaAirport = airports['HBBA'];
console.log(bujumburaAirport.name); // Bujumbura International Airport
console.log(`${bujumburaAirport.city}, ${bujumburaAirport.country}`); // Bujumbura, BI

// Parcourir tous les aéroports
Object.entries(airports).forEach(([icao, data]) => {
    console.log(`${data.name} (${icao}) - ${data.city}, ${data.country}`);
});

// Filtrer par pays (exemple: Burundi)
const burundiAirports = Object.entries(airports)
    .filter(([icao, airport]) => airport.country === 'BI')
    .reduce((obj, [icao, airport]) => {
        obj[icao] = airport;
        return obj;
    }, {});

console.log('Aéroports au Burundi:', burundiAirports);

// Trouver les aéroports par ville
function getAirportsByCity(airports, city) {
    return Object.entries(airports)
        .filter(([icao, airport]) => 
            airport.city.toLowerCase().includes(city.toLowerCase())
        )
        .reduce((obj, [icao, airport]) => {
            obj[icao] = airport;
            return obj;
        }, {});
}

const bujumburaAirports = getAirportsByCity(airports, 'Bujumbura');
console.log(bujumburaAirports);
```

### En JavaScript (Frontend / Browser)

```javascript
// Charger les données via fetch
fetch('airports.json')
    .then(response => response.json())
    .then(airports => {
        // Afficher l'aéroport de Bujumbura
        const bujumbura = airports['HBBA'];
        document.getElementById('airport-info').innerHTML = `
            <h2>${bujumbura.name}</h2>
            <p>Ville: ${bujumbura.city}</p>
            <p>Code IATA: ${bujumbura.iata}</p>
            <p>Altitude: ${bujumbura.elevation} pieds</p>
            <p>Coordonnées: ${bujumbura.lat}, ${bujumbura.lon}</p>
        `;
    });

// Calculer la distance entre deux aéroports
function calculateDistance(lat1, lon1, lat2, lon2) {
    const R = 6371; // Rayon de la Terre en km
    const dLat = (lat2 - lat1) * Math.PI / 180;
    const dLon = (lon2 - lon1) * Math.PI / 180;
    const a = Math.sin(dLat/2) * Math.sin(dLat/2) +
              Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) *
              Math.sin(dLon/2) * Math.sin(dLon/2);
    const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
    return R * c;
}

// Trouver les aéroports proches de Bujumbura
function findNearbyAirports(airports, targetIcao, radiusKm = 500) {
    const target = airports[targetIcao];
    const nearby = [];
    
    Object.entries(airports).forEach(([icao, airport]) => {
        if (icao !== targetIcao) {
            const distance = calculateDistance(
                target.lat, target.lon,
                airport.lat, airport.lon
            );
            if (distance <= radiusKm) {
                nearby.push({
                    ...airport,
                    icao: icao,
                    distance: Math.round(distance)
                });
            }
        }
    });
    
    return nearby.sort((a, b) => a.distance - b.distance);
}

// Exemple: Aéroports dans un rayon de 500km autour de Bujumbura
const nearbyAirports = findNearbyAirports(airports, 'HBBA', 500);
console.log('Aéroports proches de Bujumbura:', nearbyAirports);
```

### En Python

```python
import json
import math

# Charger les données
with open('airports.json', 'r', encoding='utf-8') as f:
    airports = json.load(f)

# Rechercher l'aéroport de Bujumbura
bujumbura = airports['HBBA']
print(bujumbura['name'])  # Bujumbura International Airport
print(f"{bujumbura['city']}, {bujumbura['country']}")  # Bujumbura, BI

# Filtrer par pays (Burundi)
burundi_airports = {
    icao: airport for icao, airport in airports.items() 
    if airport['country'] == 'BI'
}

print(f"Aéroports au Burundi: {len(burundi_airports)}")
for icao, airport in burundi_airports.items():
    print(f"{airport['name']} ({icao}) - {airport['city']}")

# Trouver les aéroports par ville
def get_airports_by_city(airports, city):
    return {
        icao: airport for icao, airport in airports.items()
        if city.lower() in airport['city'].lower()
    }

bujumbura_airports = get_airports_by_city(airports, 'Bujumbura')

# Calculer la distance entre deux aéroports (formule de Haversine)
def calculate_distance(lat1, lon1, lat2, lon2):
    R = 6371  # Rayon de la Terre en km
    
    lat1_rad = math.radians(lat1)
    lat2_rad = math.radians(lat2)
    delta_lat = math.radians(lat2 - lat1)
    delta_lon = math.radians(lon2 - lon1)
    
    a = (math.sin(delta_lat / 2) ** 2 + 
         math.cos(lat1_rad) * math.cos(lat2_rad) * 
         math.sin(delta_lon / 2) ** 2)
    c = 2 * math.atan2(math.sqrt(a), math.sqrt(1 - a))
    
    return R * c

# Trouver les aéroports proches de Bujumbura
def find_nearby_airports(airports, target_icao, radius_km=500):
    target = airports[target_icao]
    nearby = []
    
    for icao, airport in airports.items():
        if icao != target_icao:
            distance = calculate_distance(
                target['lat'], target['lon'],
                airport['lat'], airport['lon']
            )
            if distance <= radius_km:
                nearby.append({
                    **airport,
                    'icao': icao,
                    'distance': round(distance, 2)
                })
    
    return sorted(nearby, key=lambda x: x['distance'])

# Exemple: Aéroports dans un rayon de 500km autour de Bujumbura
nearby_airports = find_nearby_airports(airports, 'HBBA', 500)
print(f"\nAéroports proches de Bujumbura (rayon 500km):")
for airport in nearby_airports[:5]:  # Afficher les 5 plus proches
    print(f"{airport['name']} - {airport['distance']}km")
```

### Utilisation avec Pandas (Python)

```python
import pandas as pd
import json

# Charger les données dans un DataFrame
with open('airports.json', 'r', encoding='utf-8') as f:
    airports = json.load(f)

df = pd.DataFrame.from_dict(airports, orient='index')

# Statistiques par pays
print(df['country'].value_counts().head(10))

# Filtrer les aéroports du Burundi
burundi_df = df[df['country'] == 'BI']
print(burundi_df[['name', 'city', 'elevation']])

# Trouver les aéroports les plus hauts en altitude
highest_airports = df.nlargest(10, 'elevation')[['name', 'city', 'country', 'elevation']]
print(highest_airports)

# Recherche par ville
bujumbura_df = df[df['city'].str.contains('Bujumbura', case=False, na=False)]
print(bujumbura_df)

# Exporter vers CSV
df.to_csv('airports_processed.csv', index=True, encoding='utf-8')
```

### En PHP

```php
<?php
$airports = json_decode(file_get_contents('airports.json'), true);

// Rechercher l'aéroport de Bujumbura
$bujumbura = $airports['HBBA'];
echo $bujumbura['name']; // Bujumbura International Airport

// Filtrer par pays (Burundi)
$burundiAirports = array_filter($airports, function($airport) {
    return $airport['country'] === 'BI';
});

// Afficher les options de vol pour une ville
function getAirportsByCity($airports, $city) {
    return array_filter($airports, function($airport) use ($city) {
        return stripos($airport['city'], $city) !== false;
    });
}

$bujumburaAirports = getAirportsByCity($airports, 'Bujumbura');
```

### 📁 Formats disponibles

- **airports.json**: Format JSON complet (~29k entrées)
- **airports.csv**: Format CSV pour import dans des tableurs ou bases de données

## 🌟 Cas d'utilisation

### Pour les portails d'aéroports
```javascript
// Afficher les informations de vol en temps réel
const flightInfo = {
    departure: airports['HBBA'], // Bujumbura
    arrival: airports['HKJK'],   // Nairobi
    distance: calculateDistance(
        airports['HBBA'].lat, airports['HBBA'].lon,
        airports['HKJK'].lat, airports['HKJK'].lon
    )
};
```

### Pour les sites de tourisme
```javascript
// Recherche d'aéroports près d'une destination touristique
const touristDestination = { lat: -3.3731, lon: 29.3600 }; // Bujumbura
const nearestAirports = findNearbyAirports(airports, 'HBBA', 100);

// Afficher les options de transport
nearestAirports.forEach(airport => {
    console.log(`${airport.name} - ${airport.distance}km de votre destination`);
});
```

### Pour les applications de voyage
```javascript
// Planification d'itinéraires multi-destinations
const itinerary = [
    airports['HBBA'], // Bujumbura, Burundi
    airports['HKJK'], // Nairobi, Kenya
    airports['FACT']  // Cape Town, South Africa
];

let totalDistance = 0;
for (let i = 0; i < itinerary.length - 1; i++) {
    const distance = calculateDistance(
        itinerary[i].lat, itinerary[i].lon,
        itinerary[i+1].lat, itinerary[i+1].lon
    );
    totalDistance += distance;
    console.log(`${itinerary[i].city} → ${itinerary[i+1].city}: ${Math.round(distance)}km`);
}
console.log(`Distance totale: ${Math.round(totalDistance)}km`);
```

### Analyse de données avec Python
```python
import json
import pandas as pd
import matplotlib.pyplot as plt

# Charger et analyser les données
with open('airports.json', 'r') as f:
    airports = json.load(f)

df = pd.DataFrame.from_dict(airports, orient='index')

# Analyser la distribution des aéroports par continent
country_counts = df['country'].value_counts().head(10)

# Visualiser les données
plt.figure(figsize=(12, 6))
country_counts.plot(kind='bar')
plt.title('Top 10 des pays avec le plus d\'aéroports')
plt.xlabel('Pays')
plt.ylabel('Nombre d\'aéroports')
plt.tight_layout()
plt.savefig('airports_by_country.png')

# Statistiques sur le Burundi
burundi_airports = df[df['country'] == 'BI']
print(f"Nombre d'aéroports au Burundi: {len(burundi_airports)}")
print(f"Altitude moyenne: {burundi_airports['elevation'].mean():.0f} pieds")
```

## 📚 Sources des données

- Données des aéroports : Diverses sources publiques d'aviation
- Fuseaux horaires : Initialement sourcés de [TimeZoneDB](https://timezonedb.com) et mis à jour via [TimeAPI](https://www.timeapi.io/)

## 🤝 Contribution

Les contributions sont les bienvenues avec grand plaisir ! N'hésitez pas à :

1. Fork le projet
2. Créer une branche pour votre fonctionnalité (`git checkout -b feature/amelioration`)
3. Commit vos changements (`git commit -am 'Ajout d'une fonctionnalité'`)
4. Push vers la branche (`git push origin feature/amelioration`)
5. Créer une Pull Request

Toute amélioration, correction ou suggestion est appréciée ! 💪

## 📄 Licence

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.

## 👨‍💻 Auteur

**Ir Masenge**

Créé avec ❤️ pour la communauté des développeurs

- GitHub: [@Irakozemasenge](https://github.com/Irakozemasenge)
- Email: [irmasenge@gmail.com](mailto:irmasenge@gmail.com)
- Téléphone: +257 69 65 16 69
- Repository: [https://github.com/Irakozemasenge/Airports](https://github.com/Irakozemasenge/Airports)

## 💬 Support

Besoin d'aide ? N'hésitez pas à :

- Ouvrir une issue : [https://github.com/Irakozemasenge/Airports/issues](https://github.com/Irakozemasenge/Airports/issues)
- Consulter le code source : [https://github.com/Irakozemasenge/Airports](https://github.com/Irakozemasenge/Airports)

## 🏷️ Mots-clés

airports, aviation, icao, iata, airport-data, coordinates, timezone, geolocation, aviation-data, json, tourism, travel, portail-aeroport, tourisme, voyage, bujumbura, burundi, python, javascript, php

---

⭐ Si ce projet vous aide, n'hésitez pas à lui donner une étoile sur GitHub !
