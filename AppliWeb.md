Créez une appli web :

    -npm init -y
    -npm install express
    -npm i mongoose
    -npm i dotenv
    -npm i nodemon

    Utilisez le pattern MVC pour l'archi de l'appli

    Expliquez le principe de middleware express et utilisez en dans votre appli

    Middleware Express est une infra web middleware + routage qui est une succession d'appels de fonctions middleware
    Les fonctions peuvent acceder à req et res (requete resultat)

    Connectez vous à une BDD MangoDB

    Mettez en place l'auth avec JWT:
        npm i jsonwebtoken
        npm i bcryptjs

    <!> à la façon dont vous installez ces différents packages, certains ne sont utiles que pour le dev d'autres pour la prod