# ProjectInLaravelWithVueJsUsingRESTfulApi
An article gallery created using laravel as backend and vue js as frontend. Restful api is used to carry out crud operations and search operation.

Steps carried out:

>At first laravel project(laravelVueJs) is created

>Laravel contain Vue Js by default so run npm install

>Connect to db, create table(article) with fields title and body

>Inorder to insert dummy data to article table create seeder for that run php artisan make:seeder ArticleTableSeeder and create factory, run php artisan make:factories ArticleFactory. Also need some change in DatabaseSeeder.

>Define routes for API calls in routes/api.php

>Create controller run php artisan make:controller ArticleController, and define the functions mentioned in api.php

>You can test the api routes  in Postman and see the result

>Open resources/js/components delete existing file and create two new files Articles.vue and Navbar.vue. Make some changes in app.js file.

>Open welcome.blade.php file get some bootstrap css and js, and make the changes
