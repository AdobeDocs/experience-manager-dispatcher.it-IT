---
title: Configurazione di Dispatcher di Adobe Experience Manager per impedire attacchi CSRF
description: Scopri come configurare Dispatcher di Adobe Experience Manager per impedire attacchi Cross-Site Request Forgery.
topic-tags: dispatcher
content-type: reference
exl-id: bcd38878-f977-46a6-b01a-03e4d90aef01
TQID: https://experienceleague.adobe.com/xbW-j06MGU1Ku5MwXscpLdpyw8KRLE18YIz-iUgAStI
product_v2:
  - id: fd1f54a9-f50c-467d-8956-cebbaf4f3eb8
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b68483fc6956bc0e6c2b1939d2203311da62987e
workflow-type: tm+mt
source-wordcount: 236
ht-degree: 100%

---

# Configurazione di Dispatcher di Adobe Experience Manager per impedire attacchi CSRF{#configuring-dispatcher-to-prevent-csrf-attacks}

AEM (Adobe Experience Manager) fornisce un framework per prevenire gli attacchi di tipo Cross-Site Request Forgery. Per utilizzare correttamente questo framework, apporta le seguenti modifiche alla configurazione di Dispatcher:

>[!NOTE]
>
>Non dimenticare di aggiornare i numeri delle regole negli esempi sotto riportati in base alla configurazione esistente. Ricorda che i Dispatcher utilizzano l’ultima regola corrispondente per concedere o negare un’autorizzazione, pertanto posiziona le regole nella parte inferiore dell’elenco esistente.

1. Nella sezione `/clientheaders` dei file `author-farm.any` e `publish-farm.any`, aggiungi la seguente voce in fondo all’elenco:\
   `CSRF-Token`
1. Nella sezione /filters dei file `author-farm.any` e `publish-farm.any` o `publish-filters.any`, aggiungi la seguente riga per consentire le richieste di `/libs/granite/csrf/token.json` tramite il Dispatcher.\
   `/0999 { /type "allow" /glob " * /libs/granite/csrf/token.json*" }`

1. Nella sezione `/cache /rules` del file `publish-farm.any`, aggiungi una regola per impedire a Dispatcher di memorizzare in cache il file `token.json`. In genere, le istanze di authoring ignorano il caching, pertanto non è necessario aggiungere la regola nel file `author-farm.any`.

   `/0999 { /glob "/libs/granite/csrf/token.json" /type "deny" }`

Per verificare che la configurazione funzioni, controlla il file dispatcher.log in modalità DEBUG. Può aiutarti a verificare il file `token.json` per assicurarti che non venga memorizzato in cache o bloccato dai filtri. Dovresti vedere messaggi simili ai seguenti:\
`... checking [/libs/granite/csrf/token.json]  `
`... request URL not in cache rules: /libs/granite/csrf/token.json`\
`... cache-action for [/libs/granite/csrf/token.json]: NONE`

Puoi anche verificare che le richieste abbiano esito positivo nel file `access_log` di Apache. Le richieste di ``/libs/granite/csrf/token.json devono restituire il codice di stato HTTP 200.
