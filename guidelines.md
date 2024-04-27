---
source-git-commit: 2d90738d01fef6e37a2c25784ed4d1338c037c23
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 42%

---
# Linee guida per contribuire alla documentazione di Adobe Experience Manager

## Filosofia della documentazione

Gli utenti di Adobe Experience Manager lavorano in ambienti altamente competitivi per creare esperienze digitali che li distinguano dalla concorrenza. Pertanto, quando Adobe offre nuovi strumenti avanzati nell&#39;AEM, questi sono integrati da una documentazione accurata e chiara che consente ai clienti di utilizzare immediatamente il proprio investimento nell&#39;AEM e massimizzare il ritorno sull&#39;investimento.

Per la documentazione di AEM, l’obiettivo è renderla disponibile agli utenti della soluzione il prima possibile. Adobe dà quindi priorità a una documentazione accurata e fruibile e si impegna ad aggiornarla e migliorarla continuamente.

## Contributi alla documentazione

Al fine di migliorare continuamente la documentazione di AEM, apprezziamo il contributo dell’intera comunità di utenti AEM. I miglioramenti apportati alla documentazione, che derivino da richieste o segnalazioni di problemi, possono essere correzioni, chiarimenti, ampliamenti e altri esempi.

## Standard della documentazione

Mentre Adobe accoglie con favore i contributi alla sua documentazione, qualsiasi contributo alla documentazione AEM, sotto forma di richiesta o segnalazione di un problema, deve essere conforme agli standard in merito ai contributi e alla documentazione di Adobe.

I contributi che non soddisfano tali standard possono essere rifiutati.

### Adobe documenta i casi d’uso standard.

La documentazione di AEM riguarda i casi di utilizzo standard. I casi di utilizzo che esulano dall’ambito di installazione e utilizzo standard del prodotto non rientrano nella documentazione di AEM.

### In genere, l’Adobe non documenta i bug o le relative soluzioni.

La documentazione di AEM riguarda i casi di utilizzo standard. Per questo motivo, i bug, gli effetti causati da bug e le soluzioni alternative per i bug non sono documentati.

Fanne eccezione le note sulla versione, in cui possono essere elencati i problemi noti con possibili soluzioni approvate dal team AEM Product Management.

### I contributi alla documentazione non devono essere usati per rispondere a domande tecniche.

È gradita come contributo qualsiasi idea che punti al miglioramento della documentazione AEM. Tuttavia, eventuali commenti, problemi e richieste sono intesi come *contributi* solo. Non risponderanno alle tue domande su come usare l&#39;AEM, implementare il tuo progetto AEM o risolvere problemi tecnici.

È possibile segnalare qualsiasi domanda relativa all&#39;utilizzo dell&#39;AEM o ad errori tecnici utilizzando il normale processo di assistenza tramite [Portale di supporto Experience Cloud Enterprise](https://experienceleague.adobe.com/i?support-solution=General#support) o discussi in [community Experienci Manager](https://experienceleaguecommunities.adobe.com/t5/adobe-experience-manager/ct-p/adobe-experience-manager-community?lang=it).

***I contributi alla documentazione AEM non sostituiscono, ad Adobe, l’Assistenza clienti*** e tutti i contributi che richiedono risposte a domande correlate al supporto sono respinti.

### I contributi devono fare riferimento in modo chiaro alle pagine della documentazione interessate.

Se apri una segnalazione per suggerire miglioramenti alla documentazione, devi includere i collegamenti alle pagine interessate. Se si crea un problema utilizzando **Modifica questa pagina** su una pagina della documentazione, il problema viene creato automaticamente con un collegamento alla pagina.

Ciò non si applica alle richieste pull, che per loro natura fanno riferimento alle pagine interessate.

## Linee guida per la documentazione

Adobe richiede che qualsiasi contributo alla sua documentazione segua determinate linee guida di stile.

L’aderenza a queste linee guida semplifica la revisione del contributo e ne velocizza quindi l’integrazione nella documentazione di Adobe.

### Lingua e stile

#### Lingua

* La documentazione di AEM è creata e mantenuta in inglese americano.
* Usa frasi quanto più semplici possibile.
* Usa un linguaggio chiaro e conciso.

Ricorda che i lettori della documentazione dell’AEM sono di tutto il mondo e non ci si può aspettare che siano di madrelingua inglese o che parlino inglese correntemente. Evita i colloquialismi e usa un linguaggio chiaro e semplice.

#### Segui il Manuale di stile di Microsoft®

[Manuale di stile di Microsoft®](https://learn.microsoft.com/en-us/style-guide/welcome/) è una guida di stile per la documentazione disponibile gratuitamente, specifica per la documentazione di software. La documentazione AEM segue questa guida quando possibile.

### Formattazione

| Elemento | Stile |
|---|---|
| Elemento o opzione dell’interfaccia utente | **grassetto** |
| Nome file, percorso, input utente, valori di parametri | `monospaced` |
| Codice, riga di comando | ```Code Block``` |

### Schermate

Le schermate devono essere utilizzate con cautela e solo quando una descrizione testuale è insufficiente.

Nelle schermate non devono essere utilizzati marcatori o altre annotazioni (come riquadri rossi, frecce o testo). In questo modo le schermate possono essere riutilizzate o replicate più facilmente nelle versioni localizzate della documentazione.

### Riferimenti a specifiche versioni

È meglio evitare, quando possibile, riferimenti diretti a una versione specifica nel contenuto della documentazione. In tal modo la documentazione sarà più flessibile e potrà essere estesa anche a versioni future.

### Utilizzo di Day, AEM, CQ, CRX

Fai sempre riferimento al prodotto con il suo nome completo **Adobe Experience Manager** per la prima volta in un articolo e possono essere successivamente denominati **AEM**.

Day, Day Software, CQ e CRX non devono essere utilizzati a meno che non sia inevitabile, ad esempio nei nomi di classi o in riferimenti specifici a versioni storiche di AEM.