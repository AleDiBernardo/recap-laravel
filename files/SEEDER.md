[README](../README.md)<br>

### **Precedente:**
[MIGRATION](MIGRATION.md)

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

> **Nota:** Prima ti servono i dati, **[3 metodi](TEST_DATA.md)** per gererarli

### Modificare il Seeder
```php
// database/seeders/PlayersTableSeeder.php

use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\Http;
use App\Models\Player;

class PlayersTableSeeder extends Seeder {
    public function run(): void {
        $response = Http::withHeaders([
            'X-AUTH-TOKEN' => env('FUTDB_API_TOKEN')
        ])->get('https://futdb.app/api/players');
        
        $data = $response->json(); // Prendere i dati come array associativo

        foreach ($data['items'] as $playerData) {
            $newPlayer = new Player();
            $newPlayer->name = $playerData['name'];
            $newPlayer->age = $playerData['age'];
            $newPlayer->position = $playerData['position'];

            $newPlayer->save(); // Salvare i dati e aggiungerli al database
        }
    }
}
```

### Eseguire il Seeder
```bash
php artisan db:seed --class=PlayersTableSeeder
```

### **Successivo:**
[GENERARE DATI PER IL TESTING](TEST_DATA.md)

