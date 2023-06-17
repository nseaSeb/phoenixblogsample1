# Petit exemple avec Phoenix

Vous aurez besoin d'avoir :
- Une installation Erlang / Elixir 1.14 (si vous ne l'avez pas fait recherchez installation avec ASDF)
- Phoenix 1.17
- Docker

### On y va, petit projet simple et très facile à mettre en oeuvre


Pour commencer créons le projet :hammer_and_wrench: 

`mix phx.new blog`

Vous pouvez choisir d'installer les dépendances [Yn]

Pour se rendre dans le dossier du projet

`cd blog`

Si vous n'avez pas installer les dépendances :

`mix.deps.get`

Il vous faudra ajouter une base de donnée Postgres pour cela Docker est notre allié

`docker run --name blog -p 5432:5432 -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -d postgres
`
Une fois le conteneur postgres démarré (`docker ps` pour vérifier)

On va demander à ecto le wrapper de base de données et le générateur de requêtes pour Elixir de créer notre base de donnée.

`mix ecto.create`

Préparons notre schéma, notre controlleur et de quoi construire notre table dans la db

` mix phx.gen.html Blog Post posts title:string content:text`

On lance la tache ecto permettant la migration

`mix ecto.migrate`

Un peu de code enfin, rendons nous dans le dossier lib/blog_web/router.ex
et ajoutons nos routes

```
 scope "/", BlogWeb do
    pipe_through :browser
    get "/", PageController, :home
    get "/posts/", PostController, :index
    get "/posts/new", PostController, :new
    post "/posts", PostController, :create
    get "/posts/:id", PostController, :show
    get "/posts/:id/edit", PostController, :edit
    put "/posts/:id", PostController, :update
    delete "/posts/:id", PostController, :delete
  end
```

Et c'est enfin le moment de démarrer notre serveur
`mix phx.server`

Rendez-vous en http://localhost:4000/posts
<img width="1417" alt="Capture d’écran 2023-06-17 à 21 35 12" src="https://github.com/nseaSeb/phoenixblogsample1/assets/43780939/3fe34884-ba21-436b-b512-deb82f0b7950">



