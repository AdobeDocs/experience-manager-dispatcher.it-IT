---
title: Elenco di controllo della sicurezza di Dispatcher
description: Scopri di più sull’elenco di controllo della sicurezza di Dispatcher da completare prima di procedere alla produzione.
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
jcr-lastmodifiedby: remove-legacypath-6-1
index: y
internal: n
snippet: y
exl-id: 49009810-b5bf-41fd-b544-19dd0c06b013
source-git-commit: c41b4026a64f9c90318e12de5397eb4c116056d9
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 85%

---

# Elenco di controllo della sicurezza di Dispatcher{#the-dispatcher-security-checklist}

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-05T05:14:35.365-0400

<p>Food for thought listed on <a href="https://jira.corp.adobe.com/browse/DOC-5649">DOC-5649</a>. To be considered while proof-reading.</p> 
<p> </p>

 -->

Prima di procedere con la produzione, Adobe consiglia di completare l’elenco di controllo seguente.

>[!CAUTION]
>
>Completa l’elenco di controllo della sicurezza della tua versione di AEM prima prima della pubblicazione. Consulta la [Documentazione di Adobe Experience Manager](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/security/security-checklist) corrispondente.

## Usa la versione più recente di Dispatcher {#use-the-latest-version-of-dispatcher}

Installa la versione più recente disponibile per la tua piattaforma. Aggiorna l’istanza di Dispatcher per utilizzare la versione più recente e usufruire dei miglioramenti a livello di prodotto e sicurezza. Consulta [Installazione di Dispatcher](dispatcher-install.md).

>[!NOTE]
>
>L’attuale versione installata di Dispatcher è riportata nel file di registro di Dispatcher.
>
>`[Thu Apr 30 17:30:49 2015] [I] [23171(140735307338496)] Dispatcher initialized (build 4.1.9)`
>
>Per trovare il file di registro, controlla la configurazione di Dispatcher in `httpd.conf`.

## Limita i client che possono eseguire il flushing della cache {#restrict-clients-that-can-flush-your-cache}

