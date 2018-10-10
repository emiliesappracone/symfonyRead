## Create symfony project

### Start :
`composer create-project symfony/skeleton MyProject`

### Bundles :
`composer require twig`
`composer require annotations`
`composer require profiler --dev`
`composer require maker --dev`
`composer require doctrine`

### Env :
- change database connexion

### Create Database
`php bin/console doctrine:database:create`

### Build entities and controllers
- Make Controller :

`php bin/console make:controller EntityController`

- Make Entity :

`php bin/console make:entity` 

- Choose name of Entity (Table mysql)
- Choose all property (Column mysql)
- Edit migration file

`php bin/console make:migration`

- Persist new Entity in Database

`php bin/console doctrine:migrations:migrate`

### Build entities with association

- Make the controllers like previously

- Make 2 entities`: **Categories** & **Articles**

`php bin/console make:entity`

- Choose which property will be associated, edit one entity with : 

`php bin/console make:entity` 

> entity => **Articles**

> *name : category*

> *type : relation*

> *relation type : choose between them*

- Edit migration file like previously

- Persist Entities in Database like previously 

### Routes

In this structure, routes are defined in each method of controllers, due to annotations bundle.

    Class ArticlesController {
        /**
        * @Route("/articles", name="showAllArticles")
        */
        public function showAllArticles(){}
    }

- Make a redirection to this route with the name

- Can add slug to pass param in path, via : `@Route("/article/{id}", name="showOneArticle")`
- Can add requirements to slug, via : `@Route("/article/{id}", name="showOneArticle", requirements={"id"="\d+"})` 
    - if you have many params : `requirements={"param1" : "\d+", "param2" : "\d+"}`
- List all routes : `php bin/console debug:router`
- Use slug(s) value in param(s) method :


        Class ArticlesController {
            /**
            * @Route("/article/{lang}/{id}", name="showOneArticle")
            */
            public function showAllArticles($lang, $id){}
        }

### Twig

- available soon 

### Multilingue

Structure of column multilingue is in **JSON**, like below. 

    {
        "fr" : "mon titre français",
        "en" : "my english title"
    }
    
In controllers set slug {lang} parameter : `@Route("/{`**lang**`}/article/{id}", name="showOneArticle")`

In route's method use the value of slug lang : `public function showOneArticle(`**$lang**`, $id){}`

Make a query - available soon

### Assets

Download bundle asset :

`composer require asset`

- copy paste public/ directory css/assets
- copy paste link css in base.html.twig
- {{ asset("assets/link/...") }}
