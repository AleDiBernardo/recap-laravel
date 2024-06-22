[README](../README.md)<br>
### **Precedente:**
[OPERAZIONI CRUD](CRUD.md)

---

### Form Request Validation

Le form request validation sono utilizzate per validare automaticamente i dati inviati da un form HTTP prima di elaborarli nel controller.

#### Creazione di una Form Request


```bash
php artisan make:request StorePostRequest
```

Questo creerà un nuovo file `StorePostRequest.php` nella directory `app/Http/Requests`.
<br>

#### Metodi nella Classe di Validazione

##### Metodo `authorize()`

Il metodo `authorize()` determina se l'utente corrente è autorizzato a effettuare la richiesta. Per autorizzare tutti gli utenti, modificare il metodo come segue:

```php
public function authorize()
{
    return true;
}
```

##### Metodo `rules()`

Il metodo `rules()` definisce le regole di validazione per i campi del form:

```php
public function rules()
{
    return [
        'title' => 'required|string|max:255',
        'body' => 'required|string',
        'publish_date' => 'nullable|date',
    ];
}
```

#### Esempio Completo di una Form Request

Supponiamo di voler validare i dati per un form di creazione di post:

1. **Creazione della Form Request**

   ```bash
   php artisan make:request StorePostRequest
   ```

2. **Implementazione della Form Request**

   ```php
   // app/Http/Requests/StorePostRequest.php

   namespace App\Http\Requests;

   use Illuminate\Foundation\Http\FormRequest;

   class StorePostRequest extends FormRequest
   {
       public function authorize()
       {
           return true;
       }

       public function rules()
       {
           return [
               'title' => 'required|string|max:255',
               'body' => 'required|string',
               'publish_date' => 'nullable|date',
           ];
       }
   }
   ```

3. **Utilizzo nella Funzione del Controller**

   ```php
   // app/Http/Controllers/PostController.php

   namespace App\Http\Controllers;

   use App\Http\Controllers\Controller;
   use App\Http\Requests\StorePostRequest;

   class PostController extends Controller
   {
       public function store(StorePostRequest $request)
       {
           // I dati sono già validati a questo punto
           // Eseguire la logica per salvare il post
       }
   }
   ```

Le form request validation in Laravel aiutano a separare la logica di validazione dalla logica di business (gestione dei dati) nei controller, migliorando la manutenibilità del codice e garantendo una gestione sicura dei dati inviati tramite form HTTP.


### **Successivo:**
[FILE_STORAGE](FILE_STORAGE.md)