[README](../README.md)<br>

### **Precedente:**
[PRENDERE DATI DA UN DATABASE](GET_DB_DATA.md)

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

### **Successivo:**
[SEEDER](SEEDER.md)