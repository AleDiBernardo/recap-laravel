[README](../README.md)<br>
### **Precedente:**
[CONTROLLER](CONTROLLER.md)

---

### Operazioni CRUD in un Controller Resource di Laravel

In Laravel, i controller di tipo `resource` forniscono una struttura standard per gestire le operazioni CRUD. Un controller `resource` è in grado di gestire tutte le operazioni CRUD con metodi specifici predefiniti. Di seguito viene illustrato come funziona ogni metodo.

#### Generazione di un Controller Resource

Per generare un controller resource, utilizza il comando Artisan:

```bash
php artisan make:controller NomeModelController --resource
```

Questo comando crea un controller chiamato `UserController` con metodi per gestire le operazioni CRUD.

#### Metodi del Controller Resource

1. **index()** - Mostra una lista di risorse.

    ```php
    public function index()
    {
        $users = User::all();
        return view('users.index', compact('users'));
    }
    ```

2. **create()** - Mostra il form per creare una nuova risorsa.

    ```php
    public function create()
    {
        return view('users.create');
    }
    ```

3. **store()** - Salva una nuova risorsa nel database.

    ```php
    public function store(Request $request)
    {
        //esempio di validazione dei dati
        $validatedData = $request->validate([
            'name' => 'required|max:255',
            'email' => 'required|email|unique:users',
            'password' => 'required|min:6',
        ]);

        //algoritmo di hashing che crypta e aggiunge un 'salt'
        $validatedData['password'] = bcrypt($validatedData['password']);
        User::create($validatedData);

        //con width restituiamo un messaggio di successo
        return redirect()->route('users.index')->with('success', 'User created successfully.');
    }
    ```

4. **show($id)** - Mostra una specifica risorsa.

    ```php
    public function show($id)
    {
        $user = User::findOrFail($id);
        return view('users.show', compact('user'));
    }
    ```

    **Dependency Injection** syntax: 
    ```php
    public function show(User $user) //attua comunque il findOrFail
    {
        return view('users.show', compact('user'));
    }
    ```

5. **edit($id)** - Mostra il form per modificare una risorsa esistente.

    ```php
    public function edit($id)
    {
        $user = User::findOrFail($id);
        return view('users.edit', compact('user'));
    }
    ```

    **Dependency Injection** syntax: 
    ```php
    public function edit(User $user)
    {
        return view('users.edit', compact('user'));
    }
    ```

6. **update(Request $request, $id)** - Aggiorna una risorsa esistente nel database.

    ```php
    public function update(Request $request, User $user)
    {
        // Validazione dei dati
        $validatedData = $request->validate([
            'name' => 'required|max:255',
            'email' => 'required|email|unique:users,email,' . $user->id,
            'password' => 'nullable|min:6',
        ]);

        // Validazione della password
        if ($request->filled('password')) {
            $validatedData['password'] = bcrypt($validatedData['password']);
        } else {
            unset($validatedData['password']);
        }

        // Aggiornamento dell'utente nel database
        $user->update($validatedData);

        return redirect()->route('users.index')->with('success', 'User updated successfully.');
    }

    ```

7. **destroy()** - Elimina una risorsa esistente dal database.

    ```php
    public function destroy(User $user)
    {
        $user->delete();

        return redirect()->route('users.index')->with('success', 'User deleted successfully.');
    }
    ```

### Rotte per un Controller Resource

Per collegare il controller alle rotte, utilizza la funzione `Route::resource` nel file `routes/web.php`:

```php
Route::resource('users', UserController::class);
```

### Riassunto dei Metodi

- **index()**: Recupera e mostra tutte le risorse.
- **create()**: Mostra il form per creare una nuova risorsa.
- **store()**: Salva una nuova risorsa nel database.
- **show()**: Mostra una singola risorsa.
- **edit()**: Mostra il form per modificare una risorsa esistente.
- **update()**: Aggiorna una risorsa esistente nel database.
- **destroy()**: Elimina una risorsa esistente dal database.

Questi metodi coprono le operazioni CRUD e sono standardizzati per essere facilmente utilizzati e mantenuti nelle applicazioni Laravel.

### **Successivo:**
[ALTRI PASSAGGI](OTHER.md)
