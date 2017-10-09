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
# <a name="how-toouse-notification-hubs-from-java"></a><span data-ttu-id="f08a4-103">Hogyan toouse Notification Hubs Java</span><span class="sxs-lookup"><span data-stu-id="f08a4-103">How toouse Notification Hubs from Java</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="f08a4-104">Ez a témakör ismerteti a hello funkciói a hello új teljes mértékben támogatott hivatalos Azure Notification Hub Java SDK-t.</span><span class="sxs-lookup"><span data-stu-id="f08a4-104">This topic describes hello key features of hello new fully supported official Azure Notification Hub Java SDK.</span></span> <span data-ttu-id="f08a4-105">Ez egy nyílt forráskódú projektként, és megtekintheti a hello teljes SDK kódot [Java SDK].</span><span class="sxs-lookup"><span data-stu-id="f08a4-105">This is an open source project and you can view hello entire SDK code at [Java SDK].</span></span> 

<span data-ttu-id="f08a4-106">Általában elérhető összes értesítési központok szolgáltatás Java/PHP/Python vagy Ruby háttér-hello Notification Hub REST interfész használatával hello MSDN-témakörben leírtak szerint [Notification hub REST API-k](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="f08a4-106">In general, you can access all Notification Hubs features from a Java/PHP/Python/Ruby back-end using hello Notification Hub REST interface as described in hello MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span> <span data-ttu-id="f08a4-107">A Java SDK kínál egy vékony Java nyelven, a többi kapcsolatokon.</span><span class="sxs-lookup"><span data-stu-id="f08a4-107">This Java SDK provides a thin wrapper over these REST interfaces in Java.</span></span> 

<span data-ttu-id="f08a4-108">hello SDK támogatja jelenleg:</span><span class="sxs-lookup"><span data-stu-id="f08a4-108">hello SDK supports currently:</span></span>

* <span data-ttu-id="f08a4-109">A Notification hubs használatával CRUD</span><span class="sxs-lookup"><span data-stu-id="f08a4-109">CRUD on Notification Hubs</span></span> 
* <span data-ttu-id="f08a4-110">A regisztráció CRUD</span><span class="sxs-lookup"><span data-stu-id="f08a4-110">CRUD on Registrations</span></span>
* <span data-ttu-id="f08a4-111">Telepítési felügyeleti</span><span class="sxs-lookup"><span data-stu-id="f08a4-111">Installation Management</span></span>
* <span data-ttu-id="f08a4-112">Importálási/exportálási regisztrációk</span><span class="sxs-lookup"><span data-stu-id="f08a4-112">Import/Export Registrations</span></span>
* <span data-ttu-id="f08a4-113">Rendszeresen küld</span><span class="sxs-lookup"><span data-stu-id="f08a4-113">Regular Sends</span></span>
* <span data-ttu-id="f08a4-114">Ütemezett küldése</span><span class="sxs-lookup"><span data-stu-id="f08a4-114">Scheduled Sends</span></span>
* <span data-ttu-id="f08a4-115">Az aszinkron műveletek Java NIO keresztül</span><span class="sxs-lookup"><span data-stu-id="f08a4-115">Async operations via Java NIO</span></span>
* <span data-ttu-id="f08a4-116">Támogatott platformok: APNS (iOS), a GCM (Android), a WNS (a Windows Áruházbeli alkalmazások), a MPNS (Windows Phone), a ADM (Amazon Kindle tűz), a Baidu (Android Google szolgáltatások nélkül)</span><span class="sxs-lookup"><span data-stu-id="f08a4-116">Supported platforms: APNS (iOS), GCM (Android), WNS (Windows Store apps), MPNS(Windows Phone), ADM (Amazon Kindle Fire), Baidu (Android without Google services)</span></span> 

## <a name="sdk-usage"></a><span data-ttu-id="f08a4-117">SDK használata</span><span class="sxs-lookup"><span data-stu-id="f08a4-117">SDK Usage</span></span>
### <a name="compile-and-build"></a><span data-ttu-id="f08a4-118">Fordítási és -buildek</span><span class="sxs-lookup"><span data-stu-id="f08a4-118">Compile and build</span></span>
<span data-ttu-id="f08a4-119">Használjon [Maven]</span><span class="sxs-lookup"><span data-stu-id="f08a4-119">Use [Maven]</span></span>

<span data-ttu-id="f08a4-120">toobuild:</span><span class="sxs-lookup"><span data-stu-id="f08a4-120">toobuild:</span></span>

    mvn package

## <a name="code"></a><span data-ttu-id="f08a4-121">Kód</span><span class="sxs-lookup"><span data-stu-id="f08a4-121">Code</span></span>
### <a name="notification-hub-cruds"></a><span data-ttu-id="f08a4-122">Értesítési központ CRUDs</span><span class="sxs-lookup"><span data-stu-id="f08a4-122">Notification Hub CRUDs</span></span>
<span data-ttu-id="f08a4-123">**Hozzon létre egy NamespaceManager:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-123">**Create a NamespaceManager:**</span></span>

    NamespaceManager namespaceManager = new NamespaceManager("connection string")

<span data-ttu-id="f08a4-124">**Értesítési központ létrehozása:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-124">**Create Notification Hub:**</span></span>

    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);

 <span data-ttu-id="f08a4-125">VAGY</span><span class="sxs-lookup"><span data-stu-id="f08a4-125">OR</span></span>

    hub = new NotificationHub("connection string", "hubname");

