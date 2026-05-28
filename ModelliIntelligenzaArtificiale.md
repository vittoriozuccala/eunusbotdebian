# Modelli di Intelligenza artificiale
Il sistema [OpenClaw](https://openclaw.ai/) che abbiamo introdotto nel [README](README.md), ha il fulcro di tutte le attività che risiede nel modello di intelligenza artificiale.

[OpenClaw](https://openclaw.ai/) da questo punto di vista è agnostico e si può connettere a qualsiasi modello, sia esso in cloud o in  locale.
Ci sono, infatti, diverse società ed enti che rilasciano i propri modelli in forma proprietaria o opensource.
Tali modelli posso girare in alcuni casi sulla macchina in cui risiede l'agente IA oppure nei server della società proprietaria.
Ovviamente per i modelli sulla propria macchina, è necessario avere un sistema ben carrozzato con almeno 32Gb di RAM altrimenti si rischierebbe di non riuscire a ottenere
alcun risultato e bloccare le elaborazioni.

Ogni modello IA viene allenato per compiti specifici, dalla capacità di creare programmi o script (coding), all'abilità di generare immagini o video, a quella di gestire 
progetti anche complessi.
Per misurare la capacità interpretativa e di esecuzione di un modello si parla di **numero di parametri** con il quale lo stesso è stato allenato, solitamente in milioni.


Un altro fattore importante sono i **token** in ingresso ed in uscita.
I token sono i pezzi di frase (solitamente un token corrisponde ad una parola o poco meno) che vengono inviate al modello sotto forma di *contesto* e ritornano dal modello 
sotto forma di *risposta*.
Ovviamente tanto è più alto il numero di parametri con il quale è stato allenato, tanto maggiore sarà il numero di token in input ed output che potrà gestire.

Per la connessione ad un modello, [OpenClaw](https://openclaw.ai/) necessita di una **chiave API** che è fornita dal provider e implica un costo in euro/dollari 
solitamente separato ad eventuali abbonamenti che si possono avere con la società stessa.

In sintesi, un numero elevato di parametri significa:
- Maggiore profondità di contesto che è possibile inviare e risposte più accurate
- Maggiore profondità di rilfessione 
- Maggiore numero di token richiesti in uscita
- Minore velocità di risposta
- Maggiore costo per l'utente

Se ne conclude che per compiti semplici e routinari è meglio utilizzare un modello piccolo che ragiona velocemente, non necessità di capacità di ragionamento 
troppo fini e soprattutto che è accessibile con un esborso relativamente basso. 
Al contrario per compiti più delicati e nei quali è necessario uno sforzo di ragionamento aggiuntivo, meglio utilizzare modelli più complessi


## Alcuni modelli
Come abbiamo detto i modelli possono essere in cloud oppure installati localmente sfruttando l'applicativo [Ollama](https://ollama.com/).
Non entrerò nei dettagli di questo secondo caso per i motivi sopra citati ma mi limiterò a fare alcuni esempi di modelli:
- Modelli proprietari:
	- Anthropic (la società che ad oggi sembra essere leader in questo settore):
		- Opus (modello estremamente performante e dispendioso)
		- Sonnet (una buona via di mezzo)
		- Haiku (modello semplice ed economico e rapido)
	- ChatGPT:
		- gpt pro (modello top di gamma soprattutto per il coding)
		- gpt 5 mini (modello minimalista)
	- Google
		- Gemini Pro (modello performante e dispendioso)
		- Nano banana (modello ottimo per generare immagini e video brevi)
	- DeepSeek (cinese)
		- deep-seek reasoner
