---
title: Configurazione di Dispatcher di Adobe Experience Manager per impedire attacchi CSRF
description: Scopri come configurare Dispatcher di Adobe Experience Manager per impedire attacchi Cross-Site Request Forgery.
topic-tags: dispatcher
content-type: reference
exl-id: bcd38878-f977-46a6-b01a-03e4d90aef01
source-git-commit: 0a1aa854ea286a30c3527be8fc7c0998726a663f
workflow-type: tm+mt
source-wordcount: '235'
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
