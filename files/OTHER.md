
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


