<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "bc249f8b228953fafca05f94bb572aac",
  "translation_date": "2025-06-02T18:58:21+00:00",
  "source_file": "05-AdvancedTopics/web-search-mcp/README.md",
  "language_code": "it"
}
-->
# Lezione: Costruire un Web Search MCP Server

Questo capitolo mostra come costruire un agente AI reale che si integra con API esterne, gestisce tipi di dati diversi, gestisce gli errori e coordina più strumenti—tutto in un formato pronto per la produzione. Vedrai:

- **Integrazione con API esterne che richiedono autenticazione**
- **Gestione di tipi di dati diversi da più endpoint**
- **Strategie robuste di gestione degli errori e logging**
- **Orchestrazione multi-strumento in un singolo server**

Alla fine, avrai esperienza pratica con pattern e best practice essenziali per applicazioni avanzate AI e basate su LLM.

## Introduzione

In questa lezione imparerai come costruire un avanzato MCP server e client che estende le capacità di LLM con dati web in tempo reale usando SerpAPI. Questa è una competenza fondamentale per sviluppare agenti AI dinamici che possono accedere a informazioni aggiornate dal web.

## Obiettivi di Apprendimento

Al termine di questa lezione, sarai in grado di:

- Integrare API esterne (come SerpAPI) in modo sicuro in un MCP server
- Implementare più strumenti per ricerca web, notizie, prodotti e Q&A
- Analizzare e formattare dati strutturati per il consumo da parte di LLM
- Gestire errori e limiti di chiamate API in modo efficace
- Costruire e testare sia client MCP automatizzati che interattivi

## Web Search MCP Server

Questa sezione introduce l’architettura e le funzionalità del Web Search MCP Server. Vedrai come FastMCP e SerpAPI vengono usati insieme per estendere le capacità LLM con dati web in tempo reale.

### Panoramica

Questa implementazione include quattro strumenti che mostrano la capacità di MCP di gestire compiti diversi basati su API esterne in modo sicuro ed efficiente:

- **general_search**: per risultati web generali
- **news_search**: per le ultime notizie
- **product_search**: per dati di e-commerce
- **qna**: per risposte a domande

### Caratteristiche
- **Esempi di Codice**: Include blocchi di codice specifici per Python (e facilmente estendibili ad altri linguaggi) con sezioni a scomparsa per chiarezza

<details>  
<summary>Python</summary>  

```python
# Example usage of the general_search tool
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def run_search():
    server_params = StdioServerParameters(
        command="python",
        args=["server.py"],
    )
    async with stdio_client(server_params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            await session.initialize()
            result = await session.call_tool("general_search", arguments={"query": "open source LLMs"})
            print(result)
```
</details>

Prima di eseguire il client, è utile capire cosa fa il server. Il file [`server.py`](../../../../05-AdvancedTopics/web-search-mcp/server.py) file implements the MCP server, exposing tools for web, news, product search, and Q&A by integrating with SerpAPI. It handles incoming requests, manages API calls, parses responses, and returns structured results to the client.

You can review the full implementation in [`server.py`](../../../../05-AdvancedTopics/web-search-mcp/server.py).

Ecco un breve esempio di come il server definisce e registra uno strumento:

<details>  
<summary>Python Server</summary> 

```python
# server.py (excerpt)
from mcp.server import MCPServer, Tool

async def general_search(query: str):
    # ...implementation...

server = MCPServer()
server.add_tool(Tool("general_search", general_search))

if __name__ == "__main__":
    server.run()
```
</details>

- **Integrazione API Esterne**: Mostra come gestire in modo sicuro chiavi API e richieste esterne
- **Parsing Dati Strutturati**: Spiega come trasformare le risposte API in formati adatti per LLM
- **Gestione Errori**: Gestione robusta degli errori con logging appropriato
- **Client Interattivo**: Include test automatizzati e una modalità interattiva per il testing
- **Gestione del Contesto**: Usa MCP Context per logging e tracciamento delle richieste

## Prerequisiti

Prima di iniziare, assicurati che il tuo ambiente sia configurato correttamente seguendo questi passaggi. Questo garantirà che tutte le dipendenze siano installate e che le tue chiavi API siano configurate correttamente per uno sviluppo e testing senza intoppi.

- Python 3.8 o superiore
- Chiave API SerpAPI (registrati su [SerpAPI](https://serpapi.com/) - tier gratuito disponibile)

## Installazione

Per iniziare, segui questi passaggi per configurare l’ambiente:

1. Installa le dipendenze usando uv (consigliato) o pip:

```bash
# Using uv (recommended)
uv pip install -r requirements.txt

# Using pip
pip install -r requirements.txt
```

2. Crea un file `.env` nella root del progetto con la tua chiave SerpAPI:

```
SERPAPI_KEY=your_serpapi_key_here
```

## Uso

Il Web Search MCP Server è il componente centrale che espone strumenti per ricerca web, notizie, prodotti e Q&A integrandosi con SerpAPI. Gestisce le richieste in arrivo, le chiamate API, analizza le risposte e restituisce risultati strutturati al client.

Puoi consultare l’implementazione completa in [`server.py`](../../../../05-AdvancedTopics/web-search-mcp/server.py).

### Avviare il Server

Per avviare il MCP server, usa il seguente comando:

```bash
python server.py
```

Il server funzionerà come MCP server basato su stdio a cui il client può connettersi direttamente.

### Modalità Client

Il client (`client.py`) supports two modes for interacting with the MCP server:

- **Normal mode**: Runs automated tests that exercise all the tools and verify their responses. This is useful for quickly checking that the server and tools are working as expected.
- **Interactive mode**: Starts a menu-driven interface where you can manually select and call tools, enter custom queries, and see results in real time. This is ideal for exploring the server's capabilities and experimenting with different inputs.

You can review the full implementation in [`client.py`](../../../../05-AdvancedTopics/web-search-mcp/client.py).

### Avviare il Client

Per eseguire i test automatici (questo avvierà automaticamente il server):

```bash
python client.py
```

Oppure esegui in modalità interattiva:

```bash
python client.py --interactive
```

### Testare con Metodi Diversi

Ci sono diversi modi per testare e interagire con gli strumenti forniti dal server, a seconda delle tue esigenze e flusso di lavoro.

#### Scrivere Script di Test Personalizzati con MCP Python SDK
Puoi anche costruire i tuoi script di test usando MCP Python SDK:

<details>
<summary>Python</summary>

```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def test_custom_query():
    server_params = StdioServerParameters(
        command="python",
        args=["server.py"],
    )
    
    async with stdio_client(server_params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            await session.initialize()
            # Call tools with your custom parameters
            result = await session.call_tool("general_search", 
                                           arguments={"query": "your custom query"})
            # Process the result
```
</details>

In questo contesto, uno "script di test" è un programma Python personalizzato che scrivi per agire come client del MCP server. Invece di essere un test unitario formale, questo script ti permette di connetterti programmaticamente al server, chiamare uno qualsiasi dei suoi strumenti con parametri a scelta e ispezionare i risultati. Questo approccio è utile per:
- Prototipare e sperimentare chiamate agli strumenti
- Validare come il server risponde a input diversi
- Automatizzare invocazioni ripetute degli strumenti
- Costruire i propri workflow o integrazioni sopra MCP server

Puoi usare gli script di test per provare rapidamente nuove query, fare debug del comportamento degli strumenti, o come punto di partenza per automazioni più avanzate. Qui sotto un esempio di come usare MCP Python SDK per creare uno script simile:

## Descrizione degli Strumenti

Puoi usare i seguenti strumenti forniti dal server per eseguire diversi tipi di ricerche e query. Ogni strumento è descritto con i suoi parametri ed esempi di utilizzo.

Questa sezione fornisce dettagli su ogni strumento disponibile e i relativi parametri.

### general_search

Esegue una ricerca web generale e restituisce risultati formattati.

**Come chiamare questo strumento:**

Puoi chiamare `general_search` dal tuo script usando MCP Python SDK, o interattivamente tramite Inspector o la modalità client interattiva. Ecco un esempio di codice con SDK:

<details>
<summary>Esempio Python</summary>

```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def run_general_search():
    server_params = StdioServerParameters(
        command="python",
        args=["server.py"],
    )
    async with stdio_client(server_params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            await session.initialize()
            result = await session.call_tool("general_search", arguments={"query": "latest AI trends"})
            print(result)
```
</details>

In alternativa, in modalità interattiva, seleziona `general_search` from the menu and enter your query when prompted.

**Parameters:**
- `query` (stringa): La query di ricerca

**Esempio di richiesta:**

```json
{
  "query": "latest AI trends"
}
```

### news_search

Cerca articoli di notizie recenti correlati a una query.

**Come chiamare questo strumento:**

Puoi chiamare `news_search` dal tuo script usando MCP Python SDK, o interattivamente tramite Inspector o la modalità client interattiva. Ecco un esempio di codice con SDK:

<details>
<summary>Esempio Python</summary>

```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def run_news_search():
    server_params = StdioServerParameters(
        command="python",
        args=["server.py"],
    )
    async with stdio_client(server_params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            await session.initialize()
            result = await session.call_tool("news_search", arguments={"query": "AI policy updates"})
            print(result)
```
</details>

In alternativa, in modalità interattiva, seleziona `news_search` from the menu and enter your query when prompted.

**Parameters:**
- `query` (stringa): La query di ricerca

**Esempio di richiesta:**

```json
{
  "query": "AI policy updates"
}
```

### product_search

Cerca prodotti che corrispondono a una query.

**Come chiamare questo strumento:**

Puoi chiamare `product_search` dal tuo script usando MCP Python SDK, o interattivamente tramite Inspector o la modalità client interattiva. Ecco un esempio di codice con SDK:

<details>
<summary>Esempio Python</summary>

```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def run_product_search():
    server_params = StdioServerParameters(
        command="python",
        args=["server.py"],
    )
    async with stdio_client(server_params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            await session.initialize()
            result = await session.call_tool("product_search", arguments={"query": "best AI gadgets 2025"})
            print(result)
```
</details>

In alternativa, in modalità interattiva, seleziona `product_search` from the menu and enter your query when prompted.

**Parameters:**
- `query` (stringa): La query di ricerca prodotto

**Esempio di richiesta:**

```json
{
  "query": "best AI gadgets 2025"
}
```

### qna

Ottiene risposte dirette a domande da motori di ricerca.

**Come chiamare questo strumento:**

Puoi chiamare `qna` dal tuo script usando MCP Python SDK, o interattivamente tramite Inspector o la modalità client interattiva. Ecco un esempio di codice con SDK:

<details>
<summary>Esempio Python</summary>

```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def run_qna():
    server_params = StdioServerParameters(
        command="python",
        args=["server.py"],
    )
    async with stdio_client(server_params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            await session.initialize()
            result = await session.call_tool("qna", arguments={"question": "what is artificial intelligence"})
            print(result)
```
</details>

In alternativa, in modalità interattiva, seleziona `qna` from the menu and enter your question when prompted.

**Parameters:**
- `question` (stringa): La domanda a cui trovare una risposta

**Esempio di richiesta:**

```json
{
  "question": "what is artificial intelligence"
}
```

## Dettagli del Codice

Questa sezione fornisce snippet di codice e riferimenti per le implementazioni di server e client.

<details>
<summary>Python</summary>

Consulta [`server.py`](../../../../05-AdvancedTopics/web-search-mcp/server.py) and [`client.py`](../../../../05-AdvancedTopics/web-search-mcp/client.py) per i dettagli completi dell’implementazione.

```python
# Example snippet from server.py:
import os
import httpx
# ...existing code...
```
</details>

## Concetti Avanzati in Questa Lezione

Prima di iniziare a costruire, ecco alcuni concetti avanzati importanti che appariranno durante tutto il capitolo. Comprenderli ti aiuterà a seguire anche se non li conosci ancora:

- **Orchestrazione Multi-strumento**: significa eseguire diversi strumenti (come ricerca web, notizie, prodotti e Q&A) all’interno di un singolo MCP server. Permette al server di gestire una varietà di compiti, non solo uno.
- **Gestione dei Limiti API**: molte API esterne (come SerpAPI) limitano quante richieste puoi fare in un certo tempo. Un buon codice verifica questi limiti e li gestisce con grazia, così l’app non si blocca se superi il limite.
- **Parsing Dati Strutturati**: le risposte API sono spesso complesse e annidate. Questo concetto riguarda la trasformazione di quelle risposte in formati puliti e facili da usare, adatti per LLM o altri programmi.
- **Recupero dagli Errori**: a volte qualcosa va storto—magari la rete fallisce o l’API non restituisce ciò che ti aspetti. Il recupero dagli errori significa che il codice può gestire questi problemi e fornire feedback utili, invece di bloccarsi.
- **Validazione Parametri**: si tratta di verificare che tutti gli input agli strumenti siano corretti e sicuri da usare. Include impostare valori di default e assicurarsi che i tipi siano giusti, per evitare bug e confusione.

Questa sezione ti aiuterà a diagnosticare e risolvere problemi comuni che potresti incontrare lavorando con il Web Search MCP Server. Se incappi in errori o comportamenti inattesi, questa sezione di troubleshooting offre soluzioni ai problemi più frequenti. Consulta questi suggerimenti prima di cercare aiuto—spesso risolvono rapidamente i problemi.

## Risoluzione dei Problemi

Quando lavori con il Web Search MCP Server, potresti incontrare occasionalmente problemi—è normale quando si sviluppa con API esterne e nuovi strumenti. Questa sezione fornisce soluzioni pratiche ai problemi più comuni, così puoi tornare rapidamente operativo. Se trovi un errore, inizia da qui: i suggerimenti sotto affrontano le problematiche più frequenti e spesso risolvono il problema senza bisogno di ulteriore supporto.

### Problemi Comuni

Di seguito alcuni dei problemi più frequenti riscontrati dagli utenti, con spiegazioni chiare e passi per risolverli:

1. **SERPAPI_KEY mancante nel file .env**
   - Se vedi l’errore `SERPAPI_KEY environment variable not found`, it means your application can't find the API key needed to access SerpAPI. To fix this, create a file named `.env` in your project root (if it doesn't already exist) and add a line like `SERPAPI_KEY=your_serpapi_key_here`. Make sure to replace `your_serpapi_key_here` with your actual key from the SerpAPI website.

2. **Module not found errors**
   - Errors such as `ModuleNotFoundError: No module named 'httpx'` indicate that a required Python package is missing. This usually happens if you haven't installed all the dependencies. To resolve this, run `pip install -r requirements.txt` in your terminal to install everything your project needs.

3. **Connection issues**
   - If you get an error like `Error during client execution`, it often means the client can't connect to the server, or the server isn't running as expected. Double-check that both the client and server are compatible versions, and that `server.py` is present and running in the correct directory. Restarting both the server and client can also help.

4. **SerpAPI errors**
   - Seeing `Search API returned error status: 401` means your SerpAPI key is missing, incorrect, or expired. Go to your SerpAPI dashboard, verify your key, and update your ``, crea o aggiorna il file `.env` se necessario. Se la chiave è corretta ma vedi ancora l’errore, verifica se il tuo piano gratuito ha esaurito la quota.

### Modalità Debug

Di default, l’app registra solo le informazioni importanti. Se vuoi vedere più dettagli su cosa sta succedendo (per esempio per diagnosticare problemi difficili), puoi abilitare la modalità DEBUG. Questo ti mostrerà molto di più su ogni passo che l’app compie.

**Esempio: Output Normale**
```plaintext
2025-06-01 10:15:23,456 - __main__ - INFO - Calling general_search with params: {'query': 'open source LLMs'}
2025-06-01 10:15:24,123 - __main__ - INFO - Successfully called general_search

GENERAL_SEARCH RESULTS:
... (search results here) ...
```

**Esempio: Output DEBUG**
```plaintext
2025-06-01 10:15:23,456 - __main__ - INFO - Calling general_search with params: {'query': 'open source LLMs'}
2025-06-01 10:15:23,457 - httpx - DEBUG - HTTP Request: GET https://serpapi.com/search ...
2025-06-01 10:15:23,458 - httpx - DEBUG - HTTP Response: 200 OK ...
2025-06-01 10:15:24,123 - __main__ - INFO - Successfully called general_search

GENERAL_SEARCH RESULTS:
... (search results here) ...
```

Nota come la modalità DEBUG includa linee extra sulle richieste HTTP, risposte e altri dettagli interni. Questo può essere molto utile per il troubleshooting.

Per abilitare la modalità DEBUG, imposta il livello di logging a DEBUG all’inizio di `client.py` or `server.py`:

<details>
<summary>Python</summary>

```python
# At the top of your client.py or server.py
import logging
logging.basicConfig(
    level=logging.DEBUG,  # Change from INFO to DEBUG
    format="%(asctime)s - %(name)s - %(levelname)s - %(message)s"
)
```
</details>

---

## Cosa fare dopo

- [6. Community Contributions](../../06-CommunityContributions/README.md)

**Avvertenza**:  
Questo documento è stato tradotto utilizzando il servizio di traduzione automatica [Co-op Translator](https://github.com/Azure/co-op-translator). Pur impegnandoci per l’accuratezza, si prega di notare che le traduzioni automatiche possono contenere errori o imprecisioni. Il documento originale nella sua lingua nativa deve essere considerato la fonte autorevole. Per informazioni critiche, si raccomanda una traduzione professionale effettuata da un umano. Non siamo responsabili per eventuali malintesi o interpretazioni errate derivanti dall’uso di questa traduzione.