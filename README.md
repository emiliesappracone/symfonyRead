## My best practices for symfony project

<a href="https://github.com/emiliesappracone/symfony_read#start-">Start</a>

```diff
+ Sorry for my english
```

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

- Make a redirection to this route with the name : `return $this->redirectToRoute("showOneArticle", ["id" => $id]);`

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

Twig is a template engine for PHP. You can use Smarty too, but I choose Twig.
Twig have his own syntax, like below :

- echo variable : `{{ myVariable }}`
- call function : `{% for item in array %}` *to do* `{% endfor %}`

List of functions used :

* for : `{% for item in array %}` *to do* `{% endfor %}`
* if : `{% if %}` *to do* `{% endif %}`
* if else : `{% if %}` *to do* `{% else %}` *to do* `{% endif %}`
* elseif : `{% if %}` *to do* `{% elseif %}` *to do* `{% else %}` *to do* `{% endif %}`
* set variable : `{% set myVar = '1' %}`
* extends html file : `{% extends 'path' %}`
* substring : `{{ myVar|slice(start, end) }}`
* length : `{{ myVar|length }}`
* concat in echo : `{{ 'content_' ~ myVar }}`
* twig extension path : `{{ path("routeName" , {"param":myParamVar}) }}`
* if ternary : `{{ myVar|length > 1 ? 'myVar equals 1' : 'myVar doesn't equal 1' }}`
* echo children 1 : `{{ myObject.hisChild }}`
* echo children 2 : `{{ myObject['hisChild'] }}`
* date format : `{{ myDate|date('d-m-Y') }}` 
* filtre uppercase : `{% filter upper %}` *this title is in upper* `{% endfilter %}`

For more go : https://twig.symfony.com/doc/2.x/templates.html

##### My Twig structure for symfony :

Templates/ 
- Admin/
    - admin.html.twig
    - moduleTemplate/
        - twigs of template
- Front/ 
    - front.html.twig
    - moduleTemplate/
        - twigs of template
- base.html.twig

```diff
- N.B : When you'll create entity, with cli, templates/ will be updated with new directory entity and file.
```

1- Example of my **simple** *base.html.twig* :

    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="UTF-8">
            <title>{% block title %}Welcome!{% endblock %}</title>
            <meta http-equiv="X-UA-Compatible" content="IE=edge">
            <meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1, maximum-scale=1">
    
            <!-- CDN BOOTSTRAP -->
            <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
            <!-- CDN GOOGLE FONT -->
            <link href="http://fonts.googleapis.com/css?family=Nunito:300,400,700" rel="stylesheet" type="text/css">
            <!-- LINKS SUPERLIST CSS -->
            <link href="assets/libraries/font-awesome/css/font-awesome.min.css" rel="stylesheet" type="text/css">
            <link href="assets/css/superlist.css" rel="stylesheet" type="text/css" >
            {% block stylesheets %}{% endblock %}
        </head>
        <body>
            {% block body %}
                {% block header %}
                {% endblock %}
                {% block container %}
                {% endblock %}
                {% block footer %}
                {% endblock %}
            {% endblock %}
            <script src=""></script>
            {% block javascripts %}{% endblock %}
        </body>
    </html>

```diff
! here you can call all js and css will be called for front and admin part 
```

2- Example of my **simple** *admin.html.twig* :

    {% extends 'base.html.twig' %}
         {% block header %}
            <nav>
                <ul>
                    <li>Menu 1</li>
                </ul>
            </nav>
         {% endblock %}
         {% block container %}
            - this will be overloaded in twigs child
         {% endblock %}
         {% block footer %}
            <footer>My Footer</footer>
         {% endblock %}
    {% endblock %}

```diff
! here you can define menu and footer that will be call in admin part 
```

3- Example of my **simple** *admin/myModule/index.html.twig* :

    {% extends 'admin/admin.html.twig' %}
         {% block container %}
            <div>My content of index.html.twig</div>
         {% endblock %}
    {% endblock %}
    
```diff
! here you can overload block container with new data for each admin/module/ files
```

```diff
- Important : all twig blocks have to be define in base.html.twig. If you define one in child of base it won't show you the content in the new twig block.
```

### Assets

Make you link relative with Asset Bundle. More infos : https://symfony.com/doc/current/frontend/encore/versioning.html

- Download bundle asset :

`composer require asset`

###### IF YOU ALREADY HAVE CSS
- Copy paste you asset/ directory in Public/ (at the root of the project)

MyProject/

    Public/
        asset/
             css/
             js/
             ...   

- Copy paste link css in base.html.twig

`<link href="assets/css/myCss.css" rel="stylesheet" type="text/css" >`

- Use asset function from Asset Bundle

`<link href="{{ asset('assets/css/myCss.css') }}" rel="stylesheet" type="text/css">`
    
### Multilingue

Structure of column multilingue is in **JSON**, like below. 

    {
        "fr" : "mon titre français",
        "en" : "my english title"
    }
    
In controllers set slug {lang} parameter : `@Route("/{`**lang**`}/article/{id}", name="showOneArticle")`

In route's method use the value of slug lang : `public function showOneArticle(`**$lang**`, $id){}`

Make a query - available soon