<span data-ttu-id="f08a4-126">**Töltse le az értesítési központ:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-126">**Get Notification Hub:**</span></span>

    hub = namespaceManager.getNotificationHub("hubname");

<span data-ttu-id="f08a4-127">**Értesítési központ frissítése:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-127">**Update Notification Hub:**</span></span>

    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);

<span data-ttu-id="f08a4-128">**Értesítési központ törlése:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-128">**Delete Notification Hub:**</span></span>

    namespaceManager.deleteNotificationHub("hubname");

### <a name="registration-cruds"></a><span data-ttu-id="f08a4-129">Regisztrációs CRUDs</span><span class="sxs-lookup"><span data-stu-id="f08a4-129">Registration CRUDs</span></span>
<span data-ttu-id="f08a4-130">**Hozzon létre ügyfél-értesítési központ:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-130">**Create a Notification Hub client:**</span></span>

    hub = new NotificationHub("connection string", "hubname");

<span data-ttu-id="f08a4-131">**Windows regisztrációs létrehozása:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-131">**Create Windows registration:**</span></span>

    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");    
    hub.createRegistration(reg);

<span data-ttu-id="f08a4-132">**IOS-regisztráció létrehozása:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-132">**Create iOS registration:**</span></span>

    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

<span data-ttu-id="f08a4-133">Hasonlóképpen regisztrációk hozhat létre Android (GCM), a Windows Phone (MPNS) és a Kindle tűz (ADM).</span><span class="sxs-lookup"><span data-stu-id="f08a4-133">Similarly you can create registrations for Android (GCM), Windows Phone (MPNS), and Kindle Fire (ADM).</span></span>

<span data-ttu-id="f08a4-134">**Sablon regisztrációk létrehozása:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-134">**Create template registrations:**</span></span>

    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);

<span data-ttu-id="f08a4-135">**A regisztrálás létrehozása registrationid + upsert létre minta**</span><span class="sxs-lookup"><span data-stu-id="f08a4-135">**Create registrations using create registrationid + upsert pattern**</span></span>

<span data-ttu-id="f08a4-136">Eltávolítja az ismétlődő elveszett tooany válaszok miatt, ha a tároló regisztrációs azonosítók hello eszköz:</span><span class="sxs-lookup"><span data-stu-id="f08a4-136">Removes duplicates due tooany lost responses if storing registration ids on hello device:</span></span>

    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);

<span data-ttu-id="f08a4-137">**Regisztráció frissítése:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-137">**Update registrations:**</span></span>

    hub.updateRegistration(reg);

<span data-ttu-id="f08a4-138">**Regisztráció törlése:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-138">**Delete registrations:**</span></span>

    hub.deleteRegistration(regid);

<span data-ttu-id="f08a4-139">**Lekérdezés regisztrációk:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-139">**Query registrations:**</span></span>

* <span data-ttu-id="f08a4-140">**Egyetlen regisztrációs beolvasása:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-140">**Get single registration:**</span></span>
  
    <span data-ttu-id="f08a4-141">hub.getRegistration(regid);</span><span class="sxs-lookup"><span data-stu-id="f08a4-141">hub.getRegistration(regid);</span></span>
