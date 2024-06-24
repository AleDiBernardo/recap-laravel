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

### Riepilogo

1. **Migration**: Definisci le chiavi esterne e le relazioni nel file di migrazione usando il metodo `table` di `Schema`.
2. **Model**: Definisci i metodi di relazione nei modelli per rappresentare la relazione uno a molti (1 a N).