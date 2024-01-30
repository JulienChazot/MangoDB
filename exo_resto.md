récupérez la liste des restaurants ayant un grade inférieur à un score de 10 (Afficher cette liste sous forme de projection {name, grades.scores}) 
db.restaurants.find(
    {"grades.score": { $lt: 10 }},
    { "name": 1, "grades.score": 1, "_id": 0 }
)

récupérez la liste des restaurants ayant uniquement des grades inférieur à 10 
db.restaurants.find(
    { "grades": { $not: { $elemMatch: { "score": { $gte: 10 } } } } }
)