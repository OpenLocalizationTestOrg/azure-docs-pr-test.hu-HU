---
title: "aaaUse tooback REST-be, és állítsa vissza az App Service-alkalmazásokhoz"
description: "Megtudhatja, hogyan toouse RESTful API meghívja a tooback és visszaállítása egy alkalmazást az Azure App Service"
services: app-service
documentationcenter: 
author: NKing92
manager: erikre
editor: 
ms.assetid: f94d0aea-edc1-43ab-9b51-ea7375859276
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2016
ms.author: nicking
ms.openlocfilehash: f77bdfc7de1626d04d8d0c0e8979231ae1ced81a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-rest-tooback-up-and-restore-app-service-apps"></a><span data-ttu-id="68f80-103">REST tooback használja, és állítsa vissza az App Service-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="68f80-103">Use REST tooback up and restore App Service apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="68f80-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="68f80-104">PowerShell</span></span>](../app-service/app-service-powershell-backup.md)
> * [<span data-ttu-id="68f80-105">REST API</span><span class="sxs-lookup"><span data-stu-id="68f80-105">REST API</span></span>](websites-csm-backup.md)
> 
> 

<span data-ttu-id="68f80-106">[App Service apps](https://azure.microsoft.com/services/app-service/web/) , az Azure-tárfiókba blobok készíthető.</span><span class="sxs-lookup"><span data-stu-id="68f80-106">[App Service apps](https://azure.microsoft.com/services/app-service/web/) can be backed up as blobs in Azure storage.</span></span> <span data-ttu-id="68f80-107">hello biztonsági mentés hello app adatbázisokat is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="68f80-107">hello backup can also contain hello app’s databases.</span></span> <span data-ttu-id="68f80-108">Ha hello alkalmazást legalább egyszer véletlenül törlik, vagy ha hello alkalmazás igényeinek toobe visszaállít tooa korábbi verziót, visszaállítható a korábbi biztonsági mentésből.</span><span class="sxs-lookup"><span data-stu-id="68f80-108">If hello app is ever accidentally deleted, or if hello app needs toobe reverted tooa previous version, it can be restored from any previous backup.</span></span> <span data-ttu-id="68f80-109">Igény szerint bármikor végezhető biztonsági mentések, vagy a biztonsági mentés ütemezése megfelelő időközönként.</span><span class="sxs-lookup"><span data-stu-id="68f80-109">Backups can be done at any time on demand, or backups can be scheduled at suitable intervals.</span></span>

<span data-ttu-id="68f80-110">Ez a cikk azt ismerteti, hogyan toobackup és visszaállítása egy alkalmazás RESTful API-t kér.</span><span class="sxs-lookup"><span data-stu-id="68f80-110">This article explains how toobackup and restore an app with RESTful API requests.</span></span> <span data-ttu-id="68f80-111">Ha például a toocreate és alkalmazás biztonsági mentéseit grafikusan hello Azure-portálon keresztül, lásd: [készítsen biztonsági másolatot egy webalkalmazást az Azure App Service-ben](web-sites-backup.md)</span><span class="sxs-lookup"><span data-stu-id="68f80-111">If you would like toocreate and manage app backups graphically through hello Azure portal, see [Back up a web app in Azure App Service](web-sites-backup.md)</span></span>

<a name="gettingstarted"></a>

## <a name="getting-started"></a><span data-ttu-id="68f80-112">Első lépések</span><span class="sxs-lookup"><span data-stu-id="68f80-112">Getting Started</span></span>
<span data-ttu-id="68f80-113">toosend REST kér, az alkalmazás kell tooknow **neve**, **erőforráscsoport**, és **előfizetés-azonosító**. Ezek az információk találhatók az alkalmazást a hello kattintva **App Service** panelen található hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="68f80-113">toosend REST requests, you need tooknow your app’s **name**, **resource group**, and **subscription id**. This information can be found by clicking your app in hello **App Service** blade of hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="68f80-114">A cikkben szereplő példák hello, hogy megadja hello webhely **backuprestoreapiexamples.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="68f80-114">For hello examples in this article, we are configuring hello website **backuprestoreapiexamples.azurewebsites.net**.</span></span> <span data-ttu-id="68f80-115">Hello alapértelmezett-webalkalmazás-WestUS erőforráscsoport tárolja, és fut a hello azonosító 00001111-2222-3333-4444-555566667777 rendelkező előfizetés.</span><span class="sxs-lookup"><span data-stu-id="68f80-115">It is stored in hello Default-Web-WestUS resource group and is running on a subscription with hello ID 00001111-2222-3333-4444-555566667777.</span></span>

![A minta webhely-információk][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>

## <a name="backup-and-restore-rest-api"></a><span data-ttu-id="68f80-117">Biztonsági mentés és visszaállítás REST API-n</span><span class="sxs-lookup"><span data-stu-id="68f80-117">Backup and restore REST API</span></span>
<span data-ttu-id="68f80-118">Most bemutatjuk, hogyan toouse hello REST API toobackup, és állítsa vissza az alkalmazás számos példát.</span><span class="sxs-lookup"><span data-stu-id="68f80-118">We will now cover several examples of how toouse hello REST API toobackup and restore an app.</span></span> <span data-ttu-id="68f80-119">Minden például tartalmaz egy URL-cím és a HTTP-kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="68f80-119">Each example includes a URL and HTTP request body.</span></span> <span data-ttu-id="68f80-120">hello minta URL-cím kapcsos zárójelek, például {előfizetés-azonosító} becsomagolt helyőrzők tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="68f80-120">hello sample URL contains placeholders wrapped in curly braces, such as {subscription-id}.</span></span> <span data-ttu-id="68f80-121">Hello helyőrzőket cserélje le hello megfelelő információkat az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="68f80-121">Replace hello placeholders with hello corresponding information for your app.</span></span> <span data-ttu-id="68f80-122">Összehasonlításul Ez minden helyőrző hello példa URL-címek megjelenő magyarázata.</span><span class="sxs-lookup"><span data-stu-id="68f80-122">For reference, here is an explanation of each placeholder that appears in hello example URLs.</span></span>

* <span data-ttu-id="68f80-123">előfizetés-azonosító – hello Azure-előfizetés tartalmazó hello alkalmazás azonosítója</span><span class="sxs-lookup"><span data-stu-id="68f80-123">subscription-id – ID of hello Azure subscription containing hello app</span></span>
* <span data-ttu-id="68f80-124">erőforrás-csoportnevet – hello erőforrás csoportot tartalmazó hello alkalmazás nevét</span><span class="sxs-lookup"><span data-stu-id="68f80-124">resource-group-name – Name of hello resource group containing hello app</span></span>
* <span data-ttu-id="68f80-125">név – hello alkalmazás nevét</span><span class="sxs-lookup"><span data-stu-id="68f80-125">name – Name of hello app</span></span>
* <span data-ttu-id="68f80-126">biztonsági mentés-id – hello alkalmazás biztonsági azonosítója</span><span class="sxs-lookup"><span data-stu-id="68f80-126">backup-id – ID of hello app backup</span></span>

<span data-ttu-id="68f80-127">További hello hello API teljes dokumentációját, beleértve néhány választható paraméterek: hello HTTP kérelemben szereplő: hello [Azure erőforrás-kezelő](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="68f80-127">For hello complete documentation of hello API, including several optional parameters that can be included in hello HTTP request, see hello [Azure Resource Explorer](https://resources.azure.com/).</span></span>

<a name="backup-on-demand"></a>

## <a name="backup-an-app-on-demand"></a><span data-ttu-id="68f80-128">Egy alkalmazásét az igény szerinti biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="68f80-128">Backup an app on demand</span></span>
<span data-ttu-id="68f80-129">egy alkalmazás tooback azonnal, küldjön egy **POST** kérelem túl**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ biztonsági mentési /**.</span><span class="sxs-lookup"><span data-stu-id="68f80-129">tooback up an app immediately, send a **POST** request too**https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.</span></span>

<span data-ttu-id="68f80-130">Ez hello URL-cím néz a példa webhelyről.</span><span class="sxs-lookup"><span data-stu-id="68f80-130">Here is what hello URL looks like using our example website.</span></span> <span data-ttu-id="68f80-131">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**</span><span class="sxs-lookup"><span data-stu-id="68f80-131">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backup/**</span></span>

<span data-ttu-id="68f80-132">Adjon meg egy JSON-objektum, a kérelem toospecify, mely tárolási fiók toouse toostore hello biztonsági mentési hello törzsében.</span><span class="sxs-lookup"><span data-stu-id="68f80-132">Supply a JSON object in hello body of your request toospecify which storage account toouse toostore hello backup.</span></span> <span data-ttu-id="68f80-133">hello JSON-objektumból, rendelkeznie kell nevű tulajdonság **storageAccountUrl**, mely tartalmazza a [SAS URL-cím](../storage/common/storage-dotnet-shared-access-signature-part-1.md) írási toohello Azure tároló, amely tárolja a biztonsági mentési blokkblob hello megadása.</span><span class="sxs-lookup"><span data-stu-id="68f80-133">hello JSON object must have a property named **storageAccountUrl**, which holds a [SAS URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) granting write access toohello Azure Storage container that holds hello backup blob.</span></span> <span data-ttu-id="68f80-134">Ha tooback másolatot az adatbázisokról, meg kell adnia egy hello nevek, típusok és a biztonsági mentés hello adatbázisok toobe kapcsolati karakterláncokat tartalmazó lista is.</span><span class="sxs-lookup"><span data-stu-id="68f80-134">If you want tooback up your databases, you must also supply a list containing hello names, types, and connection strings of hello databases toobe backed up.</span></span>

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

<span data-ttu-id="68f80-135">Hello alkalmazás biztonsági másolatot azonnal hello kérelem érkezik elindul.</span><span class="sxs-lookup"><span data-stu-id="68f80-135">A backup of hello app begins immediately when hello request is received.</span></span> <span data-ttu-id="68f80-136">hello biztonsági mentési folyamat eltarthat egy hosszú ideig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="68f80-136">hello backup process may take a long time toocomplete.</span></span> <span data-ttu-id="68f80-137">hello HTTP-válasz egy Azonosítót, amely segítségével is hello biztonsági mentés egy másik kérelem toosee hello állapotát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="68f80-137">hello HTTP response contains an ID that you can use in another request toosee hello status of hello backup.</span></span> <span data-ttu-id="68f80-138">Itt található példa hello hello HTTP-válasz tooour biztonsági mentési kérés törzsét.</span><span class="sxs-lookup"><span data-stu-id="68f80-138">Here is an example of hello body of hello HTTP response tooour backup request.</span></span>

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

> [!NOTE]
> <span data-ttu-id="68f80-139">Hibaüzenetek hello HTTP-válasz hello napló tulajdonságában találhatók.</span><span class="sxs-lookup"><span data-stu-id="68f80-139">Error messages can be found in hello log property of hello HTTP response.</span></span>
> 
> 

<a name="schedule-automatic-backups"></a>

## <a name="schedule-automatic-backups"></a><span data-ttu-id="68f80-140">Az automatikus biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="68f80-140">Schedule automatic backups</span></span>
<span data-ttu-id="68f80-141">A hozzáadása toobacking be egy alkalmazást az igény szerinti azt is beállíthatja a biztonsági mentési toohappen automatikusan.</span><span class="sxs-lookup"><span data-stu-id="68f80-141">In addition toobacking up an app on demand, you can also schedule a backup toohappen automatically.</span></span>

### <a name="set-up-a-new-automatic-backup-schedule"></a><span data-ttu-id="68f80-142">Állítson be egy új automatikus biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="68f80-142">Set up a new automatic backup schedule</span></span>
<span data-ttu-id="68f80-143">biztonsági mentés ütemezését, tooset küldése egy **PUT** kérelem túl**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config és biztonsági mentési**.</span><span class="sxs-lookup"><span data-stu-id="68f80-143">tooset up a backup schedule, send a **PUT** request too**https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.</span></span>

<span data-ttu-id="68f80-144">Ez hello URL-cím néz a példában a webhelyhez.</span><span class="sxs-lookup"><span data-stu-id="68f80-144">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="68f80-145">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**</span><span class="sxs-lookup"><span data-stu-id="68f80-145">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup**</span></span>

<span data-ttu-id="68f80-146">hello kérelemtörzset rendelkeznie kell egy JSON-objektum hello biztonsági mentési konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="68f80-146">hello request body must have a JSON object that specifies hello backup configuration.</span></span> <span data-ttu-id="68f80-147">Íme egy példa, és az összes szükséges hello paramétert.</span><span class="sxs-lookup"><span data-stu-id="68f80-147">Here is an example with all hello required parameters.</span></span>

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set tootrue tooenable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

<span data-ttu-id="68f80-148">Ez a példa hello app toobe automatikusan biztonsági másolat hét naponta konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="68f80-148">This example configures hello app toobe automatically backed up every seven days.</span></span> <span data-ttu-id="68f80-149">paraméterek hello **frequencyInterval** és **frequencyUnit** együtt határozzák meg, milyen gyakran hello biztonsági mentések fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="68f80-149">hello parameters **frequencyInterval** and **frequencyUnit** together determine how often hello backups happen.</span></span> <span data-ttu-id="68f80-150">Az érvényes értékek **frequencyUnit** vannak **óra** és **nap**.</span><span class="sxs-lookup"><span data-stu-id="68f80-150">Valid values for **frequencyUnit** are **hour** and **day**.</span></span> <span data-ttu-id="68f80-151">Ha például egy alkalmazás 12 óránként tooback frequencyInterval too12 és frequencyUnit toohour beállítása.</span><span class="sxs-lookup"><span data-stu-id="68f80-151">For example, tooback up an app every 12 hours, set frequencyInterval too12 and frequencyUnit toohour.</span></span>

<span data-ttu-id="68f80-152">Régi biztonsági másolatok hello tárfiók automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="68f80-152">Old backups are automatically removed from hello storage account.</span></span> <span data-ttu-id="68f80-153">Megadhatja a biztonsági mentések hello beállítása is lehet, hogy régi hello **retentionPeriodInDays** paraméter.</span><span class="sxs-lookup"><span data-stu-id="68f80-153">You can control how old hello backups can be by setting hello **retentionPeriodInDays** parameter.</span></span> <span data-ttu-id="68f80-154">Ha azt szeretné, hogy a tooalways rendelkezik legalább egy biztonsági mentése, függetlenül attól, hogy régi, beállítás **keepAtLeastOneBackup** tootrue.</span><span class="sxs-lookup"><span data-stu-id="68f80-154">If you want tooalways have at least one backup saved, regardless of how old it is, set **keepAtLeastOneBackup** tootrue.</span></span>

### <a name="get-hello-automatic-backup-schedule"></a><span data-ttu-id="68f80-155">Automatikus biztonsági mentés ütemezése hello beolvasása</span><span class="sxs-lookup"><span data-stu-id="68f80-155">Get hello automatic backup schedule</span></span>
<span data-ttu-id="68f80-156">tooget alkalmazás biztonsági mentési konfiguráció, küldjön egy **POST** kérelem toohello URL-CÍMÉT **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ webhelyek / {name} / config/biztonsági mentés/lista**.</span><span class="sxs-lookup"><span data-stu-id="68f80-156">tooget an app’s backup configuration, send a **POST** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.</span></span>

<span data-ttu-id="68f80-157">a hely URL-címe hello **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.</span><span class="sxs-lookup"><span data-stu-id="68f80-157">hello URL for our example site is **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.</span></span>

<a name="get-backup-status"></a>

## <a name="get-hello-status-of-a-backup"></a><span data-ttu-id="68f80-158">A biztonsági mentés hello állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="68f80-158">Get hello status of a backup</span></span>
<span data-ttu-id="68f80-159">Attól függően, hogy milyen nagy hello alkalmazás, a biztonsági mentés eltarthat egy ideig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="68f80-159">Depending on how large hello app is, a backup may take a while toocomplete.</span></span> <span data-ttu-id="68f80-160">Biztonsági mentések is sikertelenek, túllépi az időkorlátot, előfordulhat, hogy, vagy részben sikeres.</span><span class="sxs-lookup"><span data-stu-id="68f80-160">Backups might also fail, time out, or partially succeed.</span></span> <span data-ttu-id="68f80-161">az alkalmazások biztonsági mentések toosee hello állapot küldése egy **beolvasása** kérelem toohello URL-CÍMÉT **https://management.azure.com/subscriptions/ {előfizetés-azonosító} /resourceGroups/ {csoport-erőforrásnév} /providers/ Microsoft.Web/sites/{name}/backups**.</span><span class="sxs-lookup"><span data-stu-id="68f80-161">toosee hello status of all an app’s backups, send a **GET** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.</span></span>

<span data-ttu-id="68f80-162">toosee hello állapotát egy biztonsági másolat, küldjön egy GET kérelem toohello URL-címe **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/ { biztonsági mentés-azonosító}**.</span><span class="sxs-lookup"><span data-stu-id="68f80-162">toosee hello status of a specific backup, send a GET request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.</span></span>

<span data-ttu-id="68f80-163">Ez hello URL-cím néz a példában a webhelyhez.</span><span class="sxs-lookup"><span data-stu-id="68f80-163">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="68f80-164">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**</span><span class="sxs-lookup"><span data-stu-id="68f80-164">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**</span></span>

<span data-ttu-id="68f80-165">hello adott válasz törzsének egy JSON objektum hasonló toothis példa tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="68f80-165">hello response body contains a JSON object similar toothis example.</span></span>

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

<span data-ttu-id="68f80-166">hello biztonsági állapota egy felsorolt típus.</span><span class="sxs-lookup"><span data-stu-id="68f80-166">hello status of a backup is an enumerated type.</span></span> <span data-ttu-id="68f80-167">Ez minden lehetséges állapota.</span><span class="sxs-lookup"><span data-stu-id="68f80-167">Here is every possible state.</span></span>

* <span data-ttu-id="68f80-168">0 – esetbejegyzések: hello biztonsági mentés elindult, de még nem fejeződött be.</span><span class="sxs-lookup"><span data-stu-id="68f80-168">0 – InProgress: hello backup has been started but has not yet completed.</span></span>
* <span data-ttu-id="68f80-169">1 – nem sikerült: hello biztonsági mentés sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="68f80-169">1 – Failed: hello backup was unsuccessful.</span></span>
* <span data-ttu-id="68f80-170">2 – sikeres: hello biztonsági mentése sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="68f80-170">2 – Succeeded: hello backup completed successfully.</span></span>
* <span data-ttu-id="68f80-171">3 – időtúllépésbe került: hello biztonsági mentés nem fejeződött be időben, és meg lett szakítva.</span><span class="sxs-lookup"><span data-stu-id="68f80-171">3 – TimedOut: hello backup did not finish in time and was canceled.</span></span>
* <span data-ttu-id="68f80-172">4 – létrehozott: hello biztonsági mentési kérelem várakoznia, de nem lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="68f80-172">4 – Created: hello backup request is queued but has not been started.</span></span>
* <span data-ttu-id="68f80-173">5 – kihagyva: hello biztonsági mentés nem haladt esedékes időt. túl sok biztonsági mentések tooa ütemezés.</span><span class="sxs-lookup"><span data-stu-id="68f80-173">5 – Skipped: hello backup did not proceed due tooa schedule triggering too many backups.</span></span>
* <span data-ttu-id="68f80-174">6 – PartiallySucceeded: hello biztonsági mentése sikeres volt, de az egyes fájlok biztonsági mentése nem történt, mert azok nem olvasható.</span><span class="sxs-lookup"><span data-stu-id="68f80-174">6 – PartiallySucceeded: hello backup succeeded, but some files were not backed up because they could not be read.</span></span> <span data-ttu-id="68f80-175">Ez általában akkor fordul elő, mert kizárólagos zárolást hello fájlok lett helyezve.</span><span class="sxs-lookup"><span data-stu-id="68f80-175">This usually happens because an exclusive lock was placed on hello files.</span></span>
* <span data-ttu-id="68f80-176">7 – DeleteInProgress: hello biztonsági mentés kért toobe törölve lett, de még nem lett törölve.</span><span class="sxs-lookup"><span data-stu-id="68f80-176">7 – DeleteInProgress: hello backup has been requested toobe deleted, but has not yet been deleted.</span></span>
* <span data-ttu-id="68f80-177">8 – DeleteFailed: hello biztonsági mentés nem sikerült törölni.</span><span class="sxs-lookup"><span data-stu-id="68f80-177">8 – DeleteFailed: hello backup could not be deleted.</span></span> <span data-ttu-id="68f80-178">Ez akkor fordulhat elő, mert lejárt hello SAS URL-címet, de a használt toocreate hello biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="68f80-178">This might happen because hello SAS URL that was used toocreate hello backup has expired.</span></span>
* <span data-ttu-id="68f80-179">9 – törölve: hello biztonsági mentés törlése sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="68f80-179">9 – Deleted: hello backup was deleted successfully.</span></span>

<a name="restore-app"></a>

## <a name="restore-an-app-from-a-backup"></a><span data-ttu-id="68f80-180">Egy alkalmazás visszaállítása biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="68f80-180">Restore an app from a backup</span></span>
<span data-ttu-id="68f80-181">Ha az alkalmazás törölve lett, vagy ha azt szeretné, toorevert app tooa korábbi verzióját, visszaállíthatja egy biztonsági másolatból hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="68f80-181">If your app has been deleted, or if you want toorevert your app tooa previous version, you can restore hello app from a backup.</span></span> <span data-ttu-id="68f80-182">a visszaállítás tooinvoke küldése egy **POST** kérelem toohello URL-CÍMÉT **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ biztonsági mentés / {backup-azonosító} / visszaállítása**.</span><span class="sxs-lookup"><span data-stu-id="68f80-182">tooinvoke a restore, send a **POST** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.</span></span>

<span data-ttu-id="68f80-183">Ez hello URL-cím néz a példában a webhelyhez.</span><span class="sxs-lookup"><span data-stu-id="68f80-183">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="68f80-184">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/restore**</span><span class="sxs-lookup"><span data-stu-id="68f80-184">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/restore**</span></span>

<span data-ttu-id="68f80-185">Hello kérelemtörzset küld egy JSON-objektum, amely hello visszaállítási művelet hello tulajdonságait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="68f80-185">In hello request body, send a JSON object that contains hello properties for hello restore operation.</span></span> <span data-ttu-id="68f80-186">Íme egy példa a tartalmazó minden kötelező tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="68f80-186">Here is an example containing all required properties:</span></span>

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL toostorage container containing your website backup
    }
}
```

### <a name="restore-tooa-new-app"></a><span data-ttu-id="68f80-187">Új alkalmazás tooa visszaállítása</span><span class="sxs-lookup"><span data-stu-id="68f80-187">Restore tooa new app</span></span>
<span data-ttu-id="68f80-188">Néha szükség lehet toocreate egy új alkalmazást a biztonsági mentéshez, már meglévő alkalmazások felülírása helyett visszaállításakor.</span><span class="sxs-lookup"><span data-stu-id="68f80-188">Sometimes you might want toocreate a new app when you restore a backup, instead of overwriting an already existing app.</span></span> <span data-ttu-id="68f80-189">toodo a, módosítás hello kérelem URL-cím toopoint toohello új alkalmazás toocreate, és hello megadhatjuk **felülírása** tulajdonsága túl hello JSON**hamis**.</span><span class="sxs-lookup"><span data-stu-id="68f80-189">toodo this, change hello request URL toopoint toohello new app you want toocreate, and change hello **overwrite** property in hello JSON too**false**.</span></span>

<a name="delete-app-backup"></a>

## <a name="delete-an-app-backup"></a><span data-ttu-id="68f80-190">Az alkalmazás biztonsági</span><span class="sxs-lookup"><span data-stu-id="68f80-190">Delete an app backup</span></span>
<span data-ttu-id="68f80-191">Ha szeretné, hogy a biztonsági másolat toodelete, küldjön egy **törlése** kérelem toohello URL-CÍMÉT **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ webhelyek / {name} {backup-azonosító} /backups/**.</span><span class="sxs-lookup"><span data-stu-id="68f80-191">If you would like toodelete a backup, send a **DELETE** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.</span></span>

<span data-ttu-id="68f80-192">Ez hello URL-cím néz a példában a webhelyhez.</span><span class="sxs-lookup"><span data-stu-id="68f80-192">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="68f80-193">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**</span><span class="sxs-lookup"><span data-stu-id="68f80-193">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**</span></span>

<a name="manage-sas-url"></a>

## <a name="manage-a-backups-sas-url"></a><span data-ttu-id="68f80-194">A biztonsági mentés SAS URL-cím kezelése</span><span class="sxs-lookup"><span data-stu-id="68f80-194">Manage a backup’s SAS URL</span></span>
<span data-ttu-id="68f80-195">Az Azure App Service toodelete kísérli meg a biztonsági mentés az Azure Storage hello hello biztonsági másolat létrehozásakor megadott SAS URL-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="68f80-195">Azure App Service will attempt toodelete your backup from Azure Storage using hello SAS URL that was provided when hello backup was created.</span></span> <span data-ttu-id="68f80-196">Ha az SAS URL-cím már nem érvényes, hello biztonsági mentés nem lehet törölni, hello REST API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="68f80-196">If this SAS URL is no longer valid, hello backup cannot be deleted through hello REST API.</span></span> <span data-ttu-id="68f80-197">Azonban frissítheti az SAS URL-cím elküldésével társított biztonsági hello egy **POST** kérelem toohello URL-CÍMÉT **https://management.azure.com/subscriptions/ {előfizetés-azonosító} /resourceGroups/ {csoport-erőforrásnév} / providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.</span><span class="sxs-lookup"><span data-stu-id="68f80-197">However, you can update hello SAS URL associated with a backup by sending a **POST** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.</span></span>

<span data-ttu-id="68f80-198">Ez hello URL-cím néz a példában a webhelyhez.</span><span class="sxs-lookup"><span data-stu-id="68f80-198">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="68f80-199">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/List**</span><span class="sxs-lookup"><span data-stu-id="68f80-199">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/list**</span></span>

<span data-ttu-id="68f80-200">Hello kérelemtörzset küld egy JSON-objektum, amely tartalmazza a hello új SAS URL-címet.</span><span class="sxs-lookup"><span data-stu-id="68f80-200">In hello request body, send a JSON object that contains hello new SAS URL.</span></span> <span data-ttu-id="68f80-201">Íme egy példa.</span><span class="sxs-lookup"><span data-stu-id="68f80-201">Here is an example.</span></span>

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

> [!NOTE]
> <span data-ttu-id="68f80-202">Biztonsági okokból SAS URL-címet a biztonsági mentéséhez kapcsolódó hello nem jelennek meg a biztonsági másolat egy GET kérelem küldésekor.</span><span class="sxs-lookup"><span data-stu-id="68f80-202">For security reasons, hello SAS URL associated with a backup is not returned when sending a GET request for a specific backup.</span></span> <span data-ttu-id="68f80-203">Ha azt szeretné, hogy egy biztonsági mentéséhez kapcsolódó SAS URL-cím tooview hello, küldjön egy POST kérést toohello URL-CÍMÉRE a fenti.</span><span class="sxs-lookup"><span data-stu-id="68f80-203">If you want tooview hello SAS URL associated with a backup, send a POST request toohello same URL above.</span></span> <span data-ttu-id="68f80-204">Tartalmaz egy üres JSON-objektum hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="68f80-204">Include an empty JSON object in hello request body.</span></span> <span data-ttu-id="68f80-205">hello hello kiszolgáló válasza az összes adott biztonsági mentés adatai, beleértve az SAS URL-CÍMÉT tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="68f80-205">hello response from hello server contains all of that backup’s information, including its SAS URL.</span></span>
> 
> 

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
