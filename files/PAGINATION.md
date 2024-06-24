[README](../README.md)<br>

---

### PAGINAZIONE

#### Controller: Funzione `index`

Sostituisci `::all()` con `::paginate(numeroElementi)` per abilitare la paginazione.

```php
public function index(Request $request) {
    $posts = Post::paginate(10); //Numero pagine di default
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

### Riepilogo

1. **Controller**: Sostituisci `::all()` con `::paginate($perPage)` nella funzione `index`.
2. **Blade**: Usa `{{ $posts->links() }}` per visualizzare la paginazione.
3. **AppServiceProvider**: Aggiungi `Paginator::useBootstrapFive();` nel metodo `boot`.
4. **Form**: Crea un form per permettere agli utenti di selezionare il numero di elementi per pagina.
5. **Paginazione**: Aggiorna la chiamata della paginazione per includere parametri aggiuntivi.