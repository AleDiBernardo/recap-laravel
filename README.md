### RECAP LARAVEL
---
# PRIMI PASSAGGI


## 1. Creare una Repository su GitHub da Template

- ### creare la repo per il template 
- ### andare in setting e spuntare `Template repository`

    ![alt text](image.png)
- ### cliccare sul bottone `Use this template`
    ![alt text](image-1.png)
- ### successivamente su `Create a new repository`
    ![alt text](image-2.png)
- ### mettere il nome della repo e successivamente `Create repository`
- ### clonare la propria repo in locale

## 2. Installare le Dipendenze
```sh
composer install
```

```sh
npm install
```

## 3. Copia di `.env`
- Copiare `.env.example`
- Incollare e rinominare come `.env`

## 4. Generazione della Chiave
```sh
php artisan key:generate
```

## 5. Avviare i Server
### Su due terminali diversi
```sh
php artisan serve
```
```sh
npm run dev
```

## 6. Aprire il Link di Artisan Serve

## 7. Avviare MAMP/LAMP

## 8. Creare il Database in MAMP/LAMP

## 9. Importare il File del Database
---

# PRENDERE DATI DA UN DATABASE

## Collegare il Progetto al Database
In `.env`, settare le seguenti informazioni:

```env
DB_PORT=mettereLaPorta
DB_DATABASE=nomeDatabase
DB_USERNAME=root
DB_PASSWORD=root OR vuota OR passwordDatabase
```

## Interagire con l'ORM
Utilizzare Eloquent, l'ORM di Laravel.

---

## MIGRATION
### Creazione della Migration

Per creare una nuova migration, usa il seguente comando:
```sh
php artisan make:migration create_nome_tabella_table
```
Esempio: `create_houses_table`

La migration si troverà in `database/migrations`.

### Funzioni della Migration

#### Funzione `up()`

- Viene chiamata all'avvio e si occupa di creare la tabella tramite `Schema::create()` con la primary key.
- All'interno della tabella, oltre all'id, ci saranno i campi `CREATED_AT` e `UPDATED_AT` che tengono conto della data di creazione e aggiornamento della tabella.

#### Funzione `down()`

- Viene chiamata quando si vuole tornare indietro, solitamente per eliminare la tabella.

### Avviare le Migration

Per avviare le migration, utilizza il comando:
```sh
php artisan migrate
```
Questo comando esegue la funzione `up()` di tutte le migration non ancora eseguite.

**IMPORTANTE:** Grazie a questo comando, il database sarà sempre aggiornato.

### Batch

Il batch fornisce la cronologia delle modifiche appartenenti allo stesso batch. È possibile vedere i batch nella tabella `migrations` del database.

### Rollback

Per eseguire il rollback dell'ultimo batch, utilizza il comando:
```sh
php artisan migrate:rollback
```
Questo comando rimuove l'ultimo batch eseguendo la funzione `down()` delle migration appartenenti allo stesso batch, in poche parole esegue il `DROP TABLE`.

### Migration di Modifica

Per creare una migration di modifica, utilizza il comando:
```sh
php artisan make:migration update_nome_tabella_table --table=nome_tabella
```

Si utilizza quado per esempio si lavora in team e ci sono delle modifiche per colpa di un errore e invece di dire al team di fare rollback (sarebbe complicato) facciamo una migration di modifica semplicemente.

Questo comando creerà una nuova migration con `Schema::table()` al posto di `Schema::create()`.

### Cancellare le Migration

Per cancellare tutte le migration dal database e tutti i dati, utilizza il comando:
```sh
php artisan migrate:reset
```
Le migration nel progetto restano.

**<span style="color: red;">ATTENZIONE:</span>** **è un comando pericoloso soprattutto in produzione.**

---

# SEEDER

## Creare il Seeder
```bash
php artisan make:seeder NomeTableSeeder
```
Lo troveremo in `database/seeders`.

### Funzione `run()`
**Verrà eseguita all'inizio.**

```php

class HousesTableSeeder extends Seeder{
	
	public function run(): void
	{
		//code here
	}
}
```

