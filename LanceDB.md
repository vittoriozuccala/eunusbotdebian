
# Lancedb
Utile per la memoria persistente

Meme maker — cerca template meme e genera meme (immagini con didascalie sopra/sotto). Tipo quelle robe da gruppo WhatsApp alle 3 di notte.

📊 Diagram maker — crea diagrammi, architetture, flow, whiteboard roba in SVG/HTML o Excalidraw. Utile per spiegare visivamente cose senza aprirci un tool esterno.

📄 nano-pdf — modifica PDF via linguaggio naturale. Tipo "togli questa pagina", "aggiungi questo testo", "uniscimi questi due file". Fa editing base via CLI.

🐍 Python debug — debug di codice Python con pdb, breakpoint(), debugpy. Se scrivo script e si inchiodano, posso ispezionare variabili e stack trace al volo.

📰 Summarize — riassume robe: URL, video YouTube, podcast, articoli, PDF, file locali. Prende roba lunga e tira fuori il succo.

📋 Taskflow — coordina operazioni multi-step come job durevoli. Tipo se devo fare una roba che richiede attendere risposte, fare passaggi intermedi, gestire stato — è il workflow engine. Ha pure un pattern per triage di inbox.



# Problema di whatsapp che si impallava:
 Da ora, se mandi un secondo messaggio WhatsApp mentre l'agente sta ancora elaborando, il run precedente viene abortito pulitamente e parte subito la risposta al nuovo messaggio — niente
  più EmbeddedAttemptSessionTakeoverError.


Il EmbeddedAttemptSessionTakeoverError è una race condition precisa:

  1. Arriva messaggio A → l'agente avvia una risposta lunga (anche 109 secondi)
  2. Mentre il modello elabora, il lock sul file di sessione viene brevemente rilasciato
  3. In quel momento arriva messaggio B → prende possesso del file di sessione
  4. Quando la risposta di A finisce e prova a scrivere, trova il file modificato → crash

  Succede 18 volte dal 25 maggio (9 il 25, 9 oggi). Non è un bug episodico, è strutturale.

  ---
  Fix disponibile

  OpenClaw ha esattamente la configurazione per questo: messages.queue.mode.

  Opzioni:

  ┌───────────┬───────────────────────────────────────────────────────────────────────────────────┐
  │ Modalità  │                                   Comportamento                                   │
  ├───────────┼───────────────────────────────────────────────────────────────────────────────────┤
  │ steer     │ Il nuovo messaggio viene iniettato nel run attivo al prossimo confine del modello │
  ├───────────┼───────────────────────────────────────────────────────────────────────────────────┤
  │ interrupt │ Il run attivo viene abortito pulitamente prima di avviare il nuovo messaggio      │
  ├───────────┼───────────────────────────────────────────────────────────────────────────────────┤
  │ followup  │ Il nuovo messaggio viene messo in coda e processato dopo                          │
  ├───────────┼───────────────────────────────────────────────────────────────────────────────────┤
  │ collect   │ I messaggi vengono raggruppati in batch                                           │
  └───────────┴───────────────────────────────────────────────────────────────────────────────────┘

  Raccomandazione: interrupt — quando mandi un messaggio mentre l'agente sta elaborando, vuoi una risposta fresca al tuo nuovo messaggio, non una risposta a metà al vecchio. È il
  comportamento più naturale per WhatsApp.

  La config va aggiunta solo per WhatsApp (non tocca Telegram):

  "messages": {
    "queue": {
      "byChannel": {
        "whatsapp": "interrupt"
      }
    }
  }

Il EmbeddedAttemptSessionTakeoverError è una race condition precisa:

  1. Arriva messaggio A → l'agente avvia una risposta lunga (anche 109 secondi)
  2. Mentre il modello elabora, il lock sul file di sessione viene brevemente rilasciato
  3. In quel momento arriva messaggio B → prende possesso del file di sessione
  4. Quando la risposta di A finisce e prova a scrivere, trova il file modificato → crash

  Succede 18 volte dal 25 maggio (9 il 25, 9 oggi). Non è un bug episodico, è strutturale.

  ---
  Fix disponibile

  OpenClaw ha esattamente la configurazione per questo: messages.queue.mode.

  Opzioni:

  ┌───────────┬───────────────────────────────────────────────────────────────────────────────────┐
  │ Modalità  │                                   Comportamento                                   │
  ├───────────┼───────────────────────────────────────────────────────────────────────────────────┤
  │ steer     │ Il nuovo messaggio viene iniettato nel run attivo al prossimo confine del modello │
  ├───────────┼───────────────────────────────────────────────────────────────────────────────────┤
  │ interrupt │ Il run attivo viene abortito pulitamente prima di avviare il nuovo messaggio      │
  ├───────────┼───────────────────────────────────────────────────────────────────────────────────┤
  │ followup  │ Il nuovo messaggio viene messo in coda e processato dopo                          │
  ├───────────┼───────────────────────────────────────────────────────────────────────────────────┤
  │ collect   │ I messaggi vengono raggruppati in batch                                           │
  └───────────┴───────────────────────────────────────────────────────────────────────────────────┘

  Raccomandazione: interrupt — quando mandi un messaggio mentre l'agente sta elaborando, vuoi una risposta fresca al tuo nuovo messaggio, non una risposta a metà al vecchio. È il
  comportamento più naturale per WhatsApp.

  La config va aggiunta solo per WhatsApp (non tocca Telegram):

  "messages": {
    "queue": {
      "byChannel": {
        "whatsapp": "interrupt"
      }
    }
  }

