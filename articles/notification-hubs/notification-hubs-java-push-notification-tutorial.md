---
title: a Notification Hubs Java aaaHow toouse
description: "Megtudhatja, hogyan toouse Azure Notification Hubs Java háttér-a."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 4c3f966d-0158-4a48-b949-9fa3666cb7e4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: java
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: afcf305b1acd9ee28ee4889040ece59d9399d29d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-notification-hubs-from-java"></a>Hogyan toouse Notification Hubs Java
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Ez a témakör ismerteti a hello funkciói a hello új teljes mértékben támogatott hivatalos Azure Notification Hub Java SDK-t. Ez egy nyílt forráskódú projektként, és megtekintheti a hello teljes SDK kódot [Java SDK]. 

Általában elérhető összes értesítési központok szolgáltatás Java/PHP/Python vagy Ruby háttér-hello Notification Hub REST interfész használatával hello MSDN-témakörben leírtak szerint [Notification hub REST API-k](http://msdn.microsoft.com/library/dn223264.aspx). A Java SDK kínál egy vékony Java nyelven, a többi kapcsolatokon. 

hello SDK támogatja jelenleg:

* A Notification hubs használatával CRUD 
* A regisztráció CRUD
* Telepítési felügyeleti
* Importálási/exportálási regisztrációk
* Rendszeresen küld
* Ütemezett küldése
* Az aszinkron műveletek Java NIO keresztül
* Támogatott platformok: APNS (iOS), a GCM (Android), a WNS (a Windows Áruházbeli alkalmazások), a MPNS (Windows Phone), a ADM (Amazon Kindle tűz), a Baidu (Android Google szolgáltatások nélkül) 

## <a name="sdk-usage"></a>SDK használata
### <a name="compile-and-build"></a>Fordítási és -buildek
Használjon [Maven]

toobuild:

    mvn package

## <a name="code"></a>Kód
### <a name="notification-hub-cruds"></a>Értesítési központ CRUDs
**Hozzon létre egy NamespaceManager:**

    NamespaceManager namespaceManager = new NamespaceManager("connection string")

**Értesítési központ létrehozása:**

    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);

 VAGY

    hub = new NotificationHub("connection string", "hubname");

**Töltse le az értesítési központ:**

    hub = namespaceManager.getNotificationHub("hubname");

**Értesítési központ frissítése:**

    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);

**Értesítési központ törlése:**

    namespaceManager.deleteNotificationHub("hubname");

### <a name="registration-cruds"></a>Regisztrációs CRUDs
**Hozzon létre ügyfél-értesítési központ:**

    hub = new NotificationHub("connection string", "hubname");

**Windows regisztrációs létrehozása:**

    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");    
    hub.createRegistration(reg);

**IOS-regisztráció létrehozása:**

    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

Hasonlóképpen regisztrációk hozhat létre Android (GCM), a Windows Phone (MPNS) és a Kindle tűz (ADM).

**Sablon regisztrációk létrehozása:**

    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);

**A regisztrálás létrehozása registrationid + upsert létre minta**

Eltávolítja az ismétlődő elveszett tooany válaszok miatt, ha a tároló regisztrációs azonosítók hello eszköz:

    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);

**Regisztráció frissítése:**

    hub.updateRegistration(reg);

**Regisztráció törlése:**

    hub.deleteRegistration(regid);

**Lekérdezés regisztrációk:**

* **Egyetlen regisztrációs beolvasása:**
  
    hub.getRegistration(regid);
* **Minden regisztrációk központban érhető el:**
  
    hub.getRegistrations();
* **Töltse le a regisztrációk címkével:**
  
    hub.getRegistrationsByTag("myTag");
* **Töltse le a regisztrációk csatornánként:**
  
    hub.getRegistrationsByChannel("devicetoken");

Az összes lekérdezés $top és folytatási jogkivonatokat támogatja.

### <a name="installation-api-usage"></a>Telepítési API használata
Telepítés API alternatív módszereket a regisztrációs felügyeleti. Több regisztrációk, amelyet a rendszer nem triviális könnyen tehetik tévesen vagy töredezetté fenntartása, helyett már lehetséges toouse egy egyetlen telepítés objektum. Telepítés tartalmaz mindent, amire szüksége: csatorna (eszköz jogkivonatát), a címkéket, a sablonok, a másodlagos csempék leküldéses (a wns-ből és APNS). Toocall hello szolgáltatás tooget azonosítója már nem szükséges – csak hoznak létre GUID vagy bármely más azonosító, tartsa eszköz és küldése tooyour háttér leküldéses csatorna (eszköz jogkivonatát) együtt. Hello háttérkiszolgálón csak akkor tegye a hívást: CreateOrUpdateInstallation, akkor teljes idempotent, úgy érzi, hogy szabad tooretry szükség esetén.

Az Amazon Kindle tűz példaként néz ki:

    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);

Ha azt szeretné, tooupdate azt: 

    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);

Ha speciális célú tudunk részleges frissítés funkció, amely lehetővé teszi, hogy a toomodify hello telepítési objektum csak adott tulajdonságait. Alapvetően részleges update egy részhalmazát JSON javítási műveletek telepítési objektum is futtathatók.

    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);

