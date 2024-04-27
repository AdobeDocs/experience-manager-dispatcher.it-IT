---
title: Configurazione di Dispatcher per impedire attacchi CSRF
description: Scopri come configurare AEM Dispatcher per impedire attacchi Cross-Site Request Forgery.
topic-tags: dispatcher
content-type: reference
exl-id: bcd38878-f977-46a6-b01a-03e4d90aef01
source-git-commit: 2d90738d01fef6e37a2c25784ed4d1338c037c23
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 46%

---

# Configurazione di Dispatcher per impedire attacchi CSRF {#configuring-dispatcher-to-prevent-csrf-attacks}

AEM fornisce un framework per prevenire gli attacchi di tipo Cross-Site Request Forgery. Per utilizzare correttamente questo framework, apporta le seguenti modifiche alla configurazione di Dispatcher:

>[!NOTE]
>
>Non dimenticare di aggiornare i numeri delle regole negli esempi sotto riportati in base alla configurazione esistente. Ricorda che i dispatcher utilizzano l’ultima regola corrispondente per concedere o negare un’autorizzazione, pertanto posiziona le regole in fondo all’elenco esistente.

1. In `/clientheaders` sezione del tuo `author-farm.any` e `publish-farm.any`, aggiungi la seguente voce in fondo all’elenco:\
   `CSRF-Token`
1. Nella sezione /filters del `author-farm.any` e `publish-farm.any` o `publish-filters.any` , aggiungi la seguente riga per consentire le richieste di `/libs/granite/csrf/token.json` tramite Dispatcher.\
   `/0999 { /type "allow" /glob " * /libs/granite/csrf/token.json*" }`
1. Sotto `/cache /rules` sezione del tuo `publish-farm.any`, aggiungi una regola per impedire a Dispatcher di memorizzare in cache `token.json` file. In genere, gli autori ignorano il caching, pertanto non è necessario aggiungere la regola nel file `author-farm.any`.\
   `/0999 { /glob "/libs/granite/csrf/token.json" /type "deny" }`

Per verificare che la configurazione funzioni, esamina il file dispatcher.log in modalità DEBUG per accertarti che il file token.json non sia memorizzato in cache e non sia bloccato da filtri. Dovresti vedere messaggi simili ai seguenti:\
`... checking [/libs/granite/csrf/token.json]  `
`... request URL not in cache rules: /libs/granite/csrf/token.json`\
`... cache-action for [/libs/granite/csrf/token.json]: NONE`

Puoi anche verificare che le richieste abbiano esito positivo in Apache `access_log`. Le richieste di ``/libs/granite/csrf/token.json devono restituire il codice di stato HTTP 200.
