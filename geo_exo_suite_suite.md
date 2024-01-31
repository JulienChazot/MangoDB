Télécharger les jeux d'essais suivants :
https://raw.githubusercontent.com/mongodb/docs-assets/geospatial/restaurants.json
https://raw.githubusercontent.com/mongodb/docs-assets/geospatial/neighborhoods.json

Creation d'un index 2dsphere
Un index géospatial, et améliore presque toujours les performances des requêtes $geoWithin et $geoIntersects. Comme ces données sont géographiques, créez un index2dsphère sur chaque collection en utilisant le shell mongo :

Attention, la création d'un index est OBLIGATOIRE pour permettre l'utilisation des arguments :$geoIntersects, $geoSphere, $geoNear, $geoWithin, $centerSphere, $nearSphere , etc...

db.restaurants.createIndex({ "address.coord": "2dsphere" });


Explorez les données, documentez votre démarche et vos résultats dans un fichier geo_exo_suite_suite.md

Pour explorer les données, on peut simplement utiliser :

var requete = {
    "borough": "Manhattan"
};

db.restaurants.find(requete);

Ou utiliser les requêtes geospaciales :

var point = { type: "Point", coordinates: [long, lat] };
var rayon = 1 / 6371; 

var requete = {
    "address.coord": {
        $nearSphere: {
            $geometry: point,
            $maxDistance: rayon
        }
    }
};

db.restaurants.find(requete);

On prends un point avec la long et la lat, on définit le rayon par 1/6371 qui est le rayon de la terre en km
On trace un cercle autour de notre point d'un km 
On affiche le resultat


Trouvez la commande qui va retourner le restaurant Riviera Caterer... De quel type d'ojet GeoJSON s'agit-il ?

db.restaurants.findOne({ "name": "Riviera Caterer" });

C'est un type Point, car il fournit des coordonnées sous forme de long lat, parce qu'il est relié à address

Trouvez "Hell's kitchen" au sein de la collection "neighborhoods" et retournez le nom du quartier, sa superficie et sa population. Quelle est la superficie totale de ce quartier ?

db.neighborhoods.findOne({ "name": "Hell's Kitchen" });

Trouvez la requete type qui permet de recuperer le nom du quartier a partir d'un point donné.

Trouver la requete qui trouve les restaurants dans un rayon donné (8km par exemple)