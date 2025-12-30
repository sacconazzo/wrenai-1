# Guida Rapida di Avvio - WrenAI

## Comandi Essenziali per Avviare WrenAI

### Passo 1: Configurazione Iniziale

```bash
# Copia il file di configurazione
cp .env.example .env

# Modifica il file .env e aggiungi la tua chiave API di Google Gemini
# GEMINI_API_KEY=la_tua_chiave_api_qui
```

### Passo 2: Avvio dei Servizi

```bash
# Avvia tutti i servizi in background
docker-compose up -d
```

### Passo 3: Verifica dello Stato

```bash
# Controlla che tutti i servizi siano attivi
docker-compose ps

# Visualizza i log in tempo reale
docker-compose logs -f
```

### Passo 4: Accesso

Apri il browser e vai a:

```
http://localhost:3000
```

---

## Comandi Comuni

### Fermare i servizi
```bash
docker-compose down
```

### Riavviare i servizi
```bash
docker-compose restart
```

### Visualizzare i log
```bash
# Tutti i servizi
docker-compose logs -f

# Un servizio specifico
docker-compose logs -f wren-ui
```

### Aggiornare le configurazioni
```bash
# Dopo aver modificato .env o config.yaml
docker-compose down
docker-compose up -d
```

---

## Risoluzione Rapida dei Problemi

**Errore: Porta già in uso**
- Modifica `HOST_PORT` nel file `.env`

**Errore: Chiave API mancante**
- Assicurati di aver configurato `GEMINI_API_KEY` nel file `.env`

**I servizi non si avviano**
- Controlla i log: `docker-compose logs`
- Verifica che Docker sia in esecuzione
- Verifica di avere almeno 4GB di RAM disponibili per Docker

---

Per la documentazione completa, consulta il file [README.md](README.md).
