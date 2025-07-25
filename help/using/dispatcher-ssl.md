---
title: Usa SSL con Dispatcher
description: Scopri come configurare Dispatcher per comunicare con AEM utilizzando connessioni SSL.
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
index: y
internal: n
snippet: y
exl-id: ec378409-ddb7-4917-981d-dbf2198aca98
source-git-commit: c41b4026a64f9c90318e12de5397eb4c116056d9
workflow-type: tm+mt
source-wordcount: '1305'
ht-degree: 91%

---

# Usa SSL con Dispatcher {#using-ssl-with-dispatcher}

Utilizza connessioni SSL tra Dispatcher e il computer di rendering:

* [SSL unidirezionale](#use-ssl-when-dispatcher-connects-to-aem)
* [SSL reciproco](#configuring-mutual-ssl-between-dispatcher-and-aem)

>[!NOTE]
>
>Le operazioni relative ai certificati SSL sono associate a prodotti di terze parti. Non sono coperte dal contratto Adobe Platinum Maintenance and Support.

## Usa SSL quando Dispatcher si connette ad AEM {#use-ssl-when-dispatcher-connects-to-aem}

Configura Dispatcher per comunicare con l’istanza di rendering AEM o CQ utilizzando connessioni SSL.

Prima di configurare Dispatcher, configura AEM o CQ per l’utilizzo di SSL. Per ulteriori informazioni, consulta:

* [SSL/TLS per impostazione predefinita](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/security/ssl-by-default)
* [Utilizzare la procedura guidata SSL in AEM](https://experienceleague.adobe.com/it/docs/experience-manager-learn/foundation/security/use-the-ssl-wizard)

### Intestazioni di richiesta relative a SSL {#ssl-related-request-headers}

Quando il Dispatcher riceve una richiesta HTTPS, include le seguenti intestazioni nella richiesta successiva, che invia ad AEM o CQ:

* `X-Forwarded-SSL`
* `X-Forwarded-SSL-Cipher`
* `X-Forwarded-SSL-Keysize`
* `X-Forwarded-SSL-Session-ID`

Una richiesta tramite Apache-2.4 con `mod_ssl` include intestazioni simili a quelle del seguente esempio:

```shell
X-Forwarded-SSL: on
X-Forwarded-SSL-Cipher: DHE-RSA-AES256-SHA
X-Forwarded-SSL-Session-ID: 814825E8CD055B4C166C2EF6D75E1D0FE786FFB29DEB6DE1E239D5C771CB5B4D
```

### Configurare Dispatcher per l’utilizzo di SSL {#configuring-dispatcher-to-use-ssl}

Per configurare Dispatcher per la connessione con AEM o CQ tramite SSL, il file [dispatcher.any](dispatcher-configuration.md) richiede le seguenti proprietà:

* Un host virtuale che gestisca le richieste HTTPS.
* La sezione `renders` dell&#39;host virtuale include un elemento che identifica il nome host e la porta dell&#39;istanza CQ o AEM che utilizza HTTPS.
* L&#39;elemento `renders` include una proprietà denominata `secure` con valore `1`.

Nota: crea un altro host virtuale per la gestione delle richieste HTTP, se necessario.

Il seguente esempio di file `dispatcher.any` mostra i valori delle proprietà per la connessione tramite HTTPS a un’istanza CQ in esecuzione sull’host `localhost` e sulla porta `8443`:

```
/farms
{
   /secure
   { 
      /virtualhosts
      {
         # select this farm for all incoming HTTPS requests
         "https://*"
      }
      /renders
      {
      /0001
         {
            # hostname or IP of the render
            /hostname "localhost"
            # port of the render
            /port "8443"
            # connect via HTTPS
            /secure "1"
         }
      }
     # the rest of the properties are omitted
   }

   /non-secure
   { 
      /virtualhosts
      {
         # select this farm for all incoming HTTP requests
         "http://*"
      }
      /renders
      {
         /0001
      {
         # hostname or IP of the render
         /hostname "localhost"
         # port of the render
         /port "4503"
      }
   }
    # the rest of the properties are omitted
}
```

## Configurare SSL reciproco tra Dispatcher e AEM {#configuring-mutual-ssl-between-dispatcher-and-aem}

Per utilizzare l’autenticazione SSL reciproca, configura le connessioni tra il Dispatcher e il computer di rendering (in genere un’istanza di pubblicazione AEM o CQ):

* Dispatcher si connette all’istanza di rendering tramite SSL.
* L’istanza di rendering verifica la validità del certificato di Dispatcher.
* Dispatcher verifica che la CA del certificato dell’istanza di rendering sia attendibile.
* (Facoltativo) Dispatcher verifica che il certificato dell’istanza di rendering corrisponda all’indirizzo del server dell’istanza di rendering.

Per configurare SSL reciproco, è necessario disporre di certificati firmati da un’autorità di certificazione (CA) attendibile. I certificati autofirmati non sono sufficienti. Puoi fungere da CA o utilizzare i servizi di una CA di terze parti per firmare i certificati. Per configurare SSL reciproco, sono necessari i seguenti elementi:

* Certificati firmati per l’istanza di rendering e Dispatcher
* Certificato CA (se sei tu a fungere da CA)
* Librerie OpenSSL per la generazione di CA, certificati e richieste di certificati

Per configurare l’autenticazione SSL reciproca, esegui i seguenti passaggi:

1. [Installa](dispatcher-install.md) la versione più recente di Dispatcher per la piattaforma in uso. Utilizza un file binario di Dispatcher che supporta SSL (SSL compare nel nome file, ad esempio `dispatcher-apache2.4-linux-x86-64-ssl10-4.1.7.tar`).
1. [Crea o ottieni un certificato firmato dalla CA](dispatcher-ssl.md#main-pars-title-3) per Dispatcher e l’istanza di rendering.
1. [Crea un archivio chiavi contenente il certificato di rendering](dispatcher-ssl.md#main-pars-title-6) e configura il servizio HTTP del rendering.
1. [Configura il modulo server Web di Dispatcher](dispatcher-ssl.md#main-pars-title-4) per SSL reciproco.

### Creare o ottenere certificati firmati da una CA {#creating-or-obtaining-ca-signed-certificates}

Crea o ottieni i certificati firmati da una CA che autentichino l’istanza di pubblicazione e di Dispatcher.

#### Crea la tua CA {#creating-your-ca}

Se fungi da CA, utilizza [OpenSSL](https://www.openssl.org/) per creare l’Autorità di certificazione (CA) che firma i certificati per server e client. (È necessario che siano installate le librerie OpenSSL). Se utilizzi una CA di terze parti, non eseguire questa procedura.

1. Apri un terminale e cambia la directory corrente con la directory che contiene il file `CA.sh`, ad esempio `/usr/local/ssl/misc`.
1. Per creare la CA, immetti il seguente comando, quindi specifica i valori quando ti viene richiesto:

   ```shell
   ./CA.sh -newca
   ```

   >[!NOTE]
   >
   >Diverse proprietà nel file `openssl.cnf` controllano il comportamento dello script CA.sh. Modifica questo file come necessario prima di creare la CA.

#### Creare i certificati {#creating-the-certificates}

Utilizza OpenSSL per creare le richieste di certificato da inviare alla CA di terze parti o per firmare con la tua CA.

Quando crei un certificato, OpenSSL utilizza la proprietà Common Name per identificare il titolare del certificato. Per il certificato dell’istanza di rendering, utilizza il nome host del computer dell’istanza come nome comune se configuri Dispatcher per accettare il certificato. Esegui questa operazione solo se corrisponde al nome host dell’istanza di pubblicazione. Consulta la proprietà [DispatcherCheckPeerCN](dispatcher-ssl.md#main-pars-title-11).

1. Apri un terminale e cambia la directory corrente con la directory che contiene il file CH.sh delle librerie OpenSSL.
1. Immetti il seguente comando e fornisci i valori quando richiesto. Se necessario, utilizza il nome host dell’istanza di pubblicazione come nome comune. Il nome host è un nome risolvibile DNS per l&#39;indirizzo IP del rendering:

   ```shell
   ./CA.sh -newreq
   ```

   Se utilizzi una CA di terze parti, invia il file newreq.pem alla CA per la firma. Se sei tu a fungere da CA, continua con il passaggio 3.

1. Per firmare il certificato utilizzando il certificato della CA, immetti il comando seguente:

   ```shell
   ./CA.sh -sign
   ```

   Nella directory che contiene i file di gestione della CA vengono creati due file chiamati `newcert.pem` e `newkey.pem`. Questi due file sono rispettivamente il certificato pubblico e la chiave privata per il computer di rendering.

1. Rinomina `newcert.pem` in `rendercert.pem` e rinomina `newkey.pem` in `renderkey.pem`.
1. Ripeti i passaggi 2 e 3 per creare un nuovo certificato e una nuova chiave pubblica per il modulo Dispatcher. Utilizzare un nome comune specifico per l’istanza di Dispatcher.
1. Rinomina `newcert.pem` in `dispcert.pem` e rinomina `newkey.pem` in `dispkey.pem`.

### Configurare SSL sul computer di rendering {#configuring-ssl-on-the-render-computer}

Configura SSL sull’istanza di rendering utilizzando i file `rendercert.pem` e `renderkey.pem`.

#### Convertire il certificato di rendering in formato JKS (Java™ KeyStore) {#converting-the-render-certificate-to-jks-format}

Utilizza il seguente comando per convertire il certificato di rendering, che è un file PEM, in un file PKCS#12. Includi anche il certificato della CA che ha firmato il certificato di rendering:

1. In una finestra del terminale, cambia la directory corrente con quella che contiene il certificato di rendering e la chiave privata.
1. Per convertire il certificato di rendering, che è un file PEM, in un file PKCS#12, immetti il comando seguente. Includi anche il certificato della CA che ha firmato il certificato di rendering:

   ```shell
   openssl pkcs12 -export -in rendercert.pem -inkey renderkey.pem  -certfile demoCA/cacert.pem -out rendercert.p12
   ```

1. Per convertire il file PKCS#12 in formato Java™ KeyStore (JKS), immetti il seguente comando:

   ```shell
   keytool -importkeystore -srckeystore servercert.p12 -srcstoretype pkcs12 -destkeystore render.keystore
   ```

1. Il file Java™ Keystore viene creato utilizzando un alias predefinito. Se vuoi, puoi modificare l’alias:

   ```shell
   keytool -changealias -alias 1 -destalias jettyhttp -keystore render.keystore
   ```

#### Aggiungi il certificato CA al TrustStore del rendering {#adding-the-ca-cert-to-the-render-s-truststore}

Se sei tu a fungere da CA, importa il certificato CA in un keystore. Quindi, configura la JVM che esegue l&#39;istanza di rendering in modo che consideri attendibile il keystore.

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (sbroders@adobe.com)
Last Modified Date: 2014-08-12T13:11:21.401-0400

<p style="font-family: tahoma, arial, helvetica, sans-serif; font-size: 12px;">The jetty http service has properties to specify trusted CA certificates for mutual SSL for 6.0. Whether they are operable is undetetermined. See https://issues.adobe.com/browse/DOC-4738.</p> 
<p style="font-family: tahoma, arial, helvetica, sans-serif; font-size: 12px;"> </p> 
<p style="font-family: tahoma, arial, helvetica, sans-serif; font-size: 12px;">For 5.6.1, you would specify the system property javax.net.ssl.trustStore, using the path to cacerts as value.</p>

 -->

1. Utilizza un editor di testo per aprire il file cacert.pem e rimuovere tutto il testo che precede la seguente riga:

   `-----BEGIN CERTIFICATE-----`

1. Utilizza il seguente comando per importare il certificato in un keystore:

   ```shell
   keytool -import -keystore cacerts.keystore -alias myca -storepass password -file cacert.pem
   ```

1. Per configurare la JVM che esegue l&#39;istanza di rendering in modo che consideri attendibile il keystore, utilizza la seguente proprietà di sistema:

   ```shell
   -Djavax.net.ssl.trustStore=<location of cacerts.keystore>
   ```

   Ad esempio, se utilizzi lo script crx-quickstart/bin/quickstart per avviare l&#39;istanza Publish, puoi modificare la proprietà CQ_JVM_OPTS:

   ```shell
   CQ_JVM_OPTS='-server -Xmx2048m -XX:MaxPermSize=512M -Djavax.net.ssl.trustStore=/usr/lib/cq6.0/publish/ssl/cacerts.keystore'
   ```

#### Configurare l’istanza di rendering {#configuring-the-render-instance}

Per configurare il servizio HTTP dell’istanza di rendering per l’utilizzo di SSL, utilizza il certificato di rendering con le istruzioni contenute nella sezione *`Enable SSL on the Publish Instance`*:

* AEM 6.2: [abilitazione di HTTP su SSL](https://experienceleague.adobe.com/it/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions)
* AEM 6.1: [abilitazione di HTTP su SSL](https://experienceleague.adobe.com/it/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions)
* Versioni precedenti di AEM: visita [questa pagina.](https://experienceleague.adobe.com/it/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions)

### Configurare SSL per il modulo Dispatcher {#configuring-ssl-for-the-dispatcher-module}

Per configurare Dispatcher per l’utilizzo di SSL reciproco, prepara il certificato di Dispatcher, quindi configura il modulo server web.

### Creazione di un certificato Dispatcher unificato {#creating-a-unified-dispatcher-certificate}

Combina il certificato di Dispatcher e la chiave privata non crittografata in un unico file PEM. Utilizza un editor di testo o il comando `cat` per creare un file simile a quello del seguente esempio:

1. Apri un terminale e cambia la directory corrente con quella che contiene il file dispatkey.pem.
1. Per decrittografare la chiave privata, immetti il comando seguente:

   ```shell
   openssl rsa -in dispkey.pem -out dispkey_unencrypted.pem
   ```

1. Utilizza un editor di testo o il comando `cat` per combinare la chiave privata non crittografata e il certificato in un unico file simile a quello seguente esempio:

   ```xml
   -----BEGIN RSA PRIVATE KEY-----
   MIICxjBABgkqhkiG9w0B...
   ...M2HWhDn5ywJsX
   -----END RSA PRIVATE KEY-----
   -----BEGIN CERTIFICATE-----
   MIIC3TCCAk...
   ...roZAs=
   -----END CERTIFICATE-----
   ```

### Specifica il certificato da utilizzare per Dispatcher {#specifying-the-certificate-to-use-for-dispatcher}

Aggiungi le seguenti proprietà alla [configurazione del modulo Dispatcher](dispatcher-install.md#main-pars-55-35-1022) (nel file `httpd.conf`):

* `DispatcherCertificateFile`: percorso del file del certificato unificato di Dispatcher contenente il certificato pubblico e la chiave privata non crittografata. Questo file viene utilizzato quando il server SSL richiede il certificato del client di Dispatcher.
* `DispatcherCACertificateFile`: percorso del file del certificato CA. Utilizzato se il server SSL presenta una CA non considerata attendibile da un’autorità principale.
* `DispatcherCheckPeerCN`: determina se abilitare (`On`) o disabilitare (`Off`) la verifica del nome host per i certificati del server remoto.

Il codice che segue è un esempio di configurazione:

```xml
<IfModule disp_apache2.c>
  DispatcherConfig conf/dispatcher.any
  DispatcherLog    logs/dispatcher.log
  DispatcherLogLevel 3
  DispatcherNoServerHeader 0
  DispatcherDeclineRoot 0
  DispatcherUseProcessedURL 0
  DispatcherPassError 0
  DispatcherCertificateFile disp_unified.pem
  DispatcherCACertificateFile cacert.pem
  DispatcherCheckPeerCN On
</IfModule>
```
