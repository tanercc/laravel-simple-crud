# Laravel Simple CRUD
### PHP 8
```
# php extentions
$ sudo apt install php-xml php-mbstring php-sqlite3

# create project
$ composer create-project laravel/laravel=9 backend
$ cd backend
$ composer install
```

### Edit .env
```
$ nano .env

DB_CONNECTION=sqlite
DB_DATABASE=/home/taner/projects/backend/storage/app/db.sqlite
DB_FOREIGN_KEYS=true
```

### Create sqlite db and key
```
touch ./storage/app/db.sqlite
php artisan key:generate
```

### Create Post Modal with migration
```
$ php artisan make:model Post -m
```

### Create Post Request for validation
```
$ php artisan make:request StorePostRequest
```

### Create Post Resource for show clean data
```
$ php artisan make:resource PostResource 
```

### Create Post Api controller
```
$ php artisan make:controller Api/PostController --model=Post 
```

### edit database/migrations/create_posts_table.php
```
# add schema
$table->id();
$table->string('title');
$table->text('description');
$table->timestamps();
```
### edit app/Http/Models/Post.php
```
# add var 
protected $fillable = ['title', 'description'];
```
### edit app/Http/Requests/StorePostRequest.php
```
# add rules method to
'title' => ['required', 'max:70'],
'description'  => ['required']
```

### edit app/Http/Controllers/Api/PostController.php
```
# add use part
use App\Http\Requests\StorePostRequest;
use App\Http\Resources\PostResource;

# add index method to
$posts = Post::all();
return PostResource::collection($posts);

# add store method to
$posts = Post::create($request->all());        
return new PostResource($posts);

# add show method to
return $post;

# add update method to
$post->update($request->all());       
return new PostResource($post);

# add destroy method to
$post->delete();
return response(null, 204);
```

### edit routes/api.php
```
# add 
use App\Http\Controllers\Api\PostController;
Route::apiResource('posts',PostController::class);
```

### Create tables
```
$ php artisan migrate
``` 

### Run project
```
# http://localhost:8000
$ php artisan serve
```

#### List posts
GET
*http://localhost:8000/api/posts*

#### Get post
GET
*http://localhost:8000/api/posts/1*

#### New post
POST
*http://localhost:8000/api/posts*

formdata\
title = Test title\
description = Test text

#### Update post
PUT
*http://localhost:8000/api/posts/1*

formdata\
_method = PUT\
title = Test title\
description = Test text

#### Delete post
DELETE
*http://localhost:8000/api/posts/1*

_method = DELETE
