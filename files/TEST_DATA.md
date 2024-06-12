## Come Usare la Classe Faker per Generare Dati Fittizi (metodo 1)

[Documentazione Faker](https://fakerphp.org/)
> **Nota:** Faker è implementato in Laravel


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

## Usare Faker con Locale in Italiano

### Nella Funzione `run()`
```php
use Faker\Factory as Faker;

class HousesTableSeeder extends Seeder {
    public function run(): void {
        $faker = Faker::create('it_IT');

        $newHouse = new House(); // Istanza del model
        $newHouse->address = $faker->streetAddress();
        $newHouse->city = $faker->city();

        $newHouse->save(); // Per salvare i dati e aggiungerli al database
    }
}
```

## Prendere Dati da File di Configurazione (metodo 2)

### Creare il File `seeder-data.php` con dati creati da ChatGPT
Creiamo il file `config/seeder-data.php` con il nostro array di array associativi:

```php
<?php
return [
    // array associativi
    [
        'address' => 'Via Roma 1',
        'city' => 'Milano'
    ],
    [
        'address' => 'Via Milano 2',
        'city' => 'Roma'
    ],
    // aggiungere altri dati se necessario
];
```

### Utilizzare i Dati nella Funzione `run()`
```php
class HousesTableSeeder extends Seeder {
    public function run(): void {
        $housesData = config('seeder-data');

        foreach ($housesData as $houseData) {
            $newHouse = new House(); // Istanza del model
            $newHouse->address = $houseData['address'];
            $newHouse->city = $houseData['city'];
            
            $newHouse->save(); // Per salvare i dati e aggiungerli al database
        }
    }
}
```

## Come Non Ripetere Sempre le Chiavi (non funziona)

Nel caso in cui i dati nell'array hanno lo stesso nome delle chiavi che corrispondono ai nomi delle colonne e abbiamo inserito l'attributo `fillable` nella classe del model sul quale lavoriamo, possiamo fare così:

### House.php
```php
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class House extends Model {
    use HasFactory;
    
    protected $fillable = [
        'address', 'city', 'state', 'square_meters', 'rooms_number', 
        'description', 'price', 'is_available', 'energy_rating'
    ];
}
```

### Utilizzare `fill` nella Funzione `run()`
```php
class HousesTableSeeder extends Seeder {
    public function run(): void {
        $housesData = config('seeder-data');

        foreach ($housesData as $houseData) {
            $newHouse = new House(); // Istanza del model
            $newHouse->fill($houseData);
            
            $newHouse->save(); // Per salvare i dati e aggiungerli al database
        }
    }
}
```

## Prendere Dati Tramite API (metodo 3)

### Esempio di Utilizzo di API nella Funzione `run()`
```php
use Illuminate\Support\Facades\Http;

class HousesTableSeeder extends Seeder {
    public function run(): void {
        $response = Http::get('https://api.example.com/houses');
        $housesData = $response->json();

        foreach ($housesData as $houseData) {
            $newHouse = new House(); // Istanza del model
            $newHouse->fill($houseData);

            $newHouse->save(); // Per salvare i dati e aggiungerli al database
        }
    }
}
```

## Ricordati di:

### 1. Creare la Tabella `Players`

#### Creare la Migration
```bash
php artisan make:migration create_players_table
```

#### Definire la Migration
```php
// database/migrations/xxxx_xx_xx_xxxxxx_create_players_table.php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreatePlayersTable extends Migration {
    public function up(): void {
        Schema::create('players', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->integer('age');
            $table->string('position');
            $table->timestamps();
        });
    }

    public function down(): void {
        Schema::dropIfExists('players');
    }
}
```

### 2. Far Partire la Migration
```bash
php artisan migrate
```

### 3. Creare il Model `Player`
```bash
php artisan make:model Player
```

### 4. Creare il Seeder `PlayersTableSeeder`
```bash
php artisan make:seeder PlayersTableSeeder
```

## Effettuare la Chiamata API

### Configurare il Token nel File `.env`
```env
FUTDB_API_TOKEN=your_token_here
```

