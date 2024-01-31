Télécharger les jeux d'essais suivants :
https://raw.githubusercontent.com/mongodb/docs-assets/geospatial/restaurants.json
https://raw.githubusercontent.com/mongodb/docs-assets/geospatial/neighborhoods.json

Creation d'un index 2dsphere
Un index géospatial, et améliore presque toujours les performances des requêtes $geoWithin et $geoIntersects. Comme ces données sont géographiques, créez un index2dsphère sur chaque collection en utilisant le shell mongo :

Attention, la création d'un index est OBLIGATOIRE pour permettre l'utilisation des arguments :$geoIntersects, $geoSphere, $geoNear, $geoWithin, $centerSphere, $nearSphere , etc...

db.restaurants.createIndex({ "address.coord": "2dsphere" });


Explorez les données, documentez votre démarche et vos résultats dans un fichier geo_exo_suite_suite.md

Pour explorer les données, on peut simplement utiliser (pour une donnée spécifique, sinon on a toujours le db.collection.find({})limit.(*)) :

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

Les données sont :

-Un Id de type ObjectId
-Dans geometry, on a toutes les coordonnées
-On a le type (Polygone ...)
-Un nom

Trouvez la commande qui va retourner le restaurant Riviera Caterer... De quel type d'ojet GeoJSON s'agit-il ?

db.restaurants.findOne({ "name": "Riviera Caterer" });

C'est un type Point, car il fournit des coordonnées sous forme de long lat, parce qu'il est relié à address

Trouvez "Hell's kitchen" au sein de la collection "neighborhoods" et retournez le nom du quartier, sa superficie et sa population. Quelle est la superficie totale de ce quartier ?

db.neighborhoods.findOne({ "name": "Hell'S Kitchen" }); -> Renvoi null ??

db.neighborhoods.createIndex({ geometry: "2dsphere" })



Trouvez la requete type qui permet de recuperer le nom du quartier a partir d'un point donné.

var point = { type: "Point", coordinates: [longitude, latitude] };

db.neighborhoods.findOne({
    geometry: {
        $geoIntersects: {
            $geometry: point
        }
    }
}, { name: 1, _id: 0 });

Trouver la requete qui trouve les restaurants dans un rayon donné (8km par exemple)

var point = { type: "Point", coordinates: [longitude, latitude] };
var rayon = 8 / 6371; 
db.restaurants.find({
    "address.coord": {
        $nearSphere: {
            $geometry: point,
            $maxDistance: rayon
        }
    }
});