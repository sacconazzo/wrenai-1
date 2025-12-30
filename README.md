# WrenAI - Guida di Avvio

Questo repository contiene la configurazione per avviare WrenAI utilizzando Docker Compose.

## Prerequisiti

Prima di iniziare, assicurati di avere installato:

- **Docker** (versione 20.10 o superiore)
- **Docker Compose** (versione 1.29 o superiore)
- Una chiave API di Google Gemini (necessaria per i modelli LLM ed embedding)

## Configurazione

### 1. Configurare le variabili d'ambiente

Copia il file `.env.example` in un nuovo file chiamato `.env`:

```bash
cp .env.example .env
```

Modifica il file `.env` e configura almeno i seguenti parametri obbligatori:

```env
# Chiave API di Google Gemini (OBBLIGATORIA)
GEMINI_API_KEY=la_tua_chiave_api_gemini

# Opzionale: Modifica le porte se hai conflitti
HOST_PORT=3000
AI_SERVICE_FORWARD_PORT=5555
```

### 2. Configurare il file config.yaml

Il file `config.yaml` contiene già una configurazione predefinita che utilizza:
- **LLM**: `gemini/gemini-2.0-flash`
- **Embedder**: `gemini/text-embedding-004`

Assicurati che il campo `embedding_model_dim` sia impostato correttamente:

```yaml
type: document_store
provider: qdrant
location: http://qdrant:6333
embedding_model_dim: 768  # Dimensione del modello di embedding
```

## Comandi di Avvio

### Avviare tutti i servizi

Per avviare l'intero stack WrenAI, esegui:

```bash
docker-compose up
```

Oppure, per avviare i servizi in background (modalità detached):

```bash
docker-compose up -d
```

### Primo avvio

Al primo avvio, Docker scaricherà tutte le immagini necessarie. Questo processo può richiedere alcuni minuti.

I servizi che verranno avviati sono:
- **bootstrap**: Inizializza i volumi e i dati necessari
- **wren-engine**: Motore di elaborazione delle query
- **ibis-server**: Server per l'elaborazione dei dati
- **qdrant**: Database vettoriale per l'embedding dei documenti
- **wren-ai-service**: Servizio AI principale
- **wren-ui**: Interfaccia utente web

## Verificare lo Stato dei Servizi

### Controllare i log

Per visualizzare i log di tutti i servizi:

```bash
docker-compose logs -f
```

Per visualizzare i log di un servizio specifico:

```bash
docker-compose logs -f wren-ui
docker-compose logs -f wren-ai-service
```

### Controllare lo stato dei container

```bash
docker-compose ps
```

Tutti i servizi dovrebbero essere nello stato "Up".

## Accesso all'Interfaccia Utente

Una volta che tutti i servizi sono avviati correttamente, puoi accedere all'interfaccia utente di WrenAI:

```
http://localhost:3000
```

Se hai modificato `HOST_PORT` nel file `.env`, usa la porta configurata.

## Porte Esposte

Le seguenti porte sono esposte sull'host:

- **3000**: Interfaccia utente WrenAI (configurabile tramite `HOST_PORT`)
- **5555**: Servizio AI (configurabile tramite `AI_SERVICE_FORWARD_PORT`)

## Comandi Utili

### Fermare tutti i servizi

```bash
docker-compose down
```

### Fermare e rimuovere anche i volumi

```bash
docker-compose down -v
```

### Riavviare un servizio specifico

```bash
docker-compose restart wren-ui
```

### Ricostruire le immagini e riavviare

```bash
docker-compose up --build
```

### Visualizzare i container in esecuzione

```bash
docker-compose ps
```

## Risoluzione dei Problemi

### I servizi non si avviano

1. Verifica che Docker e Docker Compose siano installati e in esecuzione
2. Controlla che la chiave API `GEMINI_API_KEY` sia configurata correttamente nel file `.env`
3. Verifica i log per messaggi di errore: `docker-compose logs`

### Conflitti di porte

Se ricevi errori relativi a porte già in uso, modifica le variabili `HOST_PORT` o `AI_SERVICE_FORWARD_PORT` nel file `.env`.

### Problemi di memoria

WrenAI richiede risorse significative. Assicurati che Docker abbia accesso a:
- Almeno 4GB di RAM
- Almeno 10GB di spazio su disco

## Struttura del Progetto

```
.
├── docker-compose.yaml     # Configurazione Docker Compose
├── .env.example            # Esempio di file di configurazione
├── config.yaml             # Configurazione dei modelli AI e pipeline
├── google-ai-studio.yaml   # Configurazione alternativa per Google AI Studio
├── data/                   # Directory per i dati persistenti (creata automaticamente)
└── README.md               # Questo file
```

## Versioni dei Componenti

Le versioni predefinite configurate in `.env.example`:

- **WREN_PRODUCT_VERSION**: 0.29.1
- **WREN_ENGINE_VERSION**: 0.22.0
- **WREN_AI_SERVICE_VERSION**: 0.29.0
- **WREN_UI_VERSION**: 0.32.2
- **IBIS_SERVER_VERSION**: 0.22.0
- **WREN_BOOTSTRAP_VERSION**: 0.1.5

## Supporto e Documentazione

Per ulteriori informazioni su WrenAI, consulta:
- [Documentazione ufficiale WrenAI](https://github.com/canner/WrenAI)
- [Repository WrenAI su GitHub](https://github.com/canner/WrenAI)

## Note Importanti

- **Telemetria**: Per impostazione predefinita, la telemetria è abilitata. Per disabilitarla, imposta `TELEMETRY_ENABLED=false` nel file `.env`.
- **Chiavi API**: Non committare mai il file `.env` con le tue chiavi API nel repository.
- **Primo avvio**: Il bootstrap iniziale può richiedere alcuni minuti per configurare il database e i volumi.
