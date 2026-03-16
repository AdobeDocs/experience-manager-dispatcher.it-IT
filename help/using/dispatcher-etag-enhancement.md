---
title: Miglioramento di Dispatcher ETag per la riconvalida CDN
description: Disponibilità, stato del supporto e comportamento di INTERNAL_AEM_DISPATCHER_ETAG_ENHANCEMENT in AEM as a Cloud Service.
source-git-commit: ac0fafd060643903735ff565072ef2c5bee970be
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---

# Miglioramento di Dispatcher ETag per la riconvalida CDN

## Panoramica

Il flag `INTERNAL_AEM_DISPATCHER_ETAG_ENHANCEMENT` consente a Dispatcher di valutare l&#39;intestazione della richiesta `If-None-Match` negli hit della cache. Quando il valore `If-None-Match` in ingresso corrisponde a `ETag` memorizzato nella cache, Dispatcher può restituire `304 Not Modified` invece di `200 OK`.

Questo comportamento è progettato per ridurre il trasferimento di payload non necessario tra CDN e Dispatcher e aumentare l’efficienza della cache condizionale.

## Disponibilità

- Versione Dispatcher: `2.0.264`
- Build AEM SDK: `aem-sdk-2026.2.24464.20260214T050318Z-260100`

## Supporto AEM as a Cloud Service

In AEM as a Cloud Service, questa funzionalità è supportata per l’utilizzo da parte del cliente.

I clienti possono abilitarla impostando la variabile di ambiente `INTERNAL_AEM_DISPATCHER_ETAG_ENHANCEMENT` in Cloud Manager. Adobe può anche abilitarlo per conto del cliente quando necessario.

Se abilitata e quando la rete CDN invia `If-None-Match` e il relativo `ETag` è presente nella cache di Dispatcher, sono previste percentuali di risposta di `304` più elevate tra la rete CDN e Dispatcher. Questo aumento è il risultato atteso.

## Esempio di configurazione (intestazione ETag della cache)

Affinché questo miglioramento sia effettivo, assicurati che Dispatcher memorizzi nella cache l&#39;intestazione di risposta `ETag` e che il tuo server Web sia configurato in modo da evitare la generazione di ETag basati su file system.

Esempio di sezione cache `dispatcher.any`:

```text
/cache {
  /headers {
    "Cache-Control"
    "Content-Type"
    "Expires"
    "Last-Modified"
    "ETag"
  }
}
```

Esempio di direttiva Apache nel contesto Dispatcher vhost:

```apache
FileETag none
```

Per istruzioni sul caching delle intestazioni della linea di base, consulta [Memorizzazione in cache delle intestazioni di risposta HTTP](dispatcher-configuration.md#caching-http-response-headers).

## Esempio di convalida

Dopo aver abilitato la variabile di ambiente e aver distribuito le modifiche di configurazione:

1. Richiedi una volta di riscaldare la cache e acquisire `ETag` restituito.
1. Richiedi di nuovo con `If-None-Match: <etag-value>`.
1. Conferma Dispatcher restituisce `304 Not Modified` per i flussi di riconvalida con riscontri cache.

## Riferimento pubblico (comportamento correlato)

Per le linee guida della linea di base rivolte al cliente sul caching delle intestazioni e sulla gestione di `ETag` in Dispatcher, fare riferimento a:

- [Configurare Dispatcher - Memorizzazione in cache delle intestazioni di risposta HTTP](https://experienceleague.adobe.com/it/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration#caching-http-response-headers)

&quot;Questa funzionalità è disponibile in Dispatcher `2.0.264` (AEM SDK `2026.2.24464`). Quando è abilitato, Dispatcher può convalidare `If-None-Match` in base ai valori `ETag` memorizzati nella cache e restituire `304 Not Modified` negli hit della cache. In AEM as a Cloud Service, questo è supportato e può essere abilitato tramite la configurazione dell’ambiente Cloud Manager.&quot;
