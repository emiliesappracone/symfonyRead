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

> Make the controllers

> Make 2 entities

`php bin/console make:entity => Categories`
`php bin/console make:entity => Articles`

> Choose which property will be associated 

`php bin/console make:entity => Articles`

*name : category*

*type : relation*

*relation type : choose between them*

> Edit migration file

> Persist Entities in Database 

  