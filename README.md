## Create symfony project

### Start :
`composer create-project symfony/skeleton MyProject`

### Bundles :
`composer require twig`
`composer require annotations`
`composer require profiler —dev`
`composer require maker —dev`
`composer require doctrine`

### Env :
> change database connexion

### Create Database
`php bin/console doctrine:database:create`

### Build entities and controllers

> Make Controller :

`php bin/console make:controller EntityController`

> Make Entity :

`php bin/console make:entity` 

Choose name of Entity (Table mysql)

Choose all property (Column mysql)

> Edit migration file

`php bin/console make:migration`

> Persist new Entity in Database

`php bin/console doctrine:migrations:migrate`

### Build entities with association

> Make the controllers like previously

> Make 2 entities`: **Categories** & **Articles**

`php bin/console make:entity`

> Choose which property will be associated, edit one entity with : 

`php bin/console make:entity` 

entity => **Articles**

* *name : category*

* *type : relation*

* *relation type : choose between them*

> Edit migration file like previously

> Persist Entities in Database like previously 

  