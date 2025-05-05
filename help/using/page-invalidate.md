---
title: Annullamento della validità delle pagine in cache da AEM
description: Scopri come configurare l’interazione tra Dispatcher e AEM per garantire un’efficace gestione della cache.
cmgrlastmodified: 01.11.2007 08 22 29 [aheimoz]
pageversionid: 1193211344162
template: /apps/docs/templates/contentpage
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
exl-id: 90eb6a78-e867-456d-b1cf-f62f49c91851
source-git-commit: 9be9f5935c21ebbf211b5da52280a31772993c2e
workflow-type: tm+mt
source-wordcount: '1407'
ht-degree: 100%

---

# Annullamento della validità delle pagine in cache da AEM {#invalidating-cached-pages-from-aem}

Quando utilizzi Dispatcher con AEM, l’interazione deve essere configurata per garantire un’efficace gestione della cache. A seconda dell’ambiente, la configurazione può anche migliorare le prestazioni.

## Configurazione degli account utente AEM {#setting-up-aem-user-accounts}

L’account utente predefinito `admin` viene utilizzato per autenticare gli agenti di replica installati per impostazione predefinita. Crea un account utente dedicato da utilizzare con gli agenti di replica.

Per ulteriori informazioni, vedi la sezione [Configurare gli utenti di replica e trasporto](https://experienceleague.adobe.com/it/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions#VerificationSteps) dell’elenco di controllo della sicurezza di AEM.

<!-- OLD URL from above https://helpx.adobe.com/it/experience-manager/6-3/sites/administering/using/security-checklist.html#VerificationSteps -->

## Annullamento della validità della cache di Dispatcher dall’ambiente di authoring {#invalidating-dispatcher-cache-from-the-authoring-environment}

Un agente di replica sull’istanza Autore AEM invia una richiesta di annullamento della validità della cache a Dispatcher quando viene pubblicata una pagina. Dispatcher aggiorna il file alla fine nella cache quando viene pubblicato un nuovo contenuto.

<!-- 

Comment Type: draft

<p>Cache invalidation requests for a page are also generated for any aliases or vanity URLs that are configured in the page properties. </p>

 -->

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-0436B4A35714BFF67F000101@AdobeID)
Last Modified Date: 2017-05-25T10:37:23.679-0400

<p>Hiding this information due to <a href="https://jira.corp.adobe.com/browse/CQDOC-5805">CQDOC-5805</a>.</p>

 -->

Per configurare un agente di replica nell’istanza di authoring di AEM, utilizza la procedura descritta di seguito. La configurazione invalida la cache di Dispatcher all’attivazione della pagina:

1. Apri la console Strumenti AEM. (`https://localhost:4502/miscadmin#/etc`)
1. Apri l’agente di replica richiesto sotto Strumenti/replica/Agenti sull’autore. Puoi utilizzare l’agente di Dispatcher Flush installato per impostazione predefinita.
1. Fai clic su Modifica e, nella scheda Impostazioni, assicurati che sia selezionato **Abilitato**.

1. (facoltativo) Per abilitare le richieste di annullamento della validità di un alias o di un percorso personalizzato, seleziona l’opzione **Aggiornamento alias**.
1. Nella scheda Trasporto, accedi a Dispatcher immettendo l’URI.

   Se utilizzi l’agente Dispatcher Flush standard, aggiorna il nome host e la porta; ad esempio, https://&lt;*dispatcherHost*>:&lt;*portApache*>/dispatcher/invalidate.cache

   **Nota:** per gli agenti di svuotamento del Dispatcher, la proprietà URI viene utilizzata solo se utilizzi voci host virtuali basate sul percorso per differenziare le farm. Utilizza questo campo per individuare la farm da invalidare. Ad esempio, la farm n. 1 ha l’host virtuale `www.mysite.com/path1/*` e la farm n. 2 ha l’host virtuale `www.mysite.com/path2/*`. Puoi utilizzare l’URL `/path1/invalidate.cache` per individuare la prima farm e `/path2/invalidate.cache` per individuare la seconda farm. Per ulteriori informazioni, vedi [Utilizzo di Dispatcher con più domini](dispatcher-domains.md).

