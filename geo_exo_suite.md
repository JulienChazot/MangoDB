Soit un objet GeoJSON de la forme suivante :

```js
var polygone = {
  type: "Polygon",
  coordinates: [
    [
      [43.94899, 4.80908],
      [43.95292, 4.80929],
      [43.95174, 4.8056],
      [43.94899, 4.80908],
    ],
  ],
};
```

Donnez le nom des salles qui résident à l’intérieur.


var polygone = {
  type: "Polygon",
  coordinates: [
    [
      [43.94899, 4.80908],
      [43.95292, 4.80929],
      [43.95174, 4.8056],
      [43.94899, 4.80908],
    ],
  ],
};

var requete = {
  "adresse.localisation": {
    $geoWithin: {
      $geometry: polygone
    }
  }
};

var resultats = db.salles.find(requete);

resultats.forEach(function(salle) {
  print(salle.nom);
});