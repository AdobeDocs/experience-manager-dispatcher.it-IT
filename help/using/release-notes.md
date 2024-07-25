---
title: Note sulla versione di AEM Dispatcher
description: Note sulla versione specifiche di Adobe Experience Manager Dispatcher
topic-tags: release-notes
content-type: reference
products: SG_EXPERIENCEMANAGER/6.4
exl-id: b55c7a34-d57b-4d45-bd83-29890f1524de
source-git-commit: 5f743be9c143e1e720e59304feebaa2e272dad87
workflow-type: ht
source-wordcount: '1062'
ht-degree: 100%

---

# Note sulla versione di AEM Dispatcher{#aem-dispatcher-release-notes}

## Informazioni sulla versione {#release-information}

|  |  |
|--- |--- |
| Prodotti | Adobe Experience Manager (AEM) Dispatcher |
| Versione | 4.3.7 |
| Tipo | Versione secondaria |
| Data | 27 marzo 2024 |
| URL di download | <ul><li>[Apache 2.4](#apache)</li><li>[Microsoft® Internet Information Services (IIS)](#iis)</li></ul> |
| Compatibilità | AEM 6.1 o superiore |

## Requisiti e prerequisiti di sistema {#system-requirements-and-prerequisites}

Per ulteriori informazioni su requisiti e prerequisiti, consulta [Piattaforme supportate](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/implementing/deploying/introduction/technical-requirements#dispatcher-platforms-web-servers).

Adobe consiglia di utilizzare l’ultima versione di AEM Dispatcher per usufruire delle funzionalità e delle correzioni di bug più recenti e delle migliori prestazioni possibili.

## Istruzioni di installazione {#installation-instructions}

Per istruzioni dettagliate, vedi [Installazione di Dispatcher](dispatcher-install.md).

## Cronologia delle versioni {#release-history}

### Versione 4.3.7 (27 marzo 2024) {#march}

**Miglioramenti**:

* DISP-1009: impostare di nuovo la lunghezza dell’intestazione
* DISP-1013: aggiunta del supporto Openssl 3.0 per Linux®
* DISP-1014: l’elaborazione response.location genera un reindirizzamento non valido
* DISP-1017: modifica della definizione della DTD

### Versione 4.3.6 (25 luglio 2023) {#jyly}

**Miglioramenti**:

* DISP-911 - AEM-05 - X-Edge-Key può trapelare in disp_apache2.c
* DISP-937 - Registrazione di tutti i selettori.
* DISP-998 - Possibilità di rendere configurabile il caricamento di URL personalizzati all’avvio.

### Versione 4.3.5 (4 aprile 2022) {#apr}

**Miglioramenti**:

* DISP-954 - supporta l’invalidazione anche se la scadenza non è stata superata
* DISP-949: Dispatcher restituisce 200 invece di 404 anche se la richiesta POST è bloccata dal filtro

### Versione 4.3.4 (29 novembre 2021) {#nov}

**Correzioni di bug**:

* DISP-833 - Le intestazioni X-Forwarded-Host possono contenere un elenco di nomi host separati da virgola.
* DISP-835 - DispatcherUseForwardedHost assorbe l’intestazione host se arriva per ultima.

**Miglioramenti**:

* DISP-874 - Creare una configurazione di Dispatcher per attivare o disattivare l’implementazione di DISP-818 tramite un flag `DispatcherRestrictUncacheableContent`. Il valore predefinito è Disattivato. Quando è attivato, rimuove le intestazioni di memorizzazione in cache impostate da mod_expires per il contenuto da non memorizzare nella cache. L’impostazione è diversa dal comportamento rilevato nella versione 4.3.3 (dove il valore predefinito era Attivato), ma la stessa delle versioni precedenti alla 4.3.3 (dove il valore predefinito era Disattivato). Si consiglia di mantenere l’impostazione predefinita Off per `DispatcherRestrictUncacheableContent` in modo che la cache del browser sia più flessibile. Se, durante l’aggiornamento dalla versione 4.3.3 alla versione 4.3.4, desideri mantenere lo stesso comportamento della versione 4.3.3, imposta esplicitamente `DispatcherRestrictUncacheableContent` su Attivato.
* DISP-841 - Dispatcher non rispetta /serverStaleOnError per il codice di risposta 504
* DISP-874 - Creare una configurazione di Dispatcher per attivare o disattivare l’implementazione di DISP-818
* DISP-883 - Taccia che mostra la decomposizione della richiesta URL in Dispatcher
* DISP-944: pre-caricamento degli URL personalizzati

### Versione 4.3.3 (18 ottobre 2019) {#october}

**Correzioni di bug**:

* DISP-739: LogLevel Dispatcher: il **livello** non funziona
* DISP-749: arresto anomalo di Dispatcher in Alpine Linux® con livello del registro di traccia

**Miglioramenti**:

* DISP-813 - Supporto in Dispatcher per openssl 1.1.x
* DISP-814 - Errori Apache 40x durante i flushing della cache
* DISP-818 - mod_expires aggiunge intestazioni Cache-Control per contenuto non memorizzabile in cache
* DISP-821: non archiviare il contesto di registro nel socket
* DISP-822 - Dispatcher deve utilizzare `ppoll` invece di `pselect`
* DISP-824 - DispatcherUseForwardedHost sicuro
* DISP-825: inserisce nel registro un messaggio speciale quando non c’è più spazio sul disco
* DISP-826: supporto di recupero URI con una stringa di query

**Nuove funzioni**:

* DISP-703 - Rapporto hit della cache specifico per farm
* DISP-827 - Server locale per test
* DISP-828 - Creare un’immagine docker di test per Dispatcher

### Versione 4.3.2 (31 gennaio 2019) {#jan}

**Correzioni di bug**:

* DISP-734 - Dispatcher causa un arresto anomalo in insert_output_filter se non viene impostato come handler
* DISP-735 - I RE non funzionano su Alpine Linux®
* DISP-740 - Il caricamento di Dispatcher in macOS Mojave è disabilitato per impostazione predefinita
* DISP-742 - Le richieste bloccate potrebbero causare la perdita di informazioni alle risorse protette da Auth Checker

**Miglioramenti**:

* DISP-746 - Le stringhe non etichettate in dispatcher.any devono generare un avviso

**Nuova funzione**:

* DISP-747- Fornisce informazioni sulle richieste in ambiente Apache

### Versione 4.3.1 (16 ottobre 2018) {#oct}

**Correzioni di bug**:

* DISP-656 - Dispatcher fornisce un’intestazione ETag sbagliata
* DISP-694 - Elimina gli avvisi quando le connessioni keep alive diventano obsolete
* DISP-714 - La gestione delle sessioni basata su cookie non funziona in IIS
* DISP-715 - Flag sicuro per il cookie renderid
* DISP-720 - I file temporanei non chiusi possono causare la saturazione (troppi file aperti)
* DISP-721 - Dispatcher interrompe il poll() quando Apache effettua un graceful restart dell’elemento figlio
* DISP-722 - I file cache vengono creati con modalità ottale 0600
* DISP-723 - Timeout implicito di 10 minuti (e nuovo tentativo) quando i timeout del rendering sono impostati su 0
* DISP-725 - I caratteri finali dopo le stringhe vengono automaticamente convertiti in valori senza nome
* DISP-726 - Inserisce un avviso nel registro quando nessuna farm corrisponde effettivamente all’host in ingresso
* DISP-727 - Dispatcher controlla la lunghezza del contenuto della richiesta per i file di cache vuoti
* DISP-730: Errore 404 quando si tenta di accedere al file di intestazione tramite Dispatcher
* DISP-731 - Dispatcher è vulnerabile agli attacchi Log Injection
* DISP-732 - Dispatcher deve rimuovere ‘/’ consecutivi nell’URL
* DISP-733 - Dispatcher deve impostare (calcolare) un’intestazione Age

**Miglioramenti**:

* DISP-656 - Dispatcher fornisce un’intestazione ETag sbagliata
* DISP-694 - Elimina gli avvisi quando le connessioni keep alive diventano obsolete
* DISP-715 - Flag sicuro per il cookie renderid
* DISP-722 - I file cache vengono creati con modalità ottale 0600
* DISP-726 - Inserisce un avviso nel registro quando nessuna farm corrisponde effettivamente all’host in ingresso

### Versione 4.3.0 (13 giugno 2018) {#jun}

**Correzioni di bug**:

* DISP-682 - Livello di registro numerico applicato in modo errato
* DISP-685: i dati binari di Solaris™ SPARC® a 32 bit hanno un riferimento indefinito a __divdi3
* DISP-688 - Dispatcher non restituisce l’intestazione “X-Cache-Info” nella risposta 404
* DISP-690 - L’intestazione Last-Modified non è memorizzabile in cache
* DISP-691 - Violazioni di accesso in w3wp.exe
* DISP-693: è necessario aggiornare i dettagli di architettura dei server Solaris™ nella pagina di download di Dispatcher
* DISP-695 - Problema con il livello DispatcherLog nel modulo Dispatcher 4.2.3
* DISP-698 - Il TTL di Dispatcher deve supportare le direttive s-maxage e private
* DISP-700 - Il modulo non funziona correttamente su Alpine Linux®
* DISP-704 - Le richieste del browser contenenti %2b vengono inviate all’editore senza codifica
* DISP-705 - Arresto anomalo di Dispatcher a causa di una vulnerabilità DF (Double Free) o di danneggiamento (fasttop)
* DISP-706: durante l’annullamento della validità, Dispatcher segue dei symlink con riferimento all’indietro che possono causare un loop infinito
* DISP-709 - Blocca alcune estensioni di URL personalizzati
* DISP-710 - Build per Linux® non utilizzabili su Cent OS 6

**Miglioramenti**:

* DISP-652 - L’intestazione della data fornita da Dispatcher non è corretta

## Risorse utili {#helpful-resources}

* [Panoramica di AEM Dispatcher](dispatcher.md)

## Download {#downloads}

### Apache 2.4 {#apache}

| Piattaforma | Architettura | Supporto OpenSSL | Fai clic per scaricare |
|---|---|---|---|
| Linux® | i686 (32 bit) | Nessuno | [`dispatcher-apache2.4-linux-i686-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-i686-4.3.7.tar.gz) |
| Linux® | i686 (32 bit) | 1.0 | [`dispatcher-apache2.4-linux-i686-ssl1.0-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-i686-ssl1.0-4.3.7.tar.gz) |
| Linux® | i686 (32 bit) | 1.1 | [`dispatcher-apache2.4-linux-i686-ssl1.1-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-i686-ssl1.1-4.3.7.tar.gz) |
| Linux® | i686 (32 bit) | 3.0 | [`dispatcher-apache2.4-linux-i686-ssl3.0-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-i686-ssl3.0-4.3.7.tar.gz) |
| Linux® | x86_64 (64 bit) | Nessuno | [`dispatcher-apache2.4-linux-x86_64-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-x86_64-4.3.7.tar.gz) |
| Linux® | x86_64 (64 bit) | 1.0 | [`dispatcher-apache2.4-linux-x86_64-ssl1.0-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-x86_64-ssl1.0-4.3.7.tar.gz) |
| Linux® | x86_64 (64 bit) | 1.1 | [`dispatcher-apache2.4-linux-x86_64-ssl1.1-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-x86_64-ssl1.1-4.3.7.tar.gz) |
| Linux® | x86_64 (64 bit) | 3.0 | [`dispatcher-apache2.4-linux-x86_64-ssl3.0-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-x86_64-ssl3.0-4.3.7.tar.gz) |
| Linux® | aarch64 (64 bit) | Nessuno | [`dispatcher-apache2.4-linux-aarch64-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-aarch64-4.3.7.tar.gz) |
| Linux® | aarch64 (64 bit) | 1.0 | [`dispatcher-apache2.4-linux-aarch64-ssl1.0-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-aarch64-ssl1.0-4.3.7.tar.gz) |
| Linux® | aarch64 (64 bit) | 1.1 | [`dispatcher-apache2.4-linux-aarch64-ssl1.1-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-aarch64-ssl1.1-4.3.7.tar.gz) |
| Linux® | aarch64 (64 bit) | 3.0 | [`dispatcher-apache2.4-linux-aarch64-ssl3.0-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-aarch64-ssl3.0-4.3.7.tar.gz) |
| macOS | arm64 (64 bit) | Nessuno | [`dispatcher-apache2.4-darwin-arm64-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-darwin-arm64-4.3.7.tar.gz) |
| macOS | x86_64 (64 bit) | Nessuno | [`dispatcher-apache2.4-darwin-x86_64-4.3.7.tar.gz`](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-darwin-x86_64-4.3.7.tar.gz) |

### IIS {#iis}

| Piattaforma | Architettura | Supporto OpenSSL | Fai clic per scaricare |
|---|---|---|---|
| Windows | x86 (32 bit) | Nessuno | [`dispatcher-iis-windows-x86-4.3.7.zip`](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x86-4.3.7.zip) |
| Windows | x86 (32 bit) | 1.0 | [`dispatcher-iis-windows-x86-ssl1.0-4.3.7.zip`](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x86-ssl1.0-4.3.7.zip) |
| Windows | x86 (32 bit) | 1.1 | [`dispatcher-iis-windows-x86-ssl1.1-4.3.7.zip`](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x86-ssl1.1-4.3.7.zip) |
| Windows | x64 (64 bit) | Nessuno | [`dispatcher-iis-windows-x64-4.3.7.zip`](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-4.3.7.zip) |
| Windows | x64 (64 bit) | 1.0 | [`dispatcher-iis-windows-x64-ssl1.0-4.3.7.zip`](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-ssl1.0-4.3.7.zip) |
| Windows | x64 (64 bit) | 1.1 | [`dispatcher-iis-windows-x64-ssl1.1-4.3.7.zip`](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-ssl1.1-4.3.7.zip) |
| Windows | x64 (64 bit) | 3.0 | [`dispatcher-iis-windows-x64-ssl3.0-4.3.7.zip`](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-ssl3.0-4.3.7.zip) |