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
