[README](../README.md)<br>

---

## Creare una Rotta API

### 1. **Definire la Rotta**:

   ```php
   Route::get('/test', function () {
       $user = [
           'name' => 'pippo',
           'role' => 'student'
       ];
   
       return response()->json($user);
   });
   ```

   Questo creerà una rotta accessibile tramite `/api/test` che restituisce dati JSON.

### 2. **Testare con Postman**: 
Utilizza Postman per testare la rotta API. Invia una richiesta GET a `http://il-tuo-dominio/api/test` e verifica che restituisca la risposta JSON prevista.

#### Creare un Controller per l'API


1. **Generare il Controller**:

   ```
   php artisan make:controller Api/PostController
   ```

2. **Implementare la Logica del Controller**: 

   ```php
   <?php
   
   namespace App\Http\Controllers\Api;
   
   use App\Http\Controllers\Controller;
   use App\Models\Post;
   use Illuminate\Http\Request;
   
   class PostController extends Controller
   {
       public function index()
       {
           $posts = Post::all();
           $data = [
               'success' => true,
               'results' => $posts
           ];
           
           return response()->json($data);
       }
   }
   ```

3. **Definire la Rotta**: 

   ```php
   use App\Http\Controllers\Api\PostController;
   
   Route::get('/posts', [PostController::class, 'index']);
   ```

### Gestire gli accessi al server

nel file config/cors.php 

```php
<?php

return [

    /*
    |--------------------------------------------------------------------------
    | Cross-Origin Resource Sharing (CORS) Configuration
    |--------------------------------------------------------------------------
    |
    | Here you may configure your settings for cross-origin resource sharing
    | or "CORS". This determines what cross-origin operations may execute
    | in web browsers. You are free to adjust these settings as needed.
    |
    | To learn more: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
    |
    */

    'paths' => ['api/*', 'sanctum/csrf-cookie'],

    'allowed_methods' => ['*'],

    'allowed_origins' => ['*'], //qui si può decidere chi può accedere 
    // 'allowed_origins' => ['gigino.com'], 

    'allowed_origins_patterns' => [],

    'allowed_headers' => ['*'],

    'exposed_headers' => [],

    'max_age' => 0,

    'supports_credentials' => false,

];

```