* <span data-ttu-id="f08a4-142">**Minden regisztrációk központban érhető el:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-142">**Get all registrations in hub:**</span></span>
  
    <span data-ttu-id="f08a4-143">hub.getRegistrations();</span><span class="sxs-lookup"><span data-stu-id="f08a4-143">hub.getRegistrations();</span></span>
* <span data-ttu-id="f08a4-144">**Töltse le a regisztrációk címkével:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-144">**Get registrations with tag:**</span></span>
  
    <span data-ttu-id="f08a4-145">hub.getRegistrationsByTag("myTag");</span><span class="sxs-lookup"><span data-stu-id="f08a4-145">hub.getRegistrationsByTag("myTag");</span></span>
* <span data-ttu-id="f08a4-146">**Töltse le a regisztrációk csatornánként:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-146">**Get registrations by channel:**</span></span>
  
    <span data-ttu-id="f08a4-147">hub.getRegistrationsByChannel("devicetoken");</span><span class="sxs-lookup"><span data-stu-id="f08a4-147">hub.getRegistrationsByChannel("devicetoken");</span></span>

<span data-ttu-id="f08a4-148">Az összes lekérdezés $top és folytatási jogkivonatokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="f08a4-148">All collection queries support $top and continuation tokens.</span></span>

### <a name="installation-api-usage"></a><span data-ttu-id="f08a4-149">Telepítési API használata</span><span class="sxs-lookup"><span data-stu-id="f08a4-149">Installation API usage</span></span>
<span data-ttu-id="f08a4-150">Telepítés API alternatív módszereket a regisztrációs felügyeleti.</span><span class="sxs-lookup"><span data-stu-id="f08a4-150">Installation API is an alternative mechanism for registration management.</span></span> <span data-ttu-id="f08a4-151">Több regisztrációk, amelyet a rendszer nem triviális könnyen tehetik tévesen vagy töredezetté fenntartása, helyett már lehetséges toouse egy egyetlen telepítés objektum.</span><span class="sxs-lookup"><span data-stu-id="f08a4-151">Instead of maintaining multiple registrations which is not trivial and may be easily done wrongly or inefficiently, it is now possible toouse a SINGLE Installation object.</span></span> <span data-ttu-id="f08a4-152">Telepítés tartalmaz mindent, amire szüksége: csatorna (eszköz jogkivonatát), a címkéket, a sablonok, a másodlagos csempék leküldéses (a wns-ből és APNS).</span><span class="sxs-lookup"><span data-stu-id="f08a4-152">Installation contains everything you need: push channel (device token), tags, templates, secondary tiles (for WNS and APNS).</span></span> <span data-ttu-id="f08a4-153">Toocall hello szolgáltatás tooget azonosítója már nem szükséges – csak hoznak létre GUID vagy bármely más azonosító, tartsa eszköz és küldése tooyour háttér leküldéses csatorna (eszköz jogkivonatát) együtt.</span><span class="sxs-lookup"><span data-stu-id="f08a4-153">You don't need toocall hello service tooget Id anymore - just generate GUID or any other identifier, keep it on device and send tooyour backend together with push channel (device token).</span></span> <span data-ttu-id="f08a4-154">Hello háttérkiszolgálón csak akkor tegye a hívást: CreateOrUpdateInstallation, akkor teljes idempotent, úgy érzi, hogy szabad tooretry szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="f08a4-154">On hello backend you should only do a single call: CreateOrUpdateInstallation, it is fully idempotent, so feel free tooretry if needed.</span></span>

<span data-ttu-id="f08a4-155">Az Amazon Kindle tűz példaként néz ki:</span><span class="sxs-lookup"><span data-stu-id="f08a4-155">As example for Amazon Kindle Fire it looks like this:</span></span>

    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);

<span data-ttu-id="f08a4-156">Ha azt szeretné, tooupdate azt:</span><span class="sxs-lookup"><span data-stu-id="f08a4-156">If you want tooupdate it:</span></span> 

    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);

<span data-ttu-id="f08a4-157">Ha speciális célú tudunk részleges frissítés funkció, amely lehetővé teszi, hogy a toomodify hello telepítési objektum csak adott tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="f08a4-157">For advanced scenarios we have partial update capability which allows toomodify only particular properties of hello installation object.</span></span> <span data-ttu-id="f08a4-158">Alapvetően részleges update egy részhalmazát JSON javítási műveletek telepítési objektum is futtathatók.</span><span class="sxs-lookup"><span data-stu-id="f08a4-158">Basically partial update is subset of JSON Patch operations you can run against Installation object.</span></span>

    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);

