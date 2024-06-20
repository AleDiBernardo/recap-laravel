[README](../README.md)<br>

### **Precedente:**
[VIEW](VIEW.md)

---

## CONTROLLER
### Creare un Controller
In `app/Http/Controllers`, creare i controller:
```sh
php artisan make:controller NomeModelController
```

### Collegare il Model al Controller
```php
use App\Models\NomeModel;

class NomeModelController extends Controller {
    // prossimo passaggio
}
```

### Funzione `index()`
Questa funzione esegue ciò che era previsto nella route:
```php
public function index() {
    $data = config("nomeFile.nomeArrayAssociativo");
    return view('home', compact("data"));
}
```

### Richiamare il Controller nella Route
```php
Route::get('/', [NomeModelController::class, 'index'])->name('home');
```

### Collegare il Controller in `web.php`
```php
use App\Http\NomeModelController;
```

### Prelevare Tutti i Dati della Tabella
```php
public function index() {
    $booksList = NomeModel::all(); // array di oggetti
    dd($booksList); // per stampare
    dd($booksList[0]->title); // classico array
    return view('books', compact('booksList'));
}
```

### Usare `where`:
```php
Book::where('best_seller',1)->get();
Book::where('best_seller', '!=', 1)->get(); //array
Book::where('best_seller', '!=', 1)->orderByDesc('title')->get();
Book::where('best_seller', '!=', 1)->orderByDesc('title')->limit(10)->get();
Book::where('best_seller', '!=', 1)->first(); //oggetto Model
```

### Usare `find()`:
```php
Book::find($id); //oggetto Model
```

### Salvare un dato:
```php
//istanziare oggetto ecc... 
$book->save();
```

### **Successivo:**
[OPERAZIONI CRUD](CRUD.md)