1. Configura altri parametri come richiesto.
1. Fai clic su OK per attivare l’agente.

In alternativa, puoi anche accedere e configurare l’agente di Dispatcher Flush dall’[interfaccia utente di AEM Touch](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/implementing/deploying/configuring/replication#configuring-a-dispatcher-flush-agent).

Per ulteriori dettagli su come abilitare l’accesso agli URL personalizzati, consulta [Abilitazione dell’accesso agli URL personalizzati](dispatcher-configuration.md#enabling-access-to-vanity-urls-vanity-urls).

>[!NOTE]
>
>L’agente di svuotamento della cache di Dispatcher non deve necessariamente avere un nome utente e una password, ma se configurato, viene inviato con l’autenticazione di base.

Questo approccio presenta due potenziali problemi:

* Dispatcher deve essere raggiungibile dall’istanza di authoring. Se la rete (ad esempio, il firewall) è configurata in modo tale da limitare l’accesso tra le due istanze, ciò potrebbe non verificarsi.

* La pubblicazione e l’annullamento della validità della cache avvengono contemporaneamente. A seconda della tempistica, un utente potrebbe richiedere una pagina subito dopo che è stata rimossa dalla cache e appena prima della pubblicazione della nuova pagina. AEM ora restituisce la pagina obsoleta e Dispatcher la memorizza nuovamente in cache. Questo è un problema piuttosto serio per i siti di grandi dimensioni.

## Annullamento della validità della cache di Dispatcher da un’istanza di pubblicazione {#invalidating-dispatcher-cache-from-a-publishing-instance}

In determinate circostanze è possibile ottenere vantaggi in termini di prestazioni trasferendo la gestione della cache dall’ambiente di authoring a un’istanza di pubblicazione. Sarà quindi l’ambiente di pubblicazione (non l’ambiente di authoring AEM) a inviare una richiesta di annullamento della validità della cache a Dispatcher quando viene ricevuta una pagina pubblicata.

Queste circostanze includono:

<!-- 

Comment Type: draft

<p>Cache invalidation requests for a page are also generated for any aliases or vanity URLs that are configured in the page properties. </p>

 -->

* Prevenire possibili conflitti di tempistica tra Dispatcher di AEM e l’istanza di pubblicazione (vedi [Annullamento della validità della cache di Dispatcher dall’ambiente di authoring](#invalidating-dispatcher-cache-from-the-authoring-environment)).
* Il sistema include diverse istanze di pubblicazione che risiedono su server ad alte prestazioni e una sola istanza di authoring.

>[!NOTE]
>
>Sarà quindi un amministratore AEM esperto a decidere se utilizzare questo metodo.

Un agente di replica che opera sull’istanza di pubblicazione controlla lo svuotamento del Dispatcher. Tuttavia, la configurazione viene effettuata nell’ambiente di authoring e poi trasferita attivando l’agente:

1. Apri la console Strumenti AEM.
1. Apri l’agente di replica richiesto sotto Strumenti/replica/Agenti in pubblicazione. Puoi utilizzare l’agente di Dispatcher Flush installato per impostazione predefinita.
1. Fai clic su Modifica e, nella scheda Impostazioni, assicurati che sia selezionato **Abilitato**.
1. (facoltativo) Per abilitare le richieste di annullamento della validità di un alias o di un percorso personalizzato, seleziona l’opzione **Aggiornamento alias**.
1. Nella scheda Trasporto, accedi a Dispatcher immettendo l’URI necessario.\
   Se utilizzi l’agente di eliminazione di Dispatcher standard, dovrai aggiornare il nome host e la porta; ad esempio, `http://<dispatcherHost>:<portApache>/dispatcher/invalidate.cache`

   **Nota:** per gli agenti di svuotamento del Dispatcher, la proprietà URI viene utilizzata solo se usi voci di host virtuali basate sul percorso per differenziare le farm. Utilizza questo campo per individuare la farm da invalidare. Ad esempio, la farm n. 1 ha l’host virtuale `www.mysite.com/path1/*` e la farm n. 2 ha l’host virtuale `www.mysite.com/path2/*`. Puoi utilizzare l’URL `/path1/invalidate.cache` per individuare la prima farm e `/path2/invalidate.cache` per individuare la seconda farm. Per ulteriori informazioni, vedi [Utilizzo di Dispatcher con più domini](dispatcher-domains.md).

1. Configura altri parametri come richiesto.
1. Accedi all’istanza di pubblicazione e convalida la configurazione dell’agente di svuotamento. Assicurati inoltre che sia abilitato.
1. Ripeti per ogni istanza Publish interessata.

Dopo la configurazione, quando attivi una pagina da Autore a Publish, questo agente avvia una replica standard. Il registro include messaggi che indicano le richieste provenienti dal server di pubblicazione, come nell’esempio seguente:

1. `<publishserver> 13:29:47 127.0.0.1 POST /dispatcher/invalidate.cache 200`

## Annullamento manuale della validità della cache di Dispatcher {#manually-invalidating-the-dispatcher-cache}

Per annullare la validità (o eseguire lo svuotamento) della cache del Dispatcher senza attivare una pagina, puoi inviare una richiesta HTTP al Dispatcher. Ad esempio, puoi creare un’applicazione AEM che consente agli amministratori o ad altre applicazioni di eseguire lo svuotamento della cache.

La richiesta HTTP fa in modo che Dispatcher elimini file specifici dalla cache. Facoltativamente, Dispatcher aggiorna la cache con una nuova copia.

### Elimina i file memorizzati nella cache {#delete-cached-files}

Invia una richiesta HTTP che induca Dispatcher a eliminare i file dalla cache. Dispatcher memorizza nuovamente in cache i file solo quando riceve dal client una richiesta per la pagina. Questa modalità di eliminazione dei file memorizzati in cache è appropriata per i siti web che hanno poche probabilità di ricevere richieste simultanee per la stessa pagina.

La richiesta HTTP ha il seguente formato:

```xml
POST /dispatcher/invalidate.cache HTTP/1.1  
CQ-Action: Activate  
CQ-Handle: path-pattern
Content-Length: 0
```

Dispatcher elimina (esegue il flushing) i file e le cartelle memorizzati in cache con nomi che corrispondono al valore dell’intestazione `CQ-Handler`. Ad esempio, un `CQ-Handle` con il valore `/content/geomtrixx-outdoors/en` corrisponde ai seguenti elementi:

* Tutti i file (con qualsiasi estensione) denominati `en` nella directory `geometrixx-outdoors`

* Ogni directory denominata `_jcr_content` sotto la directory `en` (che, se esiste, contiene i rendering memorizzati nella cache dei sottonodi della pagina)

Tutti gli altri file nella cache del Dispatcher (o fino a un determinato livello, a seconda dell’impostazione `/statfileslevel`) vengono invalidati toccando il file `.stat`. L’ultima data di modifica di questo file viene confrontata con l’ultima data di modifica di un documento memorizzato in cache e il documento viene recuperato, se il file `.stat` è più recente. Per ulteriori informazioni, vedi [Annullamento della validità dei file per livello di cartella](dispatcher-configuration.md#main-pars_title_26).

L’annullamento della validità (ovvero toccare i file .stat) può essere impedito inviando un’intestazione `CQ-Action-Scope: ResourceOnly` aggiuntiva. Questa funzionalità può essere utilizzata per svuotare determinate risorse. Tutto senza invalidare altre parti della cache, come i dati JSON. Tali dati vengono creati in modo dinamico e richiedono uno svuotamento regolare e indipendente della cache. Ad esempio, si possono rappresentare i dati ottenuti da un sistema di terze parti per la visualizzazione di notizie, quotazioni di borsa e così via.

### Eliminazione e rimemorizzazione in cache dei file {#delete-and-recache-files}

Invia una richiesta HTTP che induca Dispatcher a eliminare i file memorizzati nella cache e a recuperarli e rimemorizzarli immediatamente nella cache. Elimina e rimemorizza in cache immediatamente i file quando è probabile che i siti web ricevano dai client richieste simultanee per la stessa pagina. Il re-caching immediato garantisce che Dispatcher recuperi e memorizzi in cache la pagina una sola volta, invece di una volta per ciascuna delle richieste simultanee inviate dai client.

**Nota:** l’eliminazione e la rimemorizzazione nella cache dei file dovrebbero essere eseguite solo nell’istanza di pubblicazione. Quando viene eseguita dall’istanza Autore, si verificano condizioni di estrema competizione quando i tentativi di recuperare le risorse vengono effettuati prima che le risorse siano pubblicate.

La richiesta HTTP ha il seguente formato:

```xml
POST /dispatcher/invalidate.cache HTTP/1.1  
CQ-Action: Activate   
`Content-Type: text/plain  
CQ-Handle: path-pattern  
Content-Length: numchars in bodypage_path0

page_path1 
...  
page_pathn
```

I percorsi delle pagine per la rimemorizzazione nella cache immediata sono elencati su righe separate nel corpo del messaggio. Il valore di `CQ-Handle` è il percorso di una pagina che invalida le pagine da rimemorizzare in cache. (Vedi il parametro `/statfileslevel` dell’elemento di configurazione [Cache](dispatcher-configuration.md#main-pars_146_44_0010)). Il seguente messaggio di richiesta HTTP elimina e quindi rimemorizza in cache la `/content/geometrixx-outdoors/en.html page`:

```xml
POST /dispatcher/invalidate.cache HTTP/1.1  
CQ-Action: Activate  
Content-Type: text/plain   
CQ-Handle: /content/geometrixx-outdoors/en/men.html  
Content-Length: 36

/content/geometrixx-outdoors/en.html
```

### Esempio di servlet per il flushing {#example-flush-servlet}

Il codice seguente implementa un servlet che invia una richiesta di annullamento della validità al Dispatcher. Il servlet riceve un messaggio di richiesta contenente i parametri `handle` e `page`. Questi parametri forniscono rispettivamente il valore dell’intestazione `CQ-Handle` e il percorso della pagina da rimemorizzare in cache. Il servlet utilizza i valori per creare la richiesta HTTP per Dispatcher.

Quando il servlet viene distribuito all’istanza di pubblicazione, il seguente URL induce Dispatcher a eliminare la pagina /content/geometrixx-outdoors/en.html e, quindi, a memorizzare nella cache una nuova copia.

`10.36.79.223:4503/bin/flushcache/html?page=/content/geometrixx-outdoors/en.html&handle=/content/geometrixx-outdoors/en/men.html`

>[!NOTE]
>
>Questo esempio di servlet non è sicuro e serve solo a illustrare l’utilizzo del messaggio di richiesta HTTP Post. La tua soluzione deve proteggere l’accesso al servlet.
>

```java
package com.adobe.example;

import org.apache.felix.scr.annotations.Component;
import org.apache.felix.scr.annotations.Service;
import org.apache.felix.scr.annotations.Property;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import org.apache.commons.httpclient.*;
import org.apache.commons.httpclient.methods.PostMethod;
import org.apache.commons.httpclient.methods.StringRequestEntity;

@Component(metatype=true)
@Service
public class Flushcache extends SlingSafeMethodsServlet {

 @Property(value="/bin/flushcache")
 static final String SERVLET_PATH="sling.servlet.paths";

 private Logger logger = LoggerFactory.getLogger(this.getClass());

 public void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  try{ 
      //retrieve the request parameters
      String handle = request.getParameter("handle");
      String page = request.getParameter("page");

      //hard-coding connection properties is a bad practice, but is done here to simplify the example
      String server = "localhost"; 
      String uri = "/dispatcher/invalidate.cache";

      HttpClient client = new HttpClient();

      PostMethod post = new PostMethod("https://"+host+uri);
      post.setRequestHeader("CQ-Action", "Activate");
      post.setRequestHeader("CQ-Handle",handle);
   
      StringRequestEntity body = new StringRequestEntity(page,null,null);
      post.setRequestEntity(body);
      post.setRequestHeader("Content-length", String.valueOf(body.getContentLength()));
      client.executeMethod(post);
      post.releaseConnection();
      //log the results
      logger.info("result: " + post.getResponseBodyAsString());
      }
  }catch(Exception e){
      logger.error("Flushcache servlet exception: " + e.getMessage());
  }
 }
}
```