<span data-ttu-id="f08a4-159">Telepítés törléséhez:</span><span class="sxs-lookup"><span data-stu-id="f08a4-159">Delete Installation:</span></span>

    hub.deleteInstallation(installation.getInstallationId());

<span data-ttu-id="f08a4-160">CreateOrUpdate, a javítás és a Delete idővel konzisztenssé váljanak a Get.</span><span class="sxs-lookup"><span data-stu-id="f08a4-160">CreateOrUpdate, Patch and Delete are eventually consistent with Get.</span></span> <span data-ttu-id="f08a4-161">A kért művelet csak toohello rendszer sor kerül a hello hívás során, és hátterében végrehajtja.</span><span class="sxs-lookup"><span data-stu-id="f08a4-161">Your requested operation just goes toohello system queue during hello call and will be executed in background.</span></span> <span data-ttu-id="f08a4-162">Vegye figyelembe, hogy a fő futásidejű forgatókönyv, de csak a Hibakeresés és hibaelhárítás céljából nem arra terveztük, Get, hello szolgáltatás szorosan leszabályozza.</span><span class="sxs-lookup"><span data-stu-id="f08a4-162">Note that Get is not designed for main runtime scenario but just for debug and troubleshooting purposes, it is tightly throttled by hello service.</span></span>

<span data-ttu-id="f08a4-163">A regisztrációhoz ugyanaz, mint a van hello telepítések küldési folyamata.</span><span class="sxs-lookup"><span data-stu-id="f08a4-163">Send flow for Installations is hello same as for Registrations.</span></span> <span data-ttu-id="f08a4-164">Jelenleg már csak bevezetett egy beállítás tootarget értesítési toohello adott telepítés – csak használja címke "végrehajtott: {szükségeskonfiguráció-azonosító}".</span><span class="sxs-lookup"><span data-stu-id="f08a4-164">We've just introduced an option tootarget notification toohello particular Installation - just use tag "InstallationId:{desired-id}".</span></span> <span data-ttu-id="f08a4-165">A fenti esetben azt néz ki:</span><span class="sxs-lookup"><span data-stu-id="f08a4-165">For case above it would look like this:</span></span>

    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");

<span data-ttu-id="f08a4-166">A több sablonok valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="f08a4-166">For one of several templates:</span></span>

    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");

### <a name="schedule-notifications-available-for-standard-tier"></a><span data-ttu-id="f08a4-167">(STANDARD csomagban érhető el) értesítések ütemezése</span><span class="sxs-lookup"><span data-stu-id="f08a4-167">Schedule Notifications (available for STANDARD Tier)</span></span>
<span data-ttu-id="f08a4-168">azonos hello rendszeres küldési megegyezik, azonban egy további paraméter - scheduledTime, mely szerint, amikor értesítési kézbesítési.</span><span class="sxs-lookup"><span data-stu-id="f08a4-168">hello same as regular send but with one additional parameter - scheduledTime which says when notification should be delivered.</span></span> <span data-ttu-id="f08a4-169">A szolgáltatás bármely pontján most + 5 perc között eltelt idő elfogadja, most már a + 7 nap.</span><span class="sxs-lookup"><span data-stu-id="f08a4-169">Service accepts any point of time between now + 5 minutes and now + 7 days.</span></span>

<span data-ttu-id="f08a4-170">**A natív Windows értesítési ütemezése:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-170">**Schedule a Windows native notification:**</span></span>

    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);    
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());

### <a name="importexport-available-for-standard-tier"></a><span data-ttu-id="f08a4-171">Importálási/exportálási (STANDARD csomagban érhető el)</span><span class="sxs-lookup"><span data-stu-id="f08a4-171">Import/Export (available for STANDARD Tier)</span></span>
<span data-ttu-id="f08a4-172">Egyes esetekben szükséges tooperform tömeges művelet regisztrációk.</span><span class="sxs-lookup"><span data-stu-id="f08a4-172">Sometimes it is required tooperform bulk operation against registrations.</span></span> <span data-ttu-id="f08a4-173">Általában van egy másik system vagy csak egy nagy javítás toosay frissítés hello címkék való integrációra.</span><span class="sxs-lookup"><span data-stu-id="f08a4-173">Usually it is for integration with another system or just a massive fix toosay update hello tags.</span></span> <span data-ttu-id="f08a4-174">Nem erősen ajánlott toouse Get/frissítési folyamat, ha azt regisztrációk ezer vannak van szó.</span><span class="sxs-lookup"><span data-stu-id="f08a4-174">It is strongly not recommended toouse Get/Update flow if we are talking about thousands of registrations.</span></span> <span data-ttu-id="f08a4-175">Importálási/exportálási képesség a tervezett toocover hello lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f08a4-175">Import/Export capability is designed toocover hello scenario.</span></span> <span data-ttu-id="f08a4-176">Alapvetően olyan hozzáférési toosome blob tároló alatt megadott a tárfiók a bejövő adatok és a kimeneti helyet forrásaként.</span><span class="sxs-lookup"><span data-stu-id="f08a4-176">Basically you provide an access toosome blob container under your storage account as a source of incoming data and location for output.</span></span>

<span data-ttu-id="f08a4-177">**Exportálási feladat elküldése:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-177">**Submit export job:**</span></span>

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);


<span data-ttu-id="f08a4-178">**Importálási feladat elküldése:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-178">**Submit import job:**</span></span>

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);

<span data-ttu-id="f08a4-179">**Várjon, amíg nem történik:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-179">**Wait until job is done:**</span></span>

    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }       

<span data-ttu-id="f08a4-180">**Töltse le az összes feladat:**</span><span class="sxs-lookup"><span data-stu-id="f08a4-180">**Get all jobs:**</span></span>

    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();

<span data-ttu-id="f08a4-181">**SAS-aláírással rendelkező URI:** Ez az hello URL-cím, néhány blob fájl vagy a blob-tároló és az engedélyek és a lejárati idő például paraméterek készletét, valamint aláírás fiók SAS-kulcs használatával létrehozott összes lépést.</span><span class="sxs-lookup"><span data-stu-id="f08a4-181">**URI with SAS signature:** This is hello URL of some blob file or blob container plus set of parameters like permissions and expiration time plus signature of all these things made using account's SAS key.</span></span> <span data-ttu-id="f08a4-182">Az Azure Storage Java SDK rendelkezik sokoldalú képességeit, beleértve az ilyen típusú URI-azonosítók létrehozását.</span><span class="sxs-lookup"><span data-stu-id="f08a4-182">Azure Storage Java SDK has rich capabilities including creation of such kind of URIs.</span></span> <span data-ttu-id="f08a4-183">Egyszerű alternatív ImportExportE2E teszt osztály (helyről hello github) amely aláírási algoritmus nagyon egyszerű és kompakt végrehajtásának egy pillantást is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="f08a4-183">As simple alternative you can take a look at ImportExportE2E test class (from hello github location) which has very basic and compact implementation of signing algorithm.</span></span>

### <a name="send-notifications"></a><span data-ttu-id="f08a4-184">Értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="f08a4-184">Send Notifications</span></span>
<span data-ttu-id="f08a4-185">hello értesítési objektum nagyon egyszerűen a fejlécek törzs, néhány segédprogram-metódusokat hello natív és a sablon értesítések objektumok létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="f08a4-185">hello Notification object is simply a body with headers, some utility methods help in building hello native and template notifications objects.</span></span>

* <span data-ttu-id="f08a4-186">**Windows áruház és Windows Phone 8.1 (nem Silverlight)**</span><span class="sxs-lookup"><span data-stu-id="f08a4-186">**Windows Store and Windows Phone 8.1 (non-Silverlight)**</span></span>
  
        String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
        Notification n = Notification.createWindowsNotification(toast);
        hub.sendNotification(n);
* <span data-ttu-id="f08a4-187">**iOS**</span><span class="sxs-lookup"><span data-stu-id="f08a4-187">**iOS**</span></span>
  
        String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
        Notification n = Notification.createAppleNotification(alert);
        hub.sendNotification(n);
* <span data-ttu-id="f08a4-188">**Android**</span><span class="sxs-lookup"><span data-stu-id="f08a4-188">**Android**</span></span>
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createGcmNotification(message);
        hub.sendNotification(n);