Adobe consiglia di [limitare il numero dei client che possono eseguire il flushing della cache.](dispatcher-configuration.md#limiting-the-clients-that-can-flush-the-cache)

## Abilitare HTTPS per la sicurezza del livello di trasporto {#enable-https-for-transport-layer-security}

Adobe consiglia di abilitare il livello di trasporto HTTPS sia sulle istanze di authoring che di pubblicazione.

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-26T04:41:28.841-0400

<p>Recommended to have SSL termination, front end SSL.</p> 
<p>Question is do we want to have SSL communication between dispatcher and AEM instances (publish and/or author).</p> 
<p>We might want to have two items:</p> 
<ul> 
 <li>MUST HTTPS clients -&gt; dispatcher / load balancer</li> 
 <li>NICE load balancer -&gt; dispatcher<br /> </li> 
 <li>NICE dispatcher -&gt; instances if sensitive information such as credit cards / or infrastructure requirements such as DMZ</li> 
</ul>

 -->

## Limita l’accesso {#restrict-access}

Durante la configurazione di Dispatcher, limita il più possibile l’accesso esterno. Vedi [Esempio di sezione /filter](dispatcher-configuration.md#main-pars_184_1_title) nella documentazione di Dispatcher.

## Assicurati che l’accesso agli URL amministrativi sia negato {#make-sure-access-to-administrative-urls-is-denied}

Utilizza i filtri per bloccare l’accesso esterno a qualsiasi URL amministrativo, ad esempio alla console Web.

Per un elenco degli URL da bloccare, consulta [Test di sicurezza di Dispatcher](dispatcher-configuration.md#testing-dispatcher-security).

## Utilizzare i Inserire nell&#39;elenco Consentiti Inserisce nell&#39;elenco Bloccati di invece di quelli di {#use-allowlists-instead-of-blocklists}

Gli elenchi Consentiti permettono un migliore controllo degli accessi, in quanto presuppongono che tutte le richieste di accesso debbano essere negate, a meno che non facciano parte esplicitamente dell’elenco Consentiti. Questo modello offre un controllo più restrittivo sulle nuove richieste che potrebbero non essere state ancora esaminate o prese in considerazione durante una determinata fase della configurazione.

## Eseguire Dispatcher con un utente di sistema dedicato {#run-dispatcher-with-a-dedicated-system-user}

Configura Dispatcher in modo che un account utente dedicato con privilegi minimi esegua il server web. Adobe consiglia di concedere l’accesso in scrittura solo alla cartella della cache di Dispatcher.

Inoltre, gli utenti IIS devono configurare il proprio sito web nel modo seguente:

1. Nell’impostazione del percorso fisico per il sito web, seleziona **Connetti come utente specifico**.
1. Imposta l’utente.

## Previeni gli attacchi Denial of Service (DoS) {#prevent-denial-of-service-dos-attacks}

Un attacco Denial of Service (DoS) è un tentativo di rendere la risorsa di un computer indisponibile per gli utenti a cui è destinata.

A livello di Dispatcher, esistono due metodi di configurazione per evitare gli attacchi DoS: i [Filtri](https://experienceleague.adobe.com/it/docs#/filter)

* Utilizza il modulo mod_rewrite (ad esempio, [Apache 2.4](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html)) per eseguire le convalide degli URL (se le regole del pattern URL non sono troppo complesse).

* Impedisci a Dispatcher di memorizzare in cache gli URL con estensioni fittizie utilizzando dei [filtri](dispatcher-configuration.md#configuring-access-to-content-filter).\
  Puoi modificare le regole di caching in modo da limitare il caching ai tipi mime previsti, ad esempio:

   * `.html`
   * `.jpg`
   * `.gif`
   * `.swf`
   * `.js`
   * `.doc`
   * `.pdf`
   * `.ppt`

  È disponibile un esempio di file di configurazione per [limitare l’accesso esterno](#restrict-access). Include restrizioni per alcuni tipi MIME.

Per abilitare in modo sicuro la funzionalità completa sulle istanze di pubblicazione, configura i filtri per impedire l’accesso ai seguenti nodi:

* `/etc/`
* `/libs/`

Quindi, configura i filtri per consentire l’accesso ai seguenti percorsi di nodi:

* `/etc/designs/*`
* `/etc/clientlibs/*`
* `/etc/segmentation.segment.js`
* `/libs/cq/personalization/components/clickstreamcloud/content/config.json`
* `/libs/wcm/stats/tracker.js`
* `/libs/cq/personalization/*` (JS, CSS e JSON)
* `/libs/cq/security/userinfo.json` (Informazioni utente CQ)
* `/libs/granite/security/currentuser.json` (**i dati non devono essere memorizzati in cache**)

* `/libs/cq/i18n/*` (Internalizzazione)

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-26T04:38:17.016-0400

<p>We need to highlight whether a path applies to all versions or specific ones.<br /> </p>

 -->

## Configurare Dispatcher per impedire attacchi CSRF {#configure-dispatcher-to-prevent-csrf-attacks}

AEM fornisce un [framework](https://experienceleague.adobe.com/it/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions#verification-steps) per prevenire gli attacchi di tipo Cross-Site Request Forgery. Per utilizzare in modo appropriato questo framework, inserisci nell’elenco Consentiti il supporto per token CSRF in Dispatcher effettuando le seguenti operazioni:

1. Crea un filtro per consentire il percorso `/libs/granite/csrf/token.json`;
1. Aggiungi l’intestazione `CSRF-Token` alla sezione `clientheaders` della configurazione di Dispatcher.

## Previeni il clickjacking {#prevent-clickjacking}

Per prevenire gli attacchi clickjacking, Adobe consiglia di configurare il server web in modo da fornire l’`X-FRAME-OPTIONS`intestazione HTTP impostata su `SAMEORIGIN`.

Per ulteriori informazioni sul clickjacking, consulta il [sito OWASP](https://owasp.org/www-community/attacks/Clickjacking).

## Eseguire un test di penetrazione {#perform-a-penetration-test}

Adobe consiglia di eseguire un test di penetrazione dell’infrastruttura AEM prima di procedere alla produzione.

