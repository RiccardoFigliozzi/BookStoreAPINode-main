# BookStore API

REST API per la gestione inventario di una libreria, costruita con Node.js ed Express 5.

## Stack

- **Runtime:** Node.js
- **Framework:** Express 5
- **Database:** SQLite (via better-sqlite3)
- **Testing:** Jest + Supertest
- **Dev tools:** npm run lint / test

## Setup

### Prerequisiti

- Node.js >= 18

### Installazione

```bash
npm install
```

## Variabili d'ambiente

Crea un file `.env` nella root del progetto.

Esempio:

```bash
env
TEST_API_KEY=dev-key-1234
```

> Per eseguire i test, assicurati di avere `TEST_API_KEY` impostato oppure usa il valore di default riportato.

## Avvio

```bash
npm run dev
```

Output di esempio:

```bash
✓ API listening on http://localhost:3000/api/v1/authors
```

## Autenticazione

Gli endpoint richiedono un header:

- `X-API-Key: <API_KEY>`

Esempio (curl):

```bash
curl -X GET http://localhost:3000/api/v1/authors \
  -H "X-API-Key: dev-key-1234"
```

## Errori

Risposta di errore strutturata nel body:

```json
{
  "error": {
    "code": "AUTHOR_NOT_FOUND",
    "message": "..."
  }
}
```

## Autori

### POST /api/v1/authors

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

#### Validazioni

- `400 VALIDATION_ERROR` se mancano campi richiesti (es. `first_name`, `last_name`).

### GET /api/v1/authors

Lista autori paginata.

Query params:

- `page` (default `1`)
- `limit` (default `20`)
- `search` (opzionale; filtra per `first_name` / `last_name`)

### GET /api/v1/authors/:id

Recupera il dettaglio di un autore.

### PATCH /api/v1/authors/:id

Aggiornamento parziale di un autore.

### DELETE /api/v1/authors/:id

Elimina un autore.

- `204` se l’autore non ha libri
- `409 AUTHOR_HAS_BOOKS` se l’autore è collegato ad almeno un libro

## Testing

### Esempio comandi

```bash
npm test
```

> Nota: i file test usano header dei commenti per rendere più chiaro il routing e l’uso della key nelle richieste.
