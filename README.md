# BookStore API

REST API per la gestione inventario di una libreria, costruita con Node.js ed Express 5.

## Stack

- **Runtime:** Node.js
- **Framework:** Express 5
- **Database:** SQLite (via better-sqlite3)
- **Testing:** Jest + Supertest
- **Dev tools:** npm / lint / test

## Setup

### Prerequisiti

- Node.js >= 18
- npm

### Installazione

```bash
npm install
```

### Variabili d’ambiente

Crea un file `.env` nella root del progetto. Esempio:

```env
TEST_API_KEY=dev-key-1234
```

> Per eseguire i test, assicurati che `TEST_API_KEY` sia impostato oppure usa il valore di fallback.

### Avvio

```bash
npm run dev
```

L’API è disponibile tipicamente su `http://localhost:3000`.

## Architettura (overview)

La struttura del progetto prevede un livello server/route, un layer di validazione e servizi, e un livello di accesso al database (SQLite). Le tabelle principali sono:

- **authors**
- **books**
- **book_authors** (relazione many-to-many)

## Documentazione API

> Nota: questa repository include sia endpoint “books” sia endpoint “authors”.

### Auth

Gli endpoint richiedono header:

- `X-API-Key: <API_KEY>`

Esempio (curl):

```bash
curl -X GET http://localhost:3000/api/v1/authors \
  -H "X-API-Key: dev-key-1234"
```

### Errors

Risposte di errore strutturate del tipo:

```json
{
  "error": {
    "code": "AUTHOR_NOT_FOUND",
    "message": "..."
  }
}
```

## Authors

### `POST /api/v1/authors`

Crea un nuovo autore.

Payload (esempio):

```json
{
  "first_name": "Umberto",
  "last_name": "Eco",
  "nationality": "Italian",
  "biography": "Italian novelist and philosopher."
}
```

### `GET /api/v1/authors`

Lista autori paginata.

Query params:

- `page` (default `1`)
- `limit` (default `20`)
- `search` (opzionale: ricerca per `first_name` / `last_name`)

### `GET /api/v1/authors/:id`

Recupera il dettaglio autore.

### `PATCH /api/v1/authors/:id`

Aggiornamento parziale dell’autore.

### `DELETE /api/v1/authors/:id`

Elimina un autore.

- `204` se l’autore non ha libri
- `409` se l’autore ha libri (`AUTHOR_HAS_BOOKS`)

## Testing

### Esempi comandi

```bash
npm test
```

I test di integrazione usano **Jest** e **Supertest** e puliscono le tabelle tra i casi.

---

> Questo README è stato aggiornato per migliorare chiarezza e usabilità della documentazione.
