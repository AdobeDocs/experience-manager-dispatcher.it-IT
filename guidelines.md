---
source-git-commit: 2d90738d01fef6e37a2c25784ed4d1338c037c23
workflow-type: ht
source-wordcount: '715'
ht-degree: 100%

---
# Linee guida per contribuire alla documentazione di Adobe Experience Manager

## Filosofia della documentazione

Gli utenti di Adobe Experience Manager lavorano in ambienti altamente competitivi per creare esperienze digitali che li distinguano dalla concorrenza. Pertanto, quando Adobe offre nuovi strumenti avanzati in AEM, tali strumenti vengono integrati da una documentazione accurata e chiara che consente ai clienti di utilizzare immediatamente il proprio investimento in AEM e massimizzare il ritorno sull’investimento.

Per la documentazione di AEM, l’obiettivo è renderla disponibile agli utenti della soluzione il prima possibile. Viene quindi data priorità a una documentazione accurata e fruibile, sottoposta ad aggiornamento e miglioramento continuo.

## Contributi alla documentazione

Al fine di migliorare continuamente la documentazione di AEM, apprezziamo il contributo dell’intera comunità di utenti AEM. I miglioramenti apportati alla documentazione, che derivino da richieste pull o da problemi, possono essere correzioni, chiarimenti, ampliamenti ed esempi aggiuntivi.

## Standard della documentazione

Pur accogliendo con favore i contributi alla documentazione, qualsiasi contributo alla documentazione di AEM, sotto forma di richiesta pull o problema, deve essere conforme agli standard di contributi e di documentazione di Adobe.

I contributi che non soddisfano tali standard possono essere rifiutati.

### Adobe documenta i casi d’uso standard.

La documentazione di AEM riguarda i casi d’uso standard. I casi di utilizzo che esulano dall’ambito di installazione e utilizzo standard del prodotto non rientrano nella documentazione di AEM.

### In genere Adobe non documenta i bug o le relative soluzioni.

La documentazione di AEM riguarda i casi d’uso standard. Per questo motivo, i bug, gli effetti causati dai bug e le soluzioni per i bug non sono generalmente documentati.

Fanne eccezione le note sulla versione, in cui possono essere elencati i problemi noti con possibili soluzioni approvate dal team AEM Product Management.

### I contributi alla documentazione non devono essere usati per rispondere a domande tecniche.

È gradita come contributo qualsiasi idea che punti al miglioramento della documentazione di AEM. Tuttavia, commenti, problemi e richieste pull sono intesi solo come *contributi*. Non hanno l’obiettivo di rispondere a domande su come usare AEM, su come implementare un progetto AEM o risolvere problemi tecnici.

È possibile segnalare qualsiasi domanda relativa all’utilizzo di AEM o ad errori tecnici utilizzando il normale processo di assistenza tramite il [portale di supporto di Experience Cloud Enterprise](https://experienceleague.adobe.com/i?support-solution=General#support) o discutendone nella [community di Experience Manager](https://experienceleaguecommunities.adobe.com/t5/adobe-experience-manager/ct-p/adobe-experience-manager-community?lang=it).

***I contributi alla documentazione di AEM non sostituiscono l’Assistenza clienti di Adobe***. Pertanto, verranno rifiutati tutti i contributi che richiedono risposte a domande correlate al supporto.

### I contributi devono fare riferimento in modo chiaro alle pagine della documentazione interessate.

Se apri una segnalazione per suggerire miglioramenti alla documentazione, devi includere i collegamenti alle pagine interessate. Se apri una segnalazione di un problema utilizzando il collegamento **Modifica questa pagina** di una pagina della documentazione, il problema verrà creato automaticamente con un collegamento alla pagina in questione.

Ciò non si applica alle richieste pull, che per loro natura fanno riferimento alle pagine interessate.

## Linee guida per la documentazione

Adobe chiede che qualsiasi contributo alla documentazione segua determinate linee guida di stile.

L’adesione a queste linee guida semplifica la revisione del contributo proposto e ne velocizza quindi l’integrazione nella documentazione di Adobe.

### Lingua e stile

#### Lingua

* La documentazione di AEM è creata e mantenuta in inglese americano.
* Usa frasi quanto più semplici possibile.
* Usa un linguaggio chiaro e conciso.

Ricorda che la documentazione di AEM viene letta in tutto il mondo da persone che potrebbero non essere di madrelingua inglese o non parlare inglese correntemente. Evita le espressioni colloquiali e usa un linguaggio chiaro e semplice.

#### Segui il manuale di stile di Microsoft®

[Il manuale di stile di Microsoft®](https://learn.microsoft.com/it-it/style-guide/welcome/) è una guida di stile per la documentazione disponibile gratuitamente, specifica per la documentazione di software. La documentazione di AEM segue questa guida quando possibile.

### Formattazione

| Elemento | Stile |
|---|---|
| Elemento o opzione dell’interfaccia utente | **grassetto** |
| Nome file, percorso, input utente, valori di parametri | `monospaced` |
| Codice, riga di comando | ```Code Block``` |

### Schermate

Le schermate vanno utilizzate in modo non eccessivo e solo quando una descrizione testuale è insufficiente.

Nelle schermate non devono essere utilizzati marcatori o altre annotazioni (come riquadri rossi, frecce o testo). In questo modo le schermate possono essere riutilizzate o replicate più facilmente nelle versioni localizzate della documentazione.

### Riferimenti a specifiche versioni

È meglio evitare, quando possibile, riferimenti diretti a una versione specifica nel contenuto della documentazione. In tal modo la documentazione sarà più flessibile e potrà essere estesa anche a versioni future.

### Utilizzo di Day, AEM, CQ, CRX

Fai sempre riferimento al prodotto con il nome completo **Adobe Experience Manager** la prima volta che viene citato in un articolo; successivamente può essere indicato come **AEM**.

Day, Day Software, CQ e CRX non devono essere utilizzati a meno che non sia inevitabile, ad esempio nei nomi di classi o in riferimenti specifici a versioni storiche di AEM.