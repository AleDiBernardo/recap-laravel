[README](../README.md)<br>
### **Precedente:**
[FORM_REQUEST_VALIDATION](FORM_REQUEST_VALIDATION.md)

---

# File Storage

#### [Documentazione](https://laravel.com/docs/10.x/filesystem#main-content)
> **Nota:** Versione 10

## Creare la colonna contenente il nome del file

Aggiungi una colonna alla tua tabella del database per memorizzare il nome o il percorso del file.

```php
Schema::table('your_table', function (Blueprint $table) {
    $table->string('nome_campo')->nullable();
});
```

## Creare l'input per il file in `create.blade.php`

Aggiungi un input per ricevere l'immagine nel tuo form.

```html
<form
  action="{{ route('your_route') }}"
  method="POST"
  enctype="multipart/form-data"
>
  @csrf
  <input type="file" name="nome_campo" />
  <button type="submit">Submit</button>
</form>
```

## Salvare i file in `storage/app/public`

### Collegare lo Storage a Public

Esegui il seguente comando per creare un link simbolico.

```bash
php artisan storage:link
```

Questo creerà una cartella `storage` in `public`.

### Assicurarsi della Configurazione in `config/filesystems.php`

Imposta il disco predefinito su `public`.

```php
return [

    /*
    |--------------------------------------------------------------------------
    | Default Filesystem Disk
    |--------------------------------------------------------------------------
    |
    | Here you may specify the default filesystem disk that should be used
    | by the framework. The "local" disk, as well as a variety of cloud
    | based disks are available to your application. Just store away!
    |
    */

    'default' => env('FILESYSTEM_DISK', 'public'),

    //continuo del file

```

## Salvare il File nel Controller

Nel tuo controller, controlla se il file è presente nella richiesta e poi salvalo.

```php
public function store(Request $request)
{
    if($request->hasFile('nome_campo')) {

        // salvo il file nello storage
        $image_path = Storage::put('nome_cartella', $request->nome_campo);

        // salvo il path del file nei dati della tabella
        $data['nome_campo'] = $image_path;
    }
}
```

**Nota:** Se utilizzi la proprietà `fillable` nel tuo modello, aggiungi il nuovo campo.

```php
protected $fillable = ['nome_campo', ...];
```

## Leggere l'Immagine

Per visualizzare l'immagine, utilizza il seguente codice nel tuo template Blade.

```html
<img src="{{ asset('storage/' . $nome->nome_campo) }}" alt="Image" />
```

## Cancellare l'Immagine

Per cancellare l'immagine, utilizza il seguente codice.

```php
public function destroy(Post $post)
{
    if($post->nome_campo) {
        Storage::delete($post->nome_campo);
    }
}
```

## Modificare l'Immagine

Nel form di `edit.blade.php`, assicurati di avere `enctype="multipart/form-data"`.

```html
<form
  action="{{ route('your_update_route', $post->id) }}"
  method="POST"
  enctype="multipart/form-data"
>
  @csrf @method('PUT')
  <input type="file" name="nome_campo" />
  <button type="submit">Update</button>
</form>
```

Nel metodo di update del tuo controller, gestisci il caricamento del file.

```php
public function update(Request $request, Post $post)
{
    if($request->hasFile('nome_campo')) {

        if($post->nome_campo) {
            Storage::delete($post->nome_campo);
        }

        $image_path = Storage::put('nome_cartella', $request->nome_campo);
        $data['nome_campo'] = $image_path;
    }
}
```

