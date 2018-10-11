## My best practices for symfony project

```diff
+ Sorry for my english
```

|      Create project                                                                                                     |     Route                                                                          | Twig                                                                                                          | Asset                                                                                           | Multilingue                                                                                              |
| ----------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| <a href="https://github.com/emiliesappracone/symfony_read#start-">Start</a>                                             | <a href="https://github.com/emiliesappracone/symfony_read#bundles-"> Routes</a>    | <a href="https://github.com/emiliesappracone/symfony_read#twig-"> Twig</a>                                    | <a href="https://github.com/emiliesappracone/symfony_read#assets"> Assets</a>                   | <a href="https://github.com/emiliesappracone/symfony_read#--mysql"> Mysql </a>                           | 
| <a href="https://github.com/emiliesappracone/symfony_read#bundles-"> Bundles</a>                                        |                                                                                    | <a href="https://github.com/emiliesappracone/symfony_read#my-twig-structure-for-symfony-">Structure</a>       |                                                                                                 | <a href="https://github.com/emiliesappracone/symfony_read#--when-makeentity">At creation of entity</a>   |                                                                                   
| <a href="https://github.com/emiliesappracone/symfony_read#env-">Env</a>                                                 |                                                                                    |                                                                                                               |                                                                                                 | <a href="https://github.com/emiliesappracone/symfony_read#--with-querybuilder-make-read-and-update-queries-">QueryBuilder</a> |
| <a href="https://github.com/emiliesappracone/symfony_read#create-database">Create Database</a>                          |                                                                                    |                                                                                                               |                                                                                                 | <a href="https://github.com/emiliesappracone/symfony_read#1--create-functions-read-and-update-in-entityrepository">EntityRepository</a>  |
| <a href="https://github.com/emiliesappracone/symfony_read#build-entities-and-controllers">Entities and Controllers</a>  |                                                                                    |                                                                                                               |                                                                                                 | <a href="https://github.com/emiliesappracone/symfony_read#2--call-functions-in-entitycontroller">EntityController</a> |
| <a href="https://github.com/emiliesappracone/symfony_read#build-entities-with-association">Entities associations</a>    |                                                                                    |                                                                                                               |                                                                                                 | <a href="https://github.com/emiliesappracone/symfony_read#2--in-view">Twig</a> |    

## Start :
`composer create-project symfony/skeleton MyProject`

## Bundles :
`composer require twig`

`composer require annotations`

`composer require profiler --dev`

`composer require maker --dev`

`composer require doctrine`

## Env :
- change database connexion

## Create Database
`php bin/console doctrine:database:create`

## Build entities and controllers
- Make Controller :

`php bin/console make:controller EntityController`

- Make Entity :

`php bin/console make:entity` 

```diff
When you create an entity : this will create Entity (Model), EntityRepository, Entity templates twig 
EntityRepository is where you'll create 
```

- Choose name of Entity (Table mysql)
- Choose all property (Column mysql)
- Edit migration file

`php bin/console make:migration`

- Persist new Entity in Database

`php bin/console doctrine:migrations:migrate`

## Build entities with association

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

## Routes

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

## Twig

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

## Assets

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

## Forms

FormBuilder bundle is used to make EntityType class where you'll store all forms for one Entity. Then you can use those forms everywhere.

**1- Add bootstrap form style to twig : add line below in config/packages/twig.yaml**

```diff
+ form_themes: ['bootstrap_4_layout.html.twig']
```
 
**2- FormBuilder installation :** `composer require symfony/form`

I used to install form validator which constraints field to be not empty based on a set of rules in Entity (Model > property type). 

**3- Validator installation :** `composer require symfony/validator`

For instance :

    /**
    * @Assert\NotBlank() ! means not empty
    * @Assert\Type(
                    type="integer",
                    message="Not integer"
                  )
    private $id;

**Default** assert type is **string**.

**List of assert type** : array, bool, callable, float, double, int, integer, iterable, long, null, numeric, object, real, resource, scalar, string, and others ... 

**4- Build form entity**
  
###### A- IN CASE OF UPDATE DESCRIPTION OF SERVICES ENTITY - **MAKE YOUR OWN QUERIES**

```diff
! cd rootOfYouProject
! mkdir Form
! cd Form/
! touch EntityType.php

or use PhpStorm or all other IDE
```

Make directory Form/ where you'll store all forms entity. In it make a file call EntityType (depends on entity's name).
Extends AbstractType class.

###### EntityType file (example : ServicesType)

    
    namespace App\Form;

    use App\Entity\Services;
    
    use Symfony\Component\Form\AbstractType;
    use Symfony\Component\Form\Extension\Core\Type\SubmitType;
    use Symfony\Component\Form\Extension\Core\Type\TextareaType;
    use Symfony\Component\Form\FormBuilderInterface;
    
    class ServicesType extends AbstractType
    {
        /**
        *   THIS IS THE FORM JUST TO UPDATE DESCRIPTION 
        */
        public function buildForm(FormBuilderInterface $builder, array $options)
        {
            $builder
                ->add('description', TextareaType::class)
                ->add('submit', SubmitType::class)
            ;
        }
    
        /**
        *   Every form needs to know the name of class (entity) that holds the underlying data
        *   HERE SET TO NULL CAUSE IT'S A SPECIAL QUERY
        */
        public function configureOptions(OptionsResolver $resolver)
        {
            $resolver->setDefaults(['data_class' => null]);
        }
    }
        
    
```diff
! Be aware of what's class you import/use for fieldType, only use Type of Form bundle. 
! But for EntityType use Doctrine bundle. Be sure you use the good Type class.
```    

###### EntityRepository file (example : ServicesRepository)

    /**
     * UPDATE ONLY DESCRIPTION FROM services entity
     * @param $lang
     * @param $id
     * @return bool
     * @throws \Doctrine\DBAL\DBALException
     */
    public function updateServiceDescription($id, $description){
        $connexion = $this->getEntityManager()->getConnection();
    
        $query = 'UPDATE services
                SET description = :description
                WHERE id = :id';
        $stmt = $connexion->prepare($query);
        $stmt->bindParam(':id' ,$id);
        $stmt->bindParam(':description' ,$description);
        return $stmt->execute();
    }

###### EntityController file (example : ServicesController)

    /**
    * UPDATE DESCRIPTION OF SERVICE 
    * @Route("/{lang}/services/update/{id}", name="updateServiceDescription")
    */
    public function updateDescription($lang, $id, Request $request){
        // call what service need to be updated
        /* method called below is "unappropiated" no need to get data by lang, only by id */   
        $service = $this->getDoctrine()->getRepository(Services::class)->findByServicesByLangAndId($lang, $id);
        // call form entity class form
        $form = $this->createForm(ServicesType::class, $service);
        // pass request to form
        $form->handleRequest($request);
        // check if $form is submitted
        if($form->isSubmitted() && $form->isValid()){
            // get data in form
            $value = $form->getData();
            // get new description
            $newDescription = $value['description'];
            // update description function
            $this->getDoctrine()->getRepository(Services::class)->updateServiceDescription($id, $newDescription);
            // redirect to route
            return $this->redirectToRoute("servicesByLang", ["lang" => $lang, "id" => $id]);
        }
        // if form not already submitted or not valid call createView method from form to send form in twig view
        return $this->render('admin/services/form.html.twig',
            [
                'form' => $form->createView(),
            ]
        );
    }

###### Entity view file retrieve all (example : admin/services/index.html.twig)

Here is a table you have to make to show all services. 
In it just add link route to edit form : 

`<a href="{{ path("updateServiceDescription", {"id":service.id, "lang":lang}) }}"><i class="fa fa-pencil text-warning"></i></a>`
                                                                                                                    

###### Entity view file update one (example : admin/services/form.html.twig)

This is a basic call to form in view.

    {% extends 'base.html.twig' %}
    
    {% block body %}
        {{ form_start(form) }}
        {{ form_end(form) }}
    {% endblock %}

###### B- IN CASE OF UPDATE ONE SERVICE OF SERVICES ENTITY - **USE MANAGER TO UPDATE**

##### EntityType => ServicesType

     public function buildForm(FormBuilderInterface $builder, array $options)
     {
         $builder
             ->add('description', TextareaType::class)
             ->add('is_highlight', CheckboxType::class, ['required' => false])
             ->add('is_valid', CheckboxType::class, ['required' => false])
             ->add('name', CollectionType::class, [])
             ->add('submit', SubmitType::class)
         ;
     }
    
     public function configureOptions(OptionsResolver $resolver)
     {
         $resolver->setDefaults(['data_class' => Services::class]);
     }
     
```diff
+ Change from A-
* Add all fields/properties of entity as input/textarea/etc...
* Important with CollectionType input you can change JSON of you property
* Add Service::class at setDefaults method of configureOptions method => purpose is to link entity and data from form
```     
    
##### EntityController => ServicesController

    /**
     * UPDATE DESCRIPTION OF SERVICE
     * @Route("/{lang}/services/update/{id}", name="updateServiceDescription")
     */
    public function updateDescription($lang, $id, Request $request){
        // call what service need to be updated
        /* method only by id */
        $service = $this->getDoctrine()->getRepository(Services::class)->find($id);
        // call form entity class form
        $form = $this->createForm(ServicesType::class, $service);
        // pass request to form
        $form->handleRequest($request);
        // check if $form is submitted
        if($form->isSubmitted() && $form->isValid()){
            $this->getDoctrine()->getManager()->persist($service);
            $this->getDoctrine()->getManager()->flush();
            // redirect to route
            return $this->redirectToRoute("servicesByLang", ["lang" => $lang, "id" => $id]);
        }
        // if form not already submitted or not valid call createView method from form to send form in twig view
        return $this->render('admin/services/form.html.twig',
            [
                'form' => $form->createView(),
            ]
        );
    } 

```diff
+ Change from A-
* Get service by id
* Processed of collecting data from $form doesn't exist anymore
* By adding Service::class at setDefaults method of configureOptions method => you linked the entity 
> then all you have to do is to pass $service in parameter of persist method
```    

###### C- PERSONALIZE FORM 
       
available soon
       
## Multilingue

##### - MySQL
Structure of column multilingue is in **JSON**, like below.
 
| myProperty                            |
| ------------------------------------- |
|{   <br>"fr" : "mon titre français",<br>"en" : "my english title"<br>} |                                  

##### - When make:entity

    > php bin/console make:entity
    > Choose entity Name : MyEntity
    > Add attribut/property : myProperty
    > - Type property : json
    > ... 
    
##### - In controllers : 
set slug {lang} parameter in route : `@Route("/{`**lang**`}/article/{id}", name="showOneArticle")`

##### - In route's method :
use the value of slug lang : `public function showOneArticle(`**$lang**`, $id){}`

##### - With queryBuilder make read and update queries :

###### 1- CREATE FUNCTIONS READ AND UPDATE IN ENTITYREPOSITORY

QueryBuilder works with EntityManager. It allows you to build queries : C.R.U.D personalized.

- READ :

Retrieve all services infos and name by lang :

     /**
      * @return Services[] Returns an array of Services objects
      */
     public function findByServicesByLang($lang)
     {
         $connexion = $this->getEntityManager()->getConnection();
   
         $query = 'SELECT id, JSON_UNQUOTE(JSON_EXTRACT(name, "$.'.$lang.'")) as name, description, is_highlight, is_valid
                 FROM services';
         $stmt = $connexion->prepare($query);
         $stmt->execute();
         return $stmt->fetchAll();
     }

- UPDATE : 

Update name by lang and id ofc :

     /**
     * @param $lang
     * @param $id
     * @return bool
     * @throws \Doctrine\DBAL\DBALException
     */
     public function updateServiceByLangAndId($lang,$id){
          $connexion = $this->getEntityManager()->getConnection();
       
          $query = 'UPDATE services
                    SET name = JSON_REPLACE(name, "$.'.$lang.'", "Jean-Pierre")
                    WHERE id = :id';
          $stmt = $connexion->prepare($query);
          $stmt->bindParam(':id' ,$id);
          return $stmt->execute();
     }

###### 2- CALL FUNCTIONS IN ENTITYCONTROLLER

    /**
     * @method show all services by lang
     * @Route("/{lang}/services", name="servicesByLang")
     */
    public function servicesByLang($lang){
        $services = $this->getDoctrine()->getRepository(Services::class)->findByServicesByLang($lang);
        return $this->render('front/services/index.html.twig', [
            'services' => $services,
            'lang' => $lang
        ]);
    }    
   

    /**
     * @method update service with id and lang
     * @Route("/{lang}/services/update/{id}", name="updateServiceByLangAndId")
     */
    public function updateService($lang, $id){
        $result = $this->getDoctrine()->getRepository(Services::class)->updateServiceByLangAndId($lang, $id);
        // if update is done successfully
        if($result){
            // redirect to route
            return $this->redirectToRoute("servicesByLang", ["lang" => $lang]);
        }
    }

###### 3- IN VIEW

    {% for service in services %}
        <tr>
            <td>{{ service.id }}</td>
            {% if service.name is not null %}
                <td>{{ service.name }}</td>
            {% else %}
                <td> - </td>
            {% endif %}
            <td>{{ service.description }}</td>
            <td>
                <a href="{{ path("showOneService", {"id":service.id, "lang":lang}) }}"><i class="fa fa-eye"></i></a>
                <a href="{{ path("updateTest", {"id":service.id, "lang":lang}) }}"><i class="fa fa-pencil text-danger"></i></a>
            </td>
        </tr>
    {% endfor %}
    
    