* <span data-ttu-id="f08a4-189">**Windows Phone 8.0-s és 8.1 Silverlight**</span><span class="sxs-lookup"><span data-stu-id="f08a4-189">**Windows Phone 8.0 and 8.1 Silverlight**</span></span>
  
        String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>Hello from Java!</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
        Notification n = Notification.createMpnsNotification(toast);
        hub.sendNotification(n);
* <span data-ttu-id="f08a4-190">**Tűz kindle**</span><span class="sxs-lookup"><span data-stu-id="f08a4-190">**Kindle Fire**</span></span>
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createAdmNotification(message);
        hub.sendNotification(n);
* <span data-ttu-id="f08a4-191">**TooTags küldése**</span><span class="sxs-lookup"><span data-stu-id="f08a4-191">**Send tooTags**</span></span>
  
        Set<String> tags = new HashSet<String>();
        tags.add("boo");
        tags.add("foo");
        hub.sendNotification(n, tags);
* <span data-ttu-id="f08a4-192">**Tootag kifejezés küldése**</span><span class="sxs-lookup"><span data-stu-id="f08a4-192">**Send tootag expression**</span></span>       
  
        hub.sendNotification(n, "foo && ! bar");
* <span data-ttu-id="f08a4-193">**Sablon értesítés küldése**</span><span class="sxs-lookup"><span data-stu-id="f08a4-193">**Send template notification**</span></span>
  
        Map<String, String> prop =  new HashMap<String, String>();
        prop.put("prop1", "v1");
        prop.put("prop2", "v2");
        Notification n = Notification.createTemplateNotification(prop);
        hub.sendNotification(n);

<span data-ttu-id="f08a4-194">A Java kódja most kell előállítania egy értesítés jelenik meg a céleszközön.</span><span class="sxs-lookup"><span data-stu-id="f08a4-194">Running your Java code should now produce a notification appearing on your target device.</span></span>

## <span data-ttu-id="f08a4-195"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f08a4-195"><a name="next-steps"></a>Next Steps</span></span>
<span data-ttu-id="f08a4-196">Ebben a témakörben azt bemutatta, hogyan toocreate egy egyszerű Java REST-a Notification Hubs-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="f08a4-196">In this topic we showed how toocreate a simple Java REST client for Notification Hubs.</span></span> <span data-ttu-id="f08a4-197">Itt a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="f08a4-197">From here you can:</span></span>

* <span data-ttu-id="f08a4-198">Töltse le a teljes hello [Java SDK], amely hello teljes SDK kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f08a4-198">Download hello full [Java SDK], which contains hello entire SDK code.</span></span> 
* <span data-ttu-id="f08a4-199">Lejátszás a hello minták:</span><span class="sxs-lookup"><span data-stu-id="f08a4-199">Play with hello samples:</span></span>
  * <span data-ttu-id="f08a4-200">[Ismerkedés a Notification hubs használatával]</span><span class="sxs-lookup"><span data-stu-id="f08a4-200">[Get Started with Notification Hubs]</span></span>
  * <span data-ttu-id="f08a4-201">[Legfrissebb hírek elküldése]</span><span class="sxs-lookup"><span data-stu-id="f08a4-201">[Send breaking news]</span></span>
  * <span data-ttu-id="f08a4-202">[Honosított legfrissebb hírek elküldése]</span><span class="sxs-lookup"><span data-stu-id="f08a4-202">[Send localized breaking news]</span></span>
  * <span data-ttu-id="f08a4-203">[Értesítések küldése tooauthenticated felhasználók]</span><span class="sxs-lookup"><span data-stu-id="f08a4-203">[Send notifications tooauthenticated users]</span></span>
  * <span data-ttu-id="f08a4-204">[Platformok közötti értesítések küldéséhez felhasználók tooauthenticated]</span><span class="sxs-lookup"><span data-stu-id="f08a4-204">[Send cross-platform notifications tooauthenticated users]</span></span>

[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend
[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Ismerkedés a Notification hubs használatával]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/
[Legfrissebb hírek elküldése]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/
[Honosított legfrissebb hírek elküldése]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/
[Értesítések küldése tooauthenticated felhasználók]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/
[Platformok közötti értesítések küldéséhez felhasználók tooauthenticated]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/
[Maven]: http://maven.apache.org/

