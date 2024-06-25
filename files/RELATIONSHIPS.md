[README](../README.md)<br>

---

### Relazioni tra Tabelle 
#### Migration: Aggiunta delle relazioni

Nel file di migrazione, puoi definire le relazioni tra tabelle utilizzando il metodo `table` di `Schema` e il costruttore di Blueprint.

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class AddForeignKeyToNomeTabella extends Migration
{
    public function up()
    {
        Schema::table('nomeTabella', function (Blueprint $table) {
            // Aggiungi la colonna e la chiave esterna
            $table->unsignedBigInteger('nome_id')->nullable()->after('nomeCampo');

            $table->foreign('nome_id')
                ->references('id')
                ->on('nomeTabellaDaCollegare');
        });
    }

    public function down()
    {
        Schema::table('nomeTabella', function (Blueprint $table) {
            // Rimuovi la chiave esterna e la colonna
            $table->dropForeign(['nome_id']);
            $table->dropColumn('nome_id');
        });
    }
}
```

#### Collegare i Model

##### Esempio di relazione Uno a Molti (1 a N)

###### Modello Brand

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Brand extends Model
{
    use HasFactory;

    public function cars()
    {
        return $this->hasMany(Car::class); // Un Brand ha molte Car
    }
}
```

###### Modello Car

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Car extends Model
{
    use HasFactory;

    public function brand()
    {
        return $this->belongsTo(Brand::class); // Una Car appartiene a un Brand
    }
}
```
<br>


### Relazione N a N 

Per collegare le tabelle `accessories` e `cars` in una relazione molti-a-molti (N a N) è necessario creare la tabella ponte e definire le relazioni nei rispettivi modelli.

#### 1. Creare la Tabella pivot

La tabella di pivot conterrà le chiavi esterne di entrambe le tabelle principali.

##### Esempio di Migrazione

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateAccessoryCarTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('accessory_car', function (Blueprint $table) {
            $table->unsignedBigInteger('accessory_id');
            $table->foreign('accessory_id')->references('id')->on('accessories')->cascadeOnDelete();

            $table->unsignedBigInteger('car_id');
            $table->foreign('car_id')->references('id')->on('cars')->cascadeOnDelete();
            $table->primary(['accessory_id','car_id']);

        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('accessory_car');
    }
}
```
> **Nota:** Il nome della tabella pivot deve essere composto dal nome di entrambi le tabelle in ordine alfabetico


#### 2. Definire le Relazioni nei Modelli

##### Modello Accessory

Nel modello `Accessory`, aggiungi una funzione `cars` per definire la relazione molti-a-molti.

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Accessory extends Model
{
    use HasFactory;

    public function cars()
    {
        return $this->belongsToMany(Car::class);
    }
}
```

##### Modello Car

Nel modello `Car`, aggiungi una funzione `accessories` per definire la relazione molti-a-molti.

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Car extends Model
{
    use HasFactory;

    public function accessories()
    {
        return $this->belongsToMany(Accessory::class);
    }
}
```
