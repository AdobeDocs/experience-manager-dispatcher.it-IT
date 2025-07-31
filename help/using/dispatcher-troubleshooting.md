---
title: Risoluzione dei problemi di Dispatcher
description: Scopri come risolvere i problemi di Dispatcher.
cmgrlastmodified: 01.11.2007 08 22 29 [aheimoz]
pageversionid: 1193211344162
template: /apps/docs/templates/contentpage
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
exl-id: 29f338ab-5d25-48a4-9309-058e0cc94cff
source-git-commit: c41b4026a64f9c90318e12de5397eb4c116056d9
workflow-type: ht
source-wordcount: '472'
ht-degree: 100%

---

# Risoluzione dei problemi di Dispatcher {#troubleshooting-dispatcher-problems}

>[!NOTE]
>
>Le versioni di Dispatcher sono indipendenti da AEM. Tuttavia, la documentazione di Dispatcher è incorporata nella documentazione di AEM. Utilizza sempre la documentazione di Dispatcher incorporata nella documentazione della versione più recente di AEM.
>
>Se hai seguito un collegamento alla documentazione di Dispatcher, potresti essere stato reindirizzato a questa pagina. Tale collegamento è incorporato nella documentazione di una versione precedente di AEM.

>[!NOTE]
>
>Per ulteriori informazioni, controlla la sezione <!-- URL is 404[Dispatcher Knowledge Base](https://helpx.adobe.com/experience-manager/kb/index/dispatcher.html), -->[Risoluzione dei problemi di svuotamento di Dispatcher](https://experienceleague.adobe.com/search.html?lang=it#q=troubleshooting%20dispatcher%20flushing%20issues&sort=relevancy&f:el_product=[Experience%20Manager]) e le [Domande frequenti sui problemi principali di Dispatcher](dispatcher-faq.md).

## Controlla la configurazione di base {#check-the-basic-configuration}

Come sempre, i primi passaggi consistono nel controllare le nozioni di base:

* [Verifica il funzionamento di base](/help/using/dispatcher-configuration.md#confirming-basic-operation)
* Verifica tutti i file di registro del server web e del Dispatcher. Se necessario, aumenta il `loglevel` utilizzato per la [registrazione](/help/using/dispatcher-configuration.md#logging) del Dispatcher.

* [Verifica la configurazione](/help/using/dispatcher-configuration.md):

   * Hai più Dispatcher?

      * Hai stabilito quale Dispatcher gestisce il Sito Web/la pagina che stai esaminando?

   * Hai implementato i filtri?

      * Questi filtri stanno influenzando il problema che stai esaminando?

## Strumenti di diagnostica IIS {#iis-diagnostic-tools}

IIS fornisce vari strumenti di trace, a seconda della versione effettiva:

* IIS 6 - È possibile scaricare e configurare gli strumenti di diagnostica IIS
* IIS 7 - Il tracciamento è completamente integrato

Questi strumenti possono aiutarti a monitorare l’attività.

<!-- Both URLs in this topic 404! >
## IIS and 404 Not Found {#iis-and-not-found}

When using IIS, you might experience `404 Not Found` being returned in various scenarios. If so, see the following Knowledge Base articles.

* [IIS 6/7: POST method returns 404](https://helpx.adobe.com/experience-manager/kb/IIS6IsapiFilters.html)
* [IIS 6: Requests to URLs that contain the base path `/bin` return a `404 Not Found`](https://helpx.adobe.com/experience-manager/kb/RequestsToBinDirectoryFailInIIS6.html)

Also check that the Dispatcher cache root and the IIS document root are set to the same directory. -->

## Problemi durante l’eliminazione dei modelli di flussi di lavoro {#problems-deleting-workflow-models}

**Sintomi**

Problemi durante il tentativo di eliminare i modelli di flussi di lavoro quando si accede a un’istanza Autore AEM tramite Dispatcher.

**Passaggi da riprodurre:**

1. Accedi all’istanza di authoring (verifica che le richieste vengano instradate tramite il Dispatcher).
1. Crea un flusso di lavoro; ad esempio, con il titolo impostato su workflowToDelete.
1. Verifica che il flusso di lavoro sia stato creato correttamente.
1. Seleziona e fai clic con il pulsante destro del mouse sul flusso di lavoro, quindi fai clic su **Elimina**.

1. Fai clic su **Sì** per confermare.
1. Viene visualizzata una finestra di messaggio di errore che mostra quanto segue:\
   `ERROR 'Could not delete workflow model!!`.

**Risoluzione**

Aggiungi le seguenti intestazioni alla sezione `/clientheaders` del file `dispatcher.any`:

* `x-http-method-override`
* `x-requested-with`

```
{  
{  
/clientheaders  
{  
...  
"x-http-method-override"  
"x-requested-with"  
}
```

## Interferenza con mod_dir (Apache) {#interference-with-mod-dir-apache}

Il processo descrive come il Dispatcher interagisce con `mod_dir` all’interno del server web Apache, in quanto ciò può portare a vari effetti potenzialmente inaspettati:

### Apache 1.3 {#apache}

In Apache 1.3, `mod_dir` gestisce ogni richiesta in cui l’URL viene mappato a una directory nel file system.

Si verifica una delle seguenti alternative:

* La richiesta viene reindirizzata a un file `index.html` esistente
* Viene generato un elenco di directory

Quando il Dispatcher è abilitato, elabora queste richieste registrandosi come handler del tipo di contenuto `httpd/unix-directory`.

### Apache 2.x {#apache-x}

In Apache 2.x, le cose funzionano diversamente. Un modulo può gestire diverse fasi della richiesta, ad esempio la correzione dell’URL. `mod_dir` gestisce questa fase reindirizzando una richiesta (quando l’URL è mappato a una directory) all’URL con una `/` aggiunta.

Il Dispatcher non intercetta la correzione `mod_dir`, ma gestisce completamente la richiesta all’URL reindirizzato (vale a dire, con una `/` aggiunta). Ciò potrebbe causare un problema, se il server remoto (ad esempio, AEM) gestisce le richieste indirizzate a `/a_path` in modo diverso rispetto alle richieste indirizzate a `/a_path/` (quando `/a_path` viene mappato a una directory esistente).

Se ciò si verifica, occorre effettuare una di queste due azioni:

* Disattivare `mod_dir` per il sottoalbero `Directory` o `Location` gestito da Dispatcher

* Utilizzare `DirectorySlash Off` per configurare `mod_dir` in modo da evitare che venga aggiunta la `/`
