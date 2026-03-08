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
"KOSH": {
    "icao": "KOSH",
    "iata": "OSH",
    "name": "Wittman Regional Airport",
    "city": "Oshkosh",
    "state": "Wisconsin",
    "country": "US",
    "elevation": 808,
    "lat": 43.9844017029,
    "lon": -88.5569992065,
    "tz": "America/Chicago"
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

## 💻 Utilisation

### En PHP

```php
<?php
$airports = json_decode(file_get_contents('airports.json'), true);

// Rechercher un aéroport par code ICAO
$airport = $airports['KOSH'];
echo $airport['name']; // Wittman Regional Airport

// Parcourir tous les aéroports
foreach ($airports as $icao => $data) {
    echo "{$data['name']} ({$icao}) - {$data['city']}, {$data['country']}\n";
}

// Filtrer par pays
$usAirports = array_filter($airports, function($airport) {
    return $airport['country'] === 'US';
});

// Trouver les aéroports dans une zone géographique
function findNearbyAirports($airports, $lat, $lon, $radius = 100) {
    $nearby = [];
    foreach ($airports as $icao => $airport) {
        $distance = calculateDistance($lat, $lon, $airport['lat'], $airport['lon']);
        if ($distance <= $radius) {
            $nearby[$icao] = $airport;
        }
    }
    return $nearby;
}
```

### Cas d'usage pour le tourisme

```php
// Trouver tous les aéroports d'une destination touristique
$franceAirports = array_filter($airports, function($airport) {
    return $airport['country'] === 'FR';
});

// Afficher les options de vol pour une ville
function getAirportsByCity($airports, $city) {
    return array_filter($airports, function($airport) use ($city) {
        return stripos($airport['city'], $city) !== false;
    });
}

$parisAirports = getAirportsByCity($airports, 'Paris');
```

### 📁 Formats disponibles

- **airports.json**: Format JSON complet (~29k entrées)
- **airports.csv**: Format CSV pour import dans des tableurs ou bases de données

## 🌟 Cas d'utilisation

### Pour les portails d'aéroports
- Affichage des informations de vol
- Calcul des distances entre aéroports
- Gestion des fuseaux horaires pour les horaires de vol

### Pour les sites de tourisme
- Recherche d'aéroports près des destinations
- Affichage des options de transport aérien
- Intégration avec des systèmes de réservation

### Pour les applications de voyage
- Planification d'itinéraires
- Calcul des temps de vol
- Suggestions de destinations basées sur la proximité

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

airports, aviation, icao, iata, airport-data, coordinates, timezone, geolocation, aviation-data, json, tourism, travel, portail-aeroport, tourisme, voyage

---

⭐ Si ce projet vous aide, n'hésitez pas à lui donner une étoile sur GitHub !
