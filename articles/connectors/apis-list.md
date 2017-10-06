---
title: az Azure Logic Apps aaaConnectors |} Microsoft Docs
description: "Minden hello érhető el a Microsoft által felügyelt összekötők toobuild lehetőségek közül választhat, és a logic apps létrehozása"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: f1f1fd50-b7f9-4d13-824a-39678619aa7a
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: mandia; ladocs
ms.openlocfilehash: d681d13d642e6e1512d1f8ab0e1078a194b5da83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connectors-list"></a>Összekötők listája
> [!TIP]
> Hello [teljes listáját A-Z](#az) (az ebben a témakörben) felsorolja az összes hello rendelkezésre álló összekötők Logic Apps alkalmazásait is használhatja. [Connector részleteket](/connectors/) bármely eseményindítók és hello swagger definiált műveletek sorolja fel, és is felsorolja az egyes összekötő semmilyen határnak.

Az összekötők a logikai alkalmazások létrehozásának szerves részei. Az összekötők, segítségével valóban bontsa ki a helyszíni és felhőalapú alkalmazások toodo különböző fogalom az Ön által létrehozott adatok és a már adatokkal. a következő kategóriák hello hello összekötők használhatók: 

* **Standard összekötők**: Automatikusan elérhetők és részei a logikai alkalmazásoknak. Néhány példa: Service Bus, Power BI, Oracle Database, OneDrive stb.

* **Integrációsfiók-összekötők**: Akkor érhetők el, ha integrációs fiókot vásárol. Ezen összekötők használatával átalakíthatja és érvényesítheti az XML-fájlokat, vállalatok közötti üzeneteket dolgozhat fel az AS2/X12/EDIFACT használatával, illetve egybesimított fájlokat kódolhat és dekódolhat. BizTalk Server használata esetén az összekötők, egy jó sorolhatók tooexpand a BizTalk munkafolyamatok Azure-e.  

    BizTalk Server is rendelkezik egy [Logic Apps adapter](https://msdn.microsoft.com/library/mt787163.aspx) , amely tartalmazza a logikai alkalmazás fogad, és küld tooa logikai alkalmazást.

* **Vállalati összekötők**: Tartalmazza az MQ-t és az SAP-t. További költségekkel érhető el. 

[Logic Apps árképzési](https://azure.microsoft.com/pricing/details/logic-apps/) és [árazás modell](../logic-apps/logic-apps-pricing.md) hello költségek további részletekkel szolgálnak. 

## <a name="popular-connectors"></a>Népszerű összekötők
Több ezer olyan alkalmazás és több millió olyan végrehajtás létezik, amely ezekkel az összekötőkkel dolgozott fel sikeresen adatokat és információkat. hello a következő táblázat sorolja fel, a legnépszerűbb hello és néhány Kedvencek a felhasználókkal:

| |  |  |  |
| --- | --- | --- | --- |
| [![API Icon][AzureBlobStorageicon]<br/>**Azure Blob<br/>Storage**][AzureBlobStoragedoc] | Ha azt szeretné tooautomate bármely feladatok a storage-fiók, majd meg kell tekintse meg ezt az összekötőt. Támogatja a CRUD (create – létrehozás, read – olvasás, update – frissítés, delete – törlés) műveleteket. | [![API Icon][Azure-Functionsicon]<br/>**Azure Functions**][azure-functionsdoc] | Létrehozhat C#- vagy node.js-környezetben készített, egyedi kódrészleteket futtatni képes függvényeket, majd logikai alkalmazásaiban felhasználhatja ezeket.  |
| [![API Icon][Dynamics-365icon]<br/>**Dynamics 365<br/>CRM Online**][Dynamics-365doc] | Az egyik leggyakrabban ismételt összekötők hello. Eseményindítók és műveletek toohelp érdeklődők, és több munkafolyamatainak automatizálásához rendelkezik. | [![API Icon][Event-Hubs-icon]<br/>**Event Hubs**][event-hubs-doc] | Az Event Hubon eseményeket használhat fel és tehet közzé. Például kimeneti beszerezni a Logic Apps alkalmazást, az Event Hubs használatával, és elküldheti-e hello kimeneti tooa valós idejű elemzési szolgáltató. |
| [![API Icon][FTPicon]<br/>**FTP**][FTPdoc] | Ha az FTP-kiszolgáló elérhető-e hello internet, majd a fájlok és mappák munkafolyamatok toowork automatizálható. <br/><br/>SFTP hello SFTP-összekötőn keresztül is érhető el. | [![API Icon][HTTPicon]<br/>**HTTP**][httpdoc] | Logic apps toocommunicate bármely végponttal használja a HTTP Protokollon keresztül. |
| [![API Icon][Office-365-Outlookicon]<br/>**Office 365<br/>Outlook**][office365-outlookdoc] | Nagy mennyiségű eseményindítók, és sok más műveletek toouse Office 365 e-mailek és a munkafolyamatok belül események. <br/><br/>Ez az összekötő tartalmazza egy *jóváhagyási e-mail* műveleti tooapprove szabadsága kérelmeire, jelentések költség, és így tovább. <br/><br/>Office 365-felhasználók is elérhetők hello Office 365-felhasználók összekötőn keresztül.| [![API Icon][HTTP-Requesticon]<br/>**Request/Response**][HTTP-Requestdoc] | Ez az összekötő HTTPS URL-címet biztosít. Hello logikai alkalmazás egy kérelem toothis URL-címet kap, amikor elindítja a hello logikai alkalmazás. |
| [![API Icon][Salesforceicon]<br/>**Salesforce**][salesforcedoc] | Könnyen jelentkezzen be a Salesforce fiók tooget hozzáférés tooobjects, például érdeklődők, és így tovább. |  [![API Icon][Service-Busicon]<br/>**Service Bus**][Service-Busdoc] | hello legnépszerűbb összekötő logic Apps, az eseményindítók és műveletek toodo aszinkron üzenetkezelési és a várólisták, előfizetések és témakörök közzétételi/előfizetési tartalmazza. |
|  [![API Icon][SharePointicon]<br/>**SharePoint<br/>Online**][SharePointdoc] | Ha olyan jellegű munkát végez a SharePointtal, amely során hasznos lenne az automatizálás, javasoljuk, hogy tekintse meg ezt az összekötőt. A helyszíni SharePointtal és a SharePoint Online-nal is használható. | [![API Icon][SQL-Servericon]<br/>**SQL Server**][SQL-Serverdoc] | Hello egyik leggyakrabban használt összekötők, hogy tooan csatlakozni a helyszíni SQL Server és az Azure SQL Database. | 
| [![API Icon][Twittericon]<br/>**Twitter**][Twitterdoc] | Könnyedén bejelentkezhet Twitter-fiók használatával, majd minden új Twitter-üzenet közzététele után elindíthat egy munkafolyamatot. Ezután mentse a Twitter-üzeneteket tooa SQL-adatbázis vagy a SharePoint-listát. | | | 

## <a name="integration-account-connectors"></a>Integrációs fiókok összekötői 

hello vállalati Integration Pack (EIP-t a) tartalmaz, amelyek a jól ismert toohello BizTalk Server közösségi összekötők. Ha vásárol egy [integrációs fiók](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), összekötők a következő hello is elérhetővé: 

|  |  |  |  |
| --- | --- | --- | --- |
| [![API Icon][as2icon]<br/>**AS2</br> decoding**][as2decode] | [![API Icon][as2icon]<br/>**AS2</br> encoding**][as2encode] | [![API Icon][x12icon]<br/>**EDIFACT</br> decoding**][EDIFACTdecode] | [![API Icon][x12icon]<br/>**EDIFACT</br> encoding**][EDIFACTencode] |
[![API Icon][flatfileicon]<br/>**Flat file</br> encoding**][flatfiledoc] | [![API Icon][flatfiledecodeicon]<br/>**Flat file</br> decoding**][flatfiledecodedoc] | [![API Icon][integrationaccounticon]<br/>**Integration<br/>account**][integrationaccountdoc] | [![API Icon][xmltransformicon]<br/>**Transform<br/>XML**][xmltransformdoc] |
| [![API Icon][x12icon]<br/>**X12</br> decoding**][x12decode] | [![API Icon][x12icon]<br/>**X12</br> encoding**][x12encode] | [![API Icon][xmlvalidateicon]<br/>**XML <br/>validation**][xmlvalidatedoc] | |

## <a name="enterprise-connectors"></a>Vállalati összekötők

Csatlakozás tooyour vállalati alkalmazások a logic apps szolgáltatásban.

|  |  |
| --- | --- |
|[![API-ikon][MQicon]<br/>**MQ**][mqdoc]|[![API Icon][SAPicon]<br/>**SAP**][sapconnector]|


## <a name="az"></a>Betűrend szerinti teljes lista

[Connector részleteket](/connectors/) bármely eseményindítók és hello swagger definiált műveletek listában, és semmilyen határnak minden összekötőhöz is tartalmazza.

| | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [**1**](#1) | [**A**](#a) | [**B**](#b) | [**C**](#c) | [**D**](#d) | [**E**](#e) | [**F**](#f) | [**G**](#g) | [**H**](#h) | [**I**](#i) | [**J**](#j) | [**L**](#l) | [**M**](#m) |
| [**N**](#n) | [**O**](#o) | [**P**](#p) | [**R**](#r) | [**S**](#s) | [**T**](#t) | [**U**](#u) | [**V**](#v) | [**W**](#w) | [**X**](#x) | [**Y**](#y) | [**Z**](#z) | | 

| | |
|---|---|
|<a name="1"></a>10to8 Appointment Scheduling<br/><br/><a name="a"></a>Act!<br/>Adobe Creative Cloud<br/>appFigures<br/>[AS2][as2doc]<br/>Asana<br/>Azure Active Directory (AD)<br/>Azure API Management<br/>Azure App Services<br/>Azure-alkalmazás<br/>Azure Automation<br/>[Azure Blob Storage][azureblobstoragedoc]<br/>Azure Data Lake<br/>Azure DocumentDB (Cosmos DB)<br/>[Azure Functions][azure-functionsdoc]<br/>[Azure Logic Apps][nested-logic-appdoc]<br/>AzureML<br/>Azure Queues<br/>Azure Resource Manager<br/>[Azure SQL Database][sql-serverdoc]<br/><br/><a name="b"></a>Basecamp 2<br/>Basecamp 3<br/>Batch<br/>Benchmark Email<br/>Bing kereső<br/>Bitbucket<br/>Bitly<br/>BizTalk Server<br/>Blogger<br/>Box<br/>Buffer<br/><br/><a name="c"></a>Calendly<br/>Campfire<br/>Capsule CRM<br/>Chatter<br/>Cognito Forms<br/>Cognitive Services számítógépes látástechnológiai API<br/>Cognitive Services arcfelismerési API<br/>Cognitive Services LUIS<br/>Cognitive Services szövegelemzés<br/>Common Data Service<br/>Content Conversion<br/>Control-Terminate<br/>[Egyéni API-k/webappok][api/web-appdoc]<br/><br/><a name="d"></a>Adatműveletek<br/>[DB2][db2doc]<br/>Disqus<br/>DocuSign<br/>Do Until<br/>Dropbox<br/>[Dynamics 365 CRM Online][Dynamics-365doc]<br/>Dynamics 365 for Financials<br/>Dynamics 365 for Operations<br/>Dynamics NAV<br/><br/><a name="e"></a>Easy Redmine<br/>EDIFACT<br/>[Event Hubs][event-hubs-doc]<br/>Eventbrite<br/><br/><a name="f"></a>Facebook<br/>[Fájlrendszer][filesystemdoc]<br/>[Egybesimított fájl][flatfiledoc]<br/>FreshBooks<br/>Freshdesk<br/>Freshservice<br/>[FTP][ftpdoc]<br/><br/><a name="g"></a>GitHub<br/>Gmail<br/>Google Naptár<br/>Google Névjegyek<br/>Google Drive<br/>Google Táblázatok<br/>Google Teendők<br/>GoToMeeting<br/>GoToTraining<br/>GoToWebinar<br/><br/><a name="h"></a>Harvest<br/>HelloSign<br/>HipChat<br/>[HTTP][httpdoc]<br/>[HTTP + Swagger][http-swaggerdoc]<br/>[HTTP Webhook][webhookdoc]<br/><br/><a name="i"></a>[Informix][informixdoc]<br/>Infusionsoft<br/>Inoreader<br/>Insightly<br/>Instagram<br/>Instapaper<br/>Integrációs fiók<br/>Intercom | <a name="j"></a>JotForm<br/>JIRA<br/><br/><a name="l"></a>LeanKit<br/>LiveChat<br/><br/><a name="m"></a>MailChimp<br/>Mandrill<br/>Közepes<br/>Microsoft Forms<br/>Microsoft Teams<br/>Microsoft Translator<br/>[MQ][mqdoc]<br/>MSN Időjárás<br/>Muhimbi PDF<br/>MySQL<br/><br/><a name="n"></a>Nexmo<br/><br/><a name="o"></a>[Office 365 Outlook][office365-outlookdoc]<br/>Office 365 Users<br/>Office 365 Video<br/>OneDrive<br/>OneDrive Vállalati verzió<br/>OneNote (vállalati)<br/>[Oracle Database][oracle-db-doc]<br/>Outlook Customer Manager<br/>Outlook Tasks<br/>Outlook.com<br/><br/><a name="p"></a>PagerDuty<br/>Parserr<br/>Paylocity<br/>Pinterest<br/>Pipedrive<br/>Pivotal Tracker<br/>Planner<br/>PostgreSQL<br/>Power BI<br/>Project Online<br/><br/><a name="r"></a>Redmine<br/>[Request/Response][http-requestdoc]<br/>RSS<br/><br/><a name="s"></a>[Salesforce][salesforcedoc]<br/>[SAP alkalmazáskiszolgáló][sapconnector]<br/>[SAP üzenetkiszolgáló][sapconnector]<br/>[Ütemezés][recurrencedoc]<br/>Hatókör<br/>SendGrid<br/>Üzenetek toobatch küldése<br/>[Szolgáltatásbusz][service-busdoc]<br/>SFTP<br/>[SharePoint Online][sharepointdoc]<br/>[SharePoint Server][sharepointserver]<br/>Slack<br/>Smartsheet<br/>SMTP<br/>SparkPost<br/>[SQL Server][sql-serverdoc]<br/>Stripe<br/>SurveyMonkey<br/>Switch Case<br/><br/><a name="t"></a>Teamwork Projects<br/>Teradata<br/>Todoist<br/>Toodledo<br/>[XML-átalakítás][xmltransformdoc]<br/>Trello<br/>Twilio<br/>[Twitter][twitterdoc]<br/>Typeform<br/><br/><a name="u"></a>UserVoice<br/><br/><a name="v"></a>Változók<br/>Vimeo<br/>Visual Studio Team Services<br/><br/><a name="w"></a>WebMerge<br/>WordPress<br/>Wunderlist<br/><br/><a name="x"></a>[X12][x12doc]<br/>[XML-érvényesítés][xmlvalidatedoc]<br/><br/><a name="y"></a>Yammer<br/>YouTube<br/><br/><a name="z"></a>Zendesk |

> [!TIP]
> t az Azure-fiók regisztrálása előtt az Azure Logic Apps lépései tooget lépjen túl[próbálja Logic Apps](https://tryappservice.azure.com/?appservice=logic). Azonnal létrehozhat egy rövid életű, kezdő szintű logikai alkalmazást. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.

## <a name="connectors-as-triggers-and-actions"></a>Eseményindítóként és műveletként használható összekötők

Egy **eseményindító** elindul, vagy lefuttatja a logikai alkalmazása egy példányát. Egyes összekötők eseményindítókat biztosítanak, amelyek adott események bekövetkezése esetén értesítik az alkalmazást. Például hello FTP összekötő rendelkezik-e hello `OnUpdatedFile` eseményindító, amely a logikai alkalmazás elindítja egy fájl frissítésekor. 

Logikai alkalmazások közé tartoznak az eseményindító-típus a következő hello:  

* *Kérdezze le az eseményindítók*: ezek az eseményindítók kérdezze le a szolgáltatást a megadott gyakorisággal toocheck, az új adatokat. 

    Új adatok nem érhető el, a logikai alkalmazás új példánya fut hello adatok bemeneti adatként. 

* *Leküldéses eseményindítók*: ezek az eseményindítók figyelje az adatok egy végponton, illetve egy adott esemény toohappen, majd elindítja a Logic Apps alkalmazást egy új példányát.

* *Ismétlődés eseményindító*: ez az eseményindító előre meghatározott ütemezés szerint létrehozza a logikai alkalmazás egy példányát.

Az összekötők **műveleteket** is biztosítanak, amelyek felhasználhatók a munkafolyamatban. A logikai alkalmazás például adatokat kereshet ki, amelyeket Ön később felhasználhat a logikai alkalmazásban. Pontosabban ügyféladatok kereshet meg egy SQL-adatbázisból, és majd használni a munkafolyamat az a felhasználói adatok toobuild. 

> [!TIP]
> Az [összekötők áttekintését](connectors-overview.md) nyújtó témakörben további részleteket olvashat az eseményindítókról és a műveletekről. 


## <a name="message-manipulation-actions"></a>Üzenetkezelési műveletek

A logikai alkalmazások beépített műveleteket tartalmaznak, amelyek a hasznos adatok módosítására vagy kezelésére szolgálnak. beépített hello **adatok műveletek** az összekötő tartalmazza a következő műveletek hello: 

| | |
|---|---|
| **Összeállítás** | Build értékek vagy objektumok toouse készítése később, vagy, ha Ön a munkafolyamat létrehozása. Például egy JSON-objektum több lépést értékeivel hozhatnak létre, vagy egy későbbi részében futtatása logikai alkalmazás állandó tooreference kiszámításához. |
| **CSV-táblázat létrehozása**<br/>**HTML-táblázat létrehozása** | Egy tömb eredménykészletét átviheti egy CSV- vagy HTML-táblázatba. Például hello CRM "Lista rekordok" műveletet, és adjon hozzá egy szűrőt a ma hozzáadott rekordok. Hello eredmények, majd küldje el e-mailben egy HTML-táblában. |
| **Tömb szűrése** (lekérdezés) | Egy eredmény set toohello tételek megkeresheti az Önt érdeklő szűréséhez. Például keresse meg az összes Twitter-üzeneteket `#Azure`, és majd "szűrő" hello visszaadott Twitter-üzeneteket, amelyek tooonly ad találatot `Tweeted_by_followers > 50`. |
| **Csatlakozás** | Tömböt csatlakoztathat valamilyen elválasztóval. Például a kulcs kifejezések észlelése művelet hello legfontosabb kifejezések tömbjét adja vissza. Ezeket „csatlakoztathatja” egy `,` vagy hasonló karakter segítségével. Tehát `["Some", "Phrase"]` helyett `"Some, Phrase"` értékkel rendelkezik majd. |
| **JSON elemzése** | Kimenő elemezni, és hozzáférési értékek hello Designer JSON-objektumból. Például az Azure-függvény visszatérési értéke, egy JSON-adattartalmat, majd meg tudja értelmezni az tooaccess hello JSON tulajdonságok később egy másik lépésben. hello művelet is ellenőrzi, hogy hello JSON egyezések hello megadott séma futásidőben. | 
| **Kiválasztás** | Kiválaszthatja egy tömb bizonyos tulajdonságait további feldolgozás céljából. Ha az SQL-ben a „Rekordok listába rendezése” parancs 15 oszlopot ad vissza, akkor kiválaszthat ezek közül néhányat a további feldolgozás érdekében. hello eredménye olyan tömb, amely csak a kiválasztott hello tulajdonságait tartalmazza. |

## <a name="custom-connectors-and-azure-certification"></a>Egyéni összekötők és Azure-hitelesítés 

az API-t egyéni kódra, vagy nem érhetők el összekötők toocall, hello Logic Apps platform által kiterjesztheti [létrehozása REST-alapú API-alkalmazások egyéni összekötők](../logic-apps/logic-apps-create-api-app.md). 

Ha azt szeretné, toomake az egyéni API Apps nyilvános és a rendelkezésre álló toouse az Azure-ban, majd küldje el a jelölések toohello [hivatalos Microsoft Azure program](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/).

## <a name="get-help"></a>Segítségkérés

tooask kérdéseket, válasz kérdések és milyen egyéb Azure Logic Apps felhasználók végzi, lásd: Nyissa meg toohello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp Azure Logic Apps alkalmazások és összekötők javítása, szavazhatnak, vagy küldje el a következő hello ötleteket [Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).

Nem talál valamilyen összekötővel kapcsolatos témakört, illetve fontosnak tartott részletet? Ha igen, segítsen tooour meglévő témakörök hozzáadásával, vagy saját írása. Dokumentációnk nyílt forrás, amelyet a GitHubon találhat. Első lépések a [GitHub-adattárban](https://github.com/Microsoft/azure-docs). 

## <a name="next-steps"></a>Következő lépések
* [Az első logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md)
* [Egyéni API-k létrehozása logikai alkalmazásokhoz](../logic-apps/logic-apps-create-api-app.md)
* [Logikai alkalmazások figyelése](../logic-apps/logic-apps-monitor-your-logic-apps.md)

<!--Connectors Documentation-->

[api/web-appdoc]: ../logic-apps/logic-apps-custom-hosted-api.md "Logikai appok integrálása App Service API Apps-alkalmazásokkal"
[azureblobstoragedoc]: ./connectors-create-api-azureblobstorage.md "A blobtárolókban található fájlok kezelése az Azure Blob Storage-összekötővel"
[azure-functionsdoc]: ../logic-apps/logic-apps-azure-functions.md "Logikai alkalmazások integrálása Azure Functions-függvényekkel"
[db2doc]: ./connectors-create-api-db2.md "Csatlakoztassa a hello felhőalapú vagy helyszíni DB2 tooIBM. Sorokat frissíthet, táblákat kérhet le, és egyéb műveleteket is végrehajthat"
[Dynamics-365doc]: ./connectors-create-api-crmonline.md "Csatlakozzon a CRM Online tooDynamics, használhatja CRM Online-adatokhoz"
[event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Az Event Hubs tooAzure csatlakozzon. Eseményeket fogadhat és küldhet a logikai alkalmazások és az Event Hubs között"
[filesystemdoc]: ../logic-apps/logic-apps-using-file-connector.md "Csatlakozás a helyi fájlrendszer tooan"
[ftpdoc]: ./connectors-create-api-ftp.md "Csatlakozás tooan FTP / FTPS-kiszolgálókhoz FTP-műveleteket, például a feltöltését, az első, fájlok törlése"
[httpdoc]: ./connectors-native-http.md "HTTP-hívások hello HTTP-összekötőn keresztül"
[http-requestdoc]: ./connectors-native-reqres.md "A HTTP kérelmeit és válaszait tooyour a logic apps-műveletek"
[http-swaggerdoc]: ./connectors-native-http-swagger.md "HTTP-hívások indítása HTTP + Swagger összekötővel"
[informixdoc]: ./connectors-create-api-informix.md "Csatlakozás tooInformix hello felhőalapú vagy helyszíni. Egy sort, lista hello táblák és további olvasása"
[nested-logic-appdoc]: ../logic-apps/logic-apps-http-endpoint.md "Logikai alkalmazások integrálása beágyazott munkafolyamatokkal"
[office365-outlookdoc]: ./connectors-create-api-office365-outlook.md "Csatlakozás tooyour Office 365-fiók. E-maileket küldhet és fogadhat, kezelheti a naptárát és névjegyeit, és egyéb műveleteket is végrehajthat"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Csatlakozás tooan Oracle adatbázis tooadd, insert, törölje a sorokat és több"
[mqdoc]: ./connectors-create-api-mq.md "Csatlakozás helyszíni tooMQ vagy Azure, és üzeneteket küldjön és fogadjon"
[recurrencedoc]:  ./connectors-native-recurrence.md "Ismétlődő műveletek kiváltása logikai alkalmazásokhoz"
[salesforcedoc]: ./connectors-create-api-salesforce.md "Csatlakozás tooyour Salesforce-fiókhoz. Kezelheti fiókjait, az érdeklődőket, üzleti lehetőségeit és egyéb elemeket"
[sapconnector]: ../logic-apps/logic-apps-using-sap-connector.md "Csatlakozás tooan helyszíni SAP rendszer"
[service-busdoc]: ./connectors-create-api-servicebus.md "Üzeneteket küldhet a Service Bus-üzenetsorokból és -témakörökből, valamint fogadhatja a Service Bus-üzenetsorok és -előfizetések üzeneteit."
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "Csatlakozás tooSharePoint Online. Dokumentumokat kezelhet, elemeket listázhat és egyéb műveleteket is végrehajthat"
[sharepointserver]: ./connectors-create-api-sharepointserver.md "Csatlakozás a helyi kiszolgálón tooSharePoint. Dokumentumokat kezelhet, elemeket listázhat és egyéb műveleteket is végrehajthat"
[sql-serverdoc]: ./connectors-create-api-sqlazure.md "Csatlakozás SQL Database tooAzure vagy SQL server. SQL-adatbázistáblában szereplő bejegyzéseket hozhat létre, frissíthet, kérhet le és törölhet."
[twitterdoc]: ./connectors-create-api-twitter.md "Csatlakozás tooTwitter. Idővonal-tartalmakat fogadhat, tweeteket tehet közzé, és egyéb műveleteket is végrehajthat"
[webhookdoc]: ./connectors-native-webhook.md "Webhook műveletek és eseményindítók tooyour logic Apps alkalmazások hozzáadása"

[as2doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Megismerheti a vállalati integrációs AS2-t."
[x12doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Megismerheti a vállalati integrációs X12-t."
[flatfiledoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Megismerheti a vállalati integrációs egybesimított fájlt."
[flatfiledecodedoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Megismerheti a vállalati integrációs egybesimított fájlt."
[xmlvalidatedoc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Megismerheti a vállalati integrációs XML-hitelesítést."
[xmltransformdoc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Megismerheti a vállalati integrációs átalakításokat."
[as2decode]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Megismerheti a vállalati integrációs AS2-dekódolást."
[as2encode]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Megismerheti a vállalati integrációs AS2-kódolást."
[X12decode]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Megismerheti a vállalati integrációs X12-dekódolást."
[X12encode]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Megismerheti a vállalati integrációs X12-kódolást."
[EDIFACTdecode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Megismerheti a vállalati integrációs EDIFACT-dekódolást."
[EDIFACTencode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Megismerheti a vállalati integrációs EDIFACT-kódolást."
[integrationaccountdoc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Sémákat, térképeket, partnereket és egyebeket kereshet ki az integrációs fiókjában"


[boxDoc]: ./connectors-create-api-box.md "Csatlakozás tooBox. Fájlokat tölthet fel, kérhet le, törölhet, listázhat, és egyéb műveleteket is végrehajthat"
[delaydoc]: ./connectors-native-delay.md "Késleltetett műveletek végrehajtása"
[dropboxdoc]: ./connectors-create-api-dropbox.md "Csatlakozás tooDropbox. Fájlokat tölthet fel, kérhet le, törölhet, listázhat, és egyéb műveleteket is végrehajthat"
[facebookdoc]: ./connectors-create-api-facebook.md "Csatlakozás tooFacebook. Tooa ütemterv utáni, a hírcsatorna lap, és több"
[githubdoc]: ./connectors-create-api-github.md "Csatlakozás tooGitHub és problémák nyomon követése"
[google-drivedoc]: ./connectors-create-api-googledrive.md "Csatlakozzon a tooGoogleDrive, az adatok dolgozhat"
[google-sheetsdoc]: ./connectors-create-api-googlesheet.md "Csatlakozás tooGoogle lapok, így is módosíthatja a smartsheetet"
[google-tasksdoc]: ./connectors-create-api-googletasks.md "TooGoogle feladatok csatlakozik, így kezelhetők a feladatok"
[google-calendardoc]: ./connectors-create-api-googlecalendar.md "Naptár tooGoogle kapcsolódik, és kezelheti a naptárban."
[http-responsedoc]: ./connectors-native-reqres.md "A HTTP kérelmeit és válaszait tooyour a logic apps-műveletek"
[instagramdoc]: ./connectors-create-api-instagram.md "Csatlakozás tooInstagram. Eseményeket indíthat vagy reagálhat rájuk"
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "Csatlakozás tooyour MailChimp-fiók. Kezelheti és automatizálhatja e-mailjeit"
[mandrilldoc]: ./connectors-create-api-mandrill.md "Csatlakozás tooMandrill kommunikációhoz"
[microsoft-translatordoc]: ./connectors-create-api-microsofttranslator.md "Csatlakozás tooMicrosoft fordító. Szövegeket fordíthat, nyelveket ismerhet fel, és egyéb műveleteket is végrehajthat" 
[office365-usersdoc]: ./connectors-create-api-office365-users.md 
[office365-videodoc]: ./connectors-create-api-office365-video.md "Videoadatokat, videólistákat és csatornákat kérhet le, valamint Office 365-videók lejátszási URL-címeit"
[onedrivedoc]: ./connectors-create-api-onedrive.md "Csatlakozás tooyour személyes Microsoft OneDrive. Fájlokat tölthet fel, törölhet, listázhat, és egyéb műveleteket is elvégezhet"
[onedrive-for-businessdoc]: ./connectors-create-api-onedriveforbusiness.md "Csatlakozás a Microsoft OneDrive tooyour üzleti. Fájlokat tölthet fel, törölhet, listázhat, és egyéb műveleteket is elvégezhet"
[outlook.comdoc]: ./connectors-create-api-outlook.md "Csatlakozás tooyour Outlook-postaládájához. Kezelheti az e-mailjeit, naptárait, névjegyeit és egyéb tartalmait"
[project-onlinedoc]: ./connectors-create-api-projectonline.md "Csatlakoztassa a Project Online tooMicrosoft. Kezelheti a projektjeit, feladatait, erőforrásait és egyéb tartalmait"
[querydoc]: ./connectors-native-query.md "Válassza ki, és hello lekérdezés művelettel tömbök szűrése"
[rssdoc]: ./connectors-create-api-rss.md "Közzétételét és lekérhetnek hírcsatornaelemeket, műveletek indul el, ha egy új cikk közzétett tooan RSS hírcsatorna."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "Csatlakozás tooSendGrid. E-maileket küldhet, és kezelheti a címzettek listáit"
[sftpdoc]: ./connectors-create-api-sftp.md "Csatlakozás tooyour SFTP fiók. Fájlokat tölthet fel, kérhet le, törölhet, és egyéb műveleteket is végrehajthat"
[slackdoc]: ./connectors-create-api-slack.md "Csatlakozás tooSlack, illetve üzenetek tooSlack csatornák"
[smtpdoc]: ./connectors-create-api-smtp.md "Csatlakozás tooa SMTP-kiszolgáló és mellékleteket tartalmazó e-mailek küldése"
[sparkpostdoc]: ./connectors-create-api-sparkpost.md "Kommunikáció tooSparkPost csatlakozik"
[trellodoc]: ./connectors-create-api-trello.md "Csatlakozás tooTrello. Kezelheti a projektjeit és bármit és bárkivel megszervezhet"
[twiliodoc]: ./connectors-create-api-twilio.md "Csatlakozás tooTwilio. Üzeneteket küldhet, lekérheti az elérhető számokat, kezelheti a bejövő hívások telefonszámait, és egyéb műveleteket is végrehajthat"
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Csatlakozás tooWunderlist. Kezelheti a feladatait és teendőlistáit, szinkronizálhatja különböző tartalmait, és egyéb műveleteket is végrehajthat"
[yammerdoc]: ./connectors-create-api-yammer.md "Csatlakozás tooYammer. Üzenetek tehet közzé, új üzeneteket fogadhat, és egyéb műveleteket is végrehajthat"
[youtubedoc]: ./connectors-create-api-youtube.md "Csatlakozás tooYouTube. Kezelheti a videóit és csatornáit"


<!--Icon references-->
[appFiguresicon]: ./media/apis-list/appfigures.png
[Asanaicon]: ./media/apis-list/asana.png
[Azure-Automation-icon]: ./media/apis-list/azure-automation.png
[AzureBlobStorageicon]: ./media/apis-list/azureblob.png
[Azure-Data-Lake-icon]: ./media/apis-list/azure-data-lake.png
[Azure-DocumentDBicon]: ./media/apis-list/azure-documentdb.png
[Azure-MLicon]: ./media/apis-list/azureml.png
[Azure-Resource-Manager-icon]: ./media/apis-list/azure-resource-manager.png
[Azure-Queues-icon]: ./media/apis-list/azure-queues.png
[Basecamp-3icon]: ./media/apis-list/basecamp.png
[Bitbucket-icon]: ./media/apis-list/bitbucket.png
[Bitlyicon]: ./media/apis-list/bitly.png
[BizTalk-Servericon]: ./media/apis-list/biztalk.png
[Bloggericon]: ./media/apis-list/blogger.png
[Boxicon]: ./media/apis-list/box.png
[Campfireicon]: ./media/apis-list/campfire.png
[Cognitive-Services-Text-Analyticsicon]: ./media/apis-list/cognitiveservicestextanalytics.png
[DB2icon]: ./media/apis-list/db2.png
[Dropboxicon]: ./media/apis-list/dropbox.png
[Dynamics-365icon]: ./media/apis-list/dynamicscrmonline.png
[Dynamics-365-for-Financialsicon]: ./media/apis-list/madeira.png
[Dynamics-365-for-Operationsicon]: ./media/apis-list/dynamicsax.png
[Easy-Redmineicon]: ./media/apis-list/easyredmine.png
[Event-Hubs-icon]: ./media/apis-list/eventhubs.png
[Facebookicon]: ./media/apis-list/facebook.png
[FTPicon]: ./media/apis-list/ftp.png
[GitHubicon]: ./media/apis-list/github.png
[Google-Calendaricon]: ./media/apis-list/googlecalendar.png
[Google-Driveicon]: ./media/apis-list/googledrive.png
[Google-Sheetsicon]: ./media/apis-list/googlesheet.png
[Google-Tasksicon]: ./media/apis-list/googletasks.png
[HideKeyicon]: ./media/apis-list/hidekey.png
[HipChaticon]: ./media/apis-list/hipchat.png
[Informixicon]: ./media/apis-list/informix.png
[Insightlyicon]: ./media/apis-list/insightly.png
[Instagramicon]: ./media/apis-list/instagram.png
[Instapapericon]: ./media/apis-list/instapaper.png
[JIRAicon]: ./media/apis-list/jira.png
[MailChimpicon]: ./media/apis-list/mailchimp.png
[Mandrillicon]: ./media/apis-list/mandrill.png
[Microsoft-Translatoricon]: ./media/apis-list/microsofttranslator.png
[MQicon]: ./media/apis-list/mq.png
[Office-365-Outlookicon]: ./media/apis-list/office365.png
[Office-365-Usersicon]: ./media/apis-list/office365users.png
[Office-365-Videoicon]: ./media/apis-list/office365video.png
[OneDriveicon]: ./media/apis-list/onedrive.png
[OneDrive-for-Businessicon]: ./media/apis-list/onedriveforbusiness.png
[Oracle-DB-icon]: ./media/apis-list/oracle-db.png
[Outlook.comicon]: ./media/apis-list/outlook.png
[PagerDutyicon]: ./media/apis-list/pagerduty.png
[Pinteresticon]: ./media/apis-list/pinterest.png
[Project-Onlineicon]: ./media/apis-list/projectonline.png
[Redmineicon]: ./media/apis-list/redmine.png
[RSSicon]: ./media/apis-list/rss.png
[Common-Data-Serviceicon]: ./media/apis-list/runtimeservice.png
[Salesforceicon]: ./media/apis-list/salesforce.png
[SAPicon]: ./media/apis-list/sap.png
[SendGridicon]: ./media/apis-list/sendgrid.png
[Service-Busicon]: ./media/apis-list/servicebus.png
[SFTPicon]: ./media/apis-list/sftp.png
[SharePointicon]: ./media/apis-list/sharepointonline.png
[Slackicon]: ./media/apis-list/slack.png
[Smartsheeticon]: ./media/apis-list/smartsheet.png
[SMTPicon]: ./media/apis-list/smtp.png
[SparkPosticon]: ./media/apis-list/sparkpost.png
[SQL-Servericon]: ./media/apis-list/sql.png
[Todoisticon]: ./media/apis-list/todoist.png
[Trelloicon]: ./media/apis-list/trello.png
[Twilioicon]: ./media/apis-list/twilio.png
[Twittericon]: ./media/apis-list/twitter.png
[Vimeoicon]: ./media/apis-list/vimeo.png
[Visual-Studio-Team-Servicesicon]: ./media/apis-list/visualstudioteamservices.png
[WordPressicon]: ./media/apis-list/wordpress.png
[Wunderlisticon]: ./media/apis-list/wunderlist.png
[Yammericon]: ./media/apis-list/yammer.png
[YouTubeicon]: ./media/apis-list/youtube.png

<!-- Primitive Icons -->
[API/Web-Appicon]: ./media/apis-list/api.png
[Azure-Functionsicon]: ./media/apis-list/function.png
[Delayicon]: ./media/apis-list/delay.png
[FileSystemIcon]: ./media/apis-list/filesystem.png
[HTTPicon]: ./media/apis-list/http.png
[HTTP-Requesticon]: ./media/apis-list/request.png
[HTTP-Responseicon]: ./media/apis-list/response.png
[HTTP-Swaggericon]: ./media/apis-list/http_swagger.png
[Nested-Logic-Appicon]: ./media/apis-list/workflow.png
[Recurrenceicon]: ./media/apis-list/recurrence.png
[Queryicon]: ./media/apis-list/query.png
[Webhookicon]: ./media/apis-list/webhook.png

<!-- EIP Icons -->
[as2icon]: ./media/apis-list/as2.png
[x12icon]: ./media/apis-list/x12new.png
[flatfileicon]: ./media/apis-list/flatfileencoding.png
[flatfiledecodeicon]: ./media/apis-list/flatfiledecoding.png
[xmlvalidateicon]: ./media/apis-list/xmlvalidation.png
[xmltransformicon]: ./media/apis-list/xsltransform.png
[integrationaccounticon]: ./media/apis-list/integrationaccount.png