Telepítés törléséhez:

    hub.deleteInstallation(installation.getInstallationId());

CreateOrUpdate, a javítás és a Delete idővel konzisztenssé váljanak a Get. A kért művelet csak toohello rendszer sor kerül a hello hívás során, és hátterében végrehajtja. Vegye figyelembe, hogy a fő futásidejű forgatókönyv, de csak a Hibakeresés és hibaelhárítás céljából nem arra terveztük, Get, hello szolgáltatás szorosan leszabályozza.

A regisztrációhoz ugyanaz, mint a van hello telepítések küldési folyamata. Jelenleg már csak bevezetett egy beállítás tootarget értesítési toohello adott telepítés – csak használja címke "végrehajtott: {szükségeskonfiguráció-azonosító}". A fenti esetben azt néz ki:

    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");

A több sablonok valamelyikét:

    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");

### <a name="schedule-notifications-available-for-standard-tier"></a>(STANDARD csomagban érhető el) értesítések ütemezése
azonos hello rendszeres küldési megegyezik, azonban egy további paraméter - scheduledTime, mely szerint, amikor értesítési kézbesítési. A szolgáltatás bármely pontján most + 5 perc között eltelt idő elfogadja, most már a + 7 nap.

**A natív Windows értesítési ütemezése:**

    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);    
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());

### <a name="importexport-available-for-standard-tier"></a>Importálási/exportálási (STANDARD csomagban érhető el)
Egyes esetekben szükséges tooperform tömeges művelet regisztrációk. Általában van egy másik system vagy csak egy nagy javítás toosay frissítés hello címkék való integrációra. Nem erősen ajánlott toouse Get/frissítési folyamat, ha azt regisztrációk ezer vannak van szó. Importálási/exportálási képesség a tervezett toocover hello lehetőséget. Alapvetően olyan hozzáférési toosome blob tároló alatt megadott a tárfiók a bejövő adatok és a kimeneti helyet forrásaként.

**Exportálási feladat elküldése:**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);


**Importálási feladat elküldése:**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);

**Várjon, amíg nem történik:**

    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }       

**Töltse le az összes feladat:**

    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();

**SAS-aláírással rendelkező URI:** Ez az hello URL-cím, néhány blob fájl vagy a blob-tároló és az engedélyek és a lejárati idő például paraméterek készletét, valamint aláírás fiók SAS-kulcs használatával létrehozott összes lépést. Az Azure Storage Java SDK rendelkezik sokoldalú képességeit, beleértve az ilyen típusú URI-azonosítók létrehozását. Egyszerű alternatív ImportExportE2E teszt osztály (helyről hello github) amely aláírási algoritmus nagyon egyszerű és kompakt végrehajtásának egy pillantást is igénybe vehet.

### <a name="send-notifications"></a>Értesítések küldése
hello értesítési objektum nagyon egyszerűen a fejlécek törzs, néhány segédprogram-metódusokat hello natív és a sablon értesítések objektumok létrehozásakor.

* **Windows áruház és Windows Phone 8.1 (nem Silverlight)**
  
        String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
        Notification n = Notification.createWindowsNotification(toast);
        hub.sendNotification(n);
* **iOS**
  
        String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
        Notification n = Notification.createAppleNotification(alert);
        hub.sendNotification(n);
* **Android**
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createGcmNotification(message);
        hub.sendNotification(n);
* **Windows Phone 8.0-s és 8.1 Silverlight**
  
        String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>Hello from Java!</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
        Notification n = Notification.createMpnsNotification(toast);
        hub.sendNotification(n);
* **Tűz kindle**
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createAdmNotification(message);
        hub.sendNotification(n);
* **TooTags küldése**
  
        Set<String> tags = new HashSet<String>();
        tags.add("boo");
        tags.add("foo");
        hub.sendNotification(n, tags);
* **Tootag kifejezés küldése**       
  
        hub.sendNotification(n, "foo && ! bar");
* **Sablon értesítés küldése**
  
        Map<String, String> prop =  new HashMap<String, String>();
        prop.put("prop1", "v1");
        prop.put("prop2", "v2");
        Notification n = Notification.createTemplateNotification(prop);
        hub.sendNotification(n);

A Java kódja most kell előállítania egy értesítés jelenik meg a céleszközön.

## <a name="next-steps"></a>Következő lépések
Ebben a témakörben azt bemutatta, hogyan toocreate egy egyszerű Java REST-a Notification Hubs-ügyfél. Itt a következőket teheti:

* Töltse le a teljes hello [Java SDK], amely hello teljes SDK kódját tartalmazza. 
* Lejátszás a hello minták:
  * [Ismerkedés a Notification hubs használatával]
  * [Legfrissebb hírek elküldése]
  * [Honosított legfrissebb hírek elküldése]
  * [Értesítések küldése tooauthenticated felhasználók]
  * [Platformok közötti értesítések küldéséhez felhasználók tooauthenticated]

[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend
[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Ismerkedés a Notification hubs használatával]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/
[Legfrissebb hírek elküldése]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/
[Honosított legfrissebb hírek elküldése]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/
[Értesítések küldése tooauthenticated felhasználók]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/
[Platformok közötti értesítések küldéséhez felhasználók tooauthenticated]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/
[Maven]: http://maven.apache.org/

