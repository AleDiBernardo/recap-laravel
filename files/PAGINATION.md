[README](../README.md)<br>

---

### PAGINAZIONE

#### Controller: Funzione `index`

Sostituisci `::all()` con `::paginate(numeroElementi)` per abilitare la paginazione.

```php
public function index(Request $request) {
    $posts = Post::paginate(10); //Numero di elementi per pagina di default
    return view('posts.index', compact('posts', 'perPage'));
}
```
<br>

#### Mostrare il menù della paginazione

Aggiungi il seguente codice nel tuo file Blade alla fine della pagina per visualizzare la paginazione.

```php
{{ $posts->links() }}
```
<br>

### Usare bootstrap al posto di tilewind
#### App\Providers\AppServiceProvider

Aggiungi questa riga nel metodo `boot` per utilizzare Bootstrap 5 per la paginazione.

```php
use Illuminate\Pagination\Paginator;

public function boot() {
    Paginator::useBootstrapFive();
}
```
<br>

#### Form per selezionare il numero di elementi per pagina

```html
<form method="GET" action="{{ url('/posts') }}">
    <label for="per_page">Items per page:</label>
    <select id="per_page" name="per_page" onchange="this.form.submit()">
        <option value="5" {{ $perPage == 5 ? 'selected' : '' }}>5</option>
        <option value="10" {{ $perPage == 10 ? 'selected' : '' }}>10</option>
        <option value="15" {{ $perPage == 15 ? 'selected' : '' }}>15</option>
        <option value="20" {{ $perPage == 20 ? 'selected' : '' }}>20</option>
    </select>
</form>
```

#### Paginazione con parametri aggiuntivi

Aggiorna la chiamata della paginazione per includere il numero di elementi per pagina come parametro.

```php
{{ $posts->appends(['per_page' => $perPage])->links() }}
```


### Assegnare la Categoria tramite Form in Laravel

#### Mostrare la Categoria nell'index.blade.php

Nel file `index.blade.php`, usa il null safe operator `?` per mostrare la categoria.

```php
<td>{{ $curPost->category?->name }}</td>
```

#### Creare il Post con la Categoria

##### Controller: Funzione `create`

Nel metodo `create` del controller principale, passa tutte le categorie alla vista.

```php
public function create()
{
    $categories = Category::all();
    return view('posts.create', compact('categories'));
}
```

##### Blade: Form di creazione (create.blade.php)

Aggiungi il seguente codice per il form di selezione della categoria.

```html 
<label for="category_id" class="form-label">Categoria</label>
<select class="form-select" name="category_id" id="category_id">
    <option value=""></option>
    @foreach($categories as $category)
        <option value="{{ $category->id }}">{{ $category->name }}</option>
    @endforeach
</select>
```

##### Validazione della Request

Nella request, aggiungi la validazione per l'attributo `category_id`.

```php
public function rules()
{
    return [
        'category_id' => ['nullable'],
        // altre regole di validazione
    ];
}
```

##### Modello: Aggiungi al fillable

Nel modello del Post, assicurati che `category_id` sia aggiunto al `$fillable`.

```php
protected $fillable = [
    'title',
    'content',
    'category_id',
    // altri attributi
];
```

#### Modifica del Valore della Categoria

##### Controller: Funzione `edit`

Nel metodo `edit` del controller, passa tutte le categorie alla vista.

```php
public function edit(Post $post)
{
    $categories = Category::all();
    return view('posts.edit', compact('post', 'categories'));
}
```

##### Blade: Form di modifica (edit.blade.php)

Usa il seguente codice per il form di selezione della categoria, con il valore pre-selezionato.

```html
<label for="category_id" class="form-label">Categoria</label>
<select class="form-select" name="category_id" id="category_id">
    <option value=""></option>
    @foreach($categories as $category)
        <option @selected($post->category?->id == $category->id) value="{{ $category->id }}">{{ $category->name }}</option>
    @endforeach
</select>
```

##### Validazione della Request

Nella request, aggiungi la validazione per l'attributo `category_id`.

```php
public function rules()
{
    return [
        'category_id' => ['nullable'],
        // altre regole di validazione
    ];
}
```

### Riepilogo

1. **Mostrare la Categoria**: Usa il null safe operator `?` per mostrare la categoria nell'index.
2. **Creazione del Post**:
    - Passa le categorie alla vista nel metodo `create` del controller.
    - Aggiungi un form di selezione della categoria nel file `create.blade.php`.
    - Aggiungi la validazione per `category_id` nella request.
    - Aggiungi `category_id` al `$fillable` nel modello.
3. **Modifica del Post**:
    - Passa le categorie alla vista nel metodo `edit` del controller.
    - Aggiungi un form di selezione della categoria nel file `edit.blade.php` con il valore pre-selezionato.
    - Aggiungi la validazione per `category_id` nella request.