## Creazione di un Record
```php
class HousesTableSeeder extends Seeder {
    public function run(): void {
        $newHouse = new House(); // Istanza del model
        $newHouse->address = "dato";
        $newHouse->city = "Milano";
        
        dd($newHouse); // Stamperà un model di house
    }
}
```

## Eseguire un Seeder
```bash
php artisan db:seed --class=HousesTableSeeder
```
> **Nota:** Per ora non ci sono i dati nel database.

## Salvare i Dati
```php
class HousesTableSeeder extends Seeder {
    public function run(): void {
        $newHouse = new House(); // Istanza del model
        $newHouse->address = "dato";
        $newHouse->city = "Milano";
        
        // dd($newHouse); // Stamperà un model di house
        
        $newHouse->save(); // Per salvare i dati e aggiungerli al database
    }
}
```

## Come Usare la Classe Faker per Generare Dati Fittizi

[Documentazione Faker](https://fakerphp.org/)

### Come Usarlo
```php
use Faker\Generator as Faker;

class HousesTableSeeder extends Seeder {
    public function run(Faker $faker): void {
        $newHouse = new House(); // Istanza del model
        $newHouse->address = $faker->streetAddress();
        $newHouse->city = $faker->city();
        
        // dd($newHouse); // Stamperà un model di house
        
        $newHouse->save(); // Per salvare i dati e aggiungerli al database
    }
}
```

> **Nota:** Se si volessero più dati basta fare un ciclo `for`.

```php
use Faker\Generator as Faker;

class HousesTableSeeder extends Seeder {
    public function run(Faker $faker): void {
        for ($i = 0; $i < 10; $i++) { // Genera 10 record
            $newHouse = new House(); // Istanza del model
            $newHouse->address = $faker->streetAddress();
            $newHouse->city = $faker->city();
            
            $newHouse->save(); // Per salvare i dati e aggiungerli al database
        }
    }
}
```
---

## MODEL
### Creare il Model
```sh
php artisan make:model NomeModel
```
Il nome deve essere come quello della tabella del database, ma al singolare e con l'iniziale maiuscola. Il model sarà già collegato alla tabella corrispondente e avrà tutti i dati.

---

## VIEW
### Creare la View
Creare `nomeTabella.blade.php` in `views` e usare `@extends` e `@section`.

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


# ALTRI PASSAGGI
---
## Ricorda
Il CSS viene scritto in `app.scss` oppure in `partials/_nomeFile.scss`, da includere in `app.scss` tramite:
```scss
@use "./partials/nomeFile" as *;
```

## Impostare Altre Rotte
In `web.php`, possiamo creare altre rotte e assegnare un nome con `->name('nome')`:
```php
Route::get('/about', function () {
    return view('about');
})->name('about');
```

## Creare un Layout
Nella cartella `views/layouts`, creare il layout base del sito chiamato `app.blade.php`. Utilizzare `@yield('content')` come segnaposto per inserire contenuti diversi in ogni pagina nel passo successivo.

## Estendere il Layout
Estendere il layout nelle altre pagine:
```php
@extends('layouts.app')

@section('content')
// QUI VA IL CONTENUTO DELLA PAGINA
@endsection
```

## Gestire il Codice
Possiamo creare una cartella `views/partials` per mettere pezzi di codice riutilizzabili come l'header e il footer, e includerli dove servono (nei file layouts) con `@include`.

## Inserire Immagini
All'interno dell'attributo `src` di un tag `img`, usare:
```php
{{ Vite::asset('percorso.estensione') }}
```

## Prendere Dati da un File (sconsigliato)
Mettere il file con i dati nell'array associativo all'interno della cartella `config`. Per prendere i dati, andare in `web.php` e nella rotta della pagina dove servono i dati, fare:
```php
$data = config("nomeFile.nomeArrayAssociativo");
```
Infine, passare i dati tramite `compact`:
```php
Route::get('/', function () {
    $data = config("nomeFile.nomeArrayAssociativo");
    return view('home', compact("data"));
})->name('home');
```


