---
title: "Azure IoT Hub eszköz identitások aaaImport exportálása |} Microsoft Docs"
description: "Hogyan toouse hello Azure IoT szolgáltatás SDK tooperform tömeges hello identitás beállításjegyzék tooimport műveleteket, és eszköz identitások exportálása. Importálási műveletek engedélyezése toocreate, update és delete eszköz identitások egyszerre."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2ade1494-45ea-46a7-ade7-cf6e11ce62da
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: b67777d381e03de05d9c007b5ce6bdaf15bbb8f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a><span data-ttu-id="53c3d-104">Az IoT Hub eszköz identitásai tömeges kezelése</span><span class="sxs-lookup"><span data-stu-id="53c3d-104">Manage your IoT Hub device identities in bulk</span></span>

<span data-ttu-id="53c3d-105">Minden egyes IoT-központ rendelkezik egy identitásjegyzékhez toocreate eszközönkénti erőforrások hello szolgáltatásban is használhatja.</span><span class="sxs-lookup"><span data-stu-id="53c3d-105">Each IoT hub has an identity registry you can use toocreate per-device resources in hello service.</span></span> <span data-ttu-id="53c3d-106">hello identitásjegyzékhez is lehetővé teszi toocontrol hozzáférés toohello eszköz felé néző végpontok.</span><span class="sxs-lookup"><span data-stu-id="53c3d-106">hello identity registry also enables you toocontrol access toohello device-facing endpoints.</span></span> <span data-ttu-id="53c3d-107">Ez a cikk ismerteti, hogyan tooimport és export eszköz identitásokat a tömeges tooand egy identitás beállításjegyzékből.</span><span class="sxs-lookup"><span data-stu-id="53c3d-107">This article describes how tooimport and export device identities in bulk tooand from an identity registry.</span></span>

<span data-ttu-id="53c3d-108">Importálás és exportálás zajlanak hello környezetében *feladatok* , amelyek tooexecute tömeges szolgáltatás műveleteket az IoT-központ lehetővé teszik.</span><span class="sxs-lookup"><span data-stu-id="53c3d-108">Import and export operations take place in hello context of *Jobs* that enable you tooexecute bulk service operations against an IoT hub.</span></span>

<span data-ttu-id="53c3d-109">Hello **RegistryManager** osztályba tartozik hello **ExportDevicesAsync** és **ImportDevicesAsync** módszereket, amelyek hello **feladat** keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="53c3d-109">hello **RegistryManager** class includes hello **ExportDevicesAsync** and **ImportDevicesAsync** methods that use hello **Job** framework.</span></span> <span data-ttu-id="53c3d-110">Ezek a módszerek lehetővé teszik a tooexport, az importálás, és az IoT hub identitásjegyzékhez hello a teljes szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="53c3d-110">These methods enable you tooexport, import, and synchronize hello entirety of an IoT hub identity registry.</span></span>

## <a name="what-are-jobs"></a><span data-ttu-id="53c3d-111">Mik azok a feladatok?</span><span class="sxs-lookup"><span data-stu-id="53c3d-111">What are jobs?</span></span>

<span data-ttu-id="53c3d-112">Identitás kapcsolatos műveletek használata hello **feladat** rendszer amikor hello a művelethez:</span><span class="sxs-lookup"><span data-stu-id="53c3d-112">Identity registry operations use hello **Job** system when hello operation:</span></span>

* <span data-ttu-id="53c3d-113">Rendelkezik potenciálisan hosszú végrehajtási időt toostandard futásidejű műveletek képest.</span><span class="sxs-lookup"><span data-stu-id="53c3d-113">Has a potentially long execution time compared toostandard run-time operations.</span></span>
* <span data-ttu-id="53c3d-114">Nagy mennyiségű adatok toohello felhasználói adja vissza.</span><span class="sxs-lookup"><span data-stu-id="53c3d-114">Returns a large amount of data toohello user.</span></span>

<span data-ttu-id="53c3d-115">Egyetlen API-hívással Várakozás vagy blokkolja a hello hello művelet eredményét, helyett hello művelet aszinkron módon létrehoz egy **feladat** , hogy az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="53c3d-115">Instead of a single API call waiting or blocking on hello result of hello operation, hello operation asynchronously creates a **Job** for that IoT hub.</span></span> <span data-ttu-id="53c3d-116">hello műveletet, majd azonnal értéket ad vissza egy **JobProperties** objektum.</span><span class="sxs-lookup"><span data-stu-id="53c3d-116">hello operation then immediately returns a **JobProperties** object.</span></span>

<span data-ttu-id="53c3d-117">következő C# hello code kódrészletet mutat be, hogyan toocreate exportálási feladat:</span><span class="sxs-lookup"><span data-stu-id="53c3d-117">hello following C# code snippet shows how toocreate an export job:</span></span>

```csharp
// Call an export job on hello IoT Hub tooretrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> <span data-ttu-id="53c3d-118">toouse hello **RegistryManager** osztály a C#-kódban, vegye fel a hello **Microsoft.Azure.Devices** NuGet csomag tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-118">toouse hello **RegistryManager** class in your C# code, add hello **Microsoft.Azure.Devices** NuGet package tooyour project.</span></span> <span data-ttu-id="53c3d-119">Hello **RegistryManager** osztály szerepel hello **Microsoft.Azure.Devices** névtér.</span><span class="sxs-lookup"><span data-stu-id="53c3d-119">hello **RegistryManager** class is in hello **Microsoft.Azure.Devices** namespace.</span></span>

<span data-ttu-id="53c3d-120">Használhatja a hello **RegistryManager** tooquery hello állapotának hello osztály **feladat** visszaadott hello segítségével **JobProperties** metaadatok.</span><span class="sxs-lookup"><span data-stu-id="53c3d-120">You can use hello **RegistryManager** class tooquery hello state of hello **Job** using hello returned **JobProperties** metadata.</span></span>

<span data-ttu-id="53c3d-121">hello következő C# kódrészletet bemutatja, hogyan toopoll minden öt másodpercenként toosee Ha hello feladat végrehajtása befejeződött:</span><span class="sxs-lookup"><span data-stu-id="53c3d-121">hello following C# code snippet shows how toopoll every five seconds toosee if hello job has finished executing:</span></span>

```csharp
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a><span data-ttu-id="53c3d-122">Eszközök exportálása</span><span class="sxs-lookup"><span data-stu-id="53c3d-122">Export devices</span></span>

<span data-ttu-id="53c3d-123">Használjon hello **ExportDevicesAsync** metódus tooexport hello a teljes az IoT hub identitás beállításjegyzék tooan [Azure Storage](../storage/index.md) blob tároló használata egy [közös hozzáférésű Jogosultságkód](../storage/common/storage-security-guide.md#data-plane-security).</span><span class="sxs-lookup"><span data-stu-id="53c3d-123">Use hello **ExportDevicesAsync** method tooexport hello entirety of an IoT hub identity registry tooan [Azure Storage](../storage/index.md) blob container using a [Shared Access Signature](../storage/common/storage-security-guide.md#data-plane-security).</span></span>

<span data-ttu-id="53c3d-124">Ez a módszer lehetővé teszi az eszköz adatait egy blob-tároló, amelyek toocreate megbízható másolatait.</span><span class="sxs-lookup"><span data-stu-id="53c3d-124">This method enables you toocreate reliable backups of your device information in a blob container that you control.</span></span>

<span data-ttu-id="53c3d-125">Hello **ExportDevicesAsync** módszernél két paramétert:</span><span class="sxs-lookup"><span data-stu-id="53c3d-125">hello **ExportDevicesAsync** method requires two parameters:</span></span>

* <span data-ttu-id="53c3d-126">A *karakterlánc* , amely tartalmazza a blob-tároló URI.</span><span class="sxs-lookup"><span data-stu-id="53c3d-126">A *string* that contains a URI of a blob container.</span></span> <span data-ttu-id="53c3d-127">Ezt az URI, amely engedélyezi az írási hozzáférést toohello tároló SAS-jogkivonat kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="53c3d-127">This URI must contain a SAS token that grants write access toohello container.</span></span> <span data-ttu-id="53c3d-128">hello feldolgozás egy blokkblob a tároló toostore szerializált hello export eszköz adatokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="53c3d-128">hello job creates a block blob in this container toostore hello serialized export device data.</span></span> <span data-ttu-id="53c3d-129">hello SAS-jogkivonat tartalmaznia kell azokat az engedélyeket:</span><span class="sxs-lookup"><span data-stu-id="53c3d-129">hello SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

* <span data-ttu-id="53c3d-130">A *logikai* , amely azt jelzi, ha azt szeretné, tooexclude hitelesítési kulcsokat az exportálás adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="53c3d-130">A *boolean* that indicates if you want tooexclude authentication keys from your export data.</span></span> <span data-ttu-id="53c3d-131">Ha **hamis**, hitelesítési kulcsokat exportálás kimeneti szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="53c3d-131">If **false**, authentication keys are included in export output.</span></span> <span data-ttu-id="53c3d-132">Ellenkező esetben kulcsok exportálása **null**.</span><span class="sxs-lookup"><span data-stu-id="53c3d-132">Otherwise, keys are exported as **null**.</span></span>

<span data-ttu-id="53c3d-133">hello következő C# kódrészletet bemutatja, hogyan tooinitiate hello eszköz hitelesítési kulcsokat tartalmazó exportálási feladat adatok exportálása és majd kérdezze le az Befejezés:</span><span class="sxs-lookup"><span data-stu-id="53c3d-133">hello following C# code snippet shows how tooinitiate an export job that includes device authentication keys in hello export data and then poll for completion:</span></span>

```csharp
// Call an export job on hello IoT Hub tooretrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

<span data-ttu-id="53c3d-134">hello feladat tárolja a kimeneti blob-tárolóban megadott hello a blokkblob hello nevű **devices.txt**.</span><span class="sxs-lookup"><span data-stu-id="53c3d-134">hello job stores its output in hello provided blob container as a block blob with hello name **devices.txt**.</span></span> <span data-ttu-id="53c3d-135">soronként egy eszközzel szerializált JSON eszközadatok, hello kimeneti adatok állnak.</span><span class="sxs-lookup"><span data-stu-id="53c3d-135">hello output data consists of JSON serialized device data, with one device per line.</span></span>

<span data-ttu-id="53c3d-136">hello alábbi példa bemutatja hello kimeneti adatokat:</span><span class="sxs-lookup"><span data-stu-id="53c3d-136">hello following example shows hello output data:</span></span>

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Ha egy eszköz iker adatokat, majd hello iker adatok is exportált hello eszközadatok együtt. hello következő példa bemutatja, ezt a formátumot. <span data-ttu-id="53c3d-139">Adatok teljes hello "twinETag" sor hello végéig olyan iker adatok.</span><span class="sxs-lookup"><span data-stu-id="53c3d-139">All data from hello "twinETag" line until hello end are twin data.</span></span>

```json
{
   "id":"export-6d84f075-0",
   "eTag":"MQ==",
   "status":"enabled",
   "statusReason":"firstUpdate",
   "authentication":null,
   "twinETag":"AAAAAAAAAAI=",
   "tags":{
      "Location":"LivingRoom"
   },
   "properties":{
      "desired":{
         "Thermostat":{
            "Temperature":75.1,
            "Unit":"F"
         },
         "$metadata":{
            "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
            "$lastUpdatedVersion":2,
            "Thermostat":{
               "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
               "$lastUpdatedVersion":2,
               "Temperature":{
                  "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
                  "$lastUpdatedVersion":2
               },
               "Unit":{
                  "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
                  "$lastUpdatedVersion":2
               }
            }
         },
         "$version":2
      },
      "reported":{
         "$metadata":{
            "$lastUpdated":"2017-03-09T18:30:51.1309437Z"
         },
         "$version":1
      }
   }
}
```

<span data-ttu-id="53c3d-140">Ha kód toothis adatokat kell használni, akkor könnyen deszerializálni a ezeket az adatokat hello **ExportImportDevice** osztály.</span><span class="sxs-lookup"><span data-stu-id="53c3d-140">If you need access toothis data in code, you can easily deserialize this data using hello **ExportImportDevice** class.</span></span> <span data-ttu-id="53c3d-141">hello következő C# kódrészletet bemutatja, hogyan tooread eszközadatokat, de a korábban exportált tooa blokkblob:</span><span class="sxs-lookup"><span data-stu-id="53c3d-141">hello following C# code snippet shows how tooread device information that was previously exported tooa block blob:</span></span>

```csharp
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), null, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [!NOTE]
> <span data-ttu-id="53c3d-142">Is használhatja a hello **GetDevicesAsync** hello metódusában **RegistryManager** osztály toofetch az eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="53c3d-142">You can also use hello **GetDevicesAsync** method of hello **RegistryManager** class toofetch a list of your devices.</span></span> <span data-ttu-id="53c3d-143">Azonban ez a megközelítés azzal 1000 rögzített cap visszaadott eszközobjektumok hello számára.</span><span class="sxs-lookup"><span data-stu-id="53c3d-143">However, this approach has a hard cap of 1000 on hello number of device objects that are returned.</span></span> <span data-ttu-id="53c3d-144">hello várható használati eset a hello **GetDevicesAsync** metódus fejlesztési forgatókönyveket tooaid hibakeresési, és nem ajánlott a termelési számítási feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="53c3d-144">hello expected use case for hello **GetDevicesAsync** method is for development scenarios tooaid debugging and is not recommended for production workloads.</span></span>

## <a name="import-devices"></a><span data-ttu-id="53c3d-145">Eszközök importálása</span><span class="sxs-lookup"><span data-stu-id="53c3d-145">Import devices</span></span>

<span data-ttu-id="53c3d-146">Hello **ImportDevicesAsync** metódus a hello **RegistryManager** osztály tooperform tömeges importálása és szinkronizálás az IoT hub identitásjegyzékhez a műveletek segítségével.</span><span class="sxs-lookup"><span data-stu-id="53c3d-146">hello **ImportDevicesAsync** method in hello **RegistryManager** class enables you tooperform bulk import and synchronization operations in an IoT hub identity registry.</span></span> <span data-ttu-id="53c3d-147">Például a hello **ExportDevicesAsync** metódust, hello **ImportDevicesAsync** metódusnak hello **feladat** keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="53c3d-147">Like hello **ExportDevicesAsync** method, hello **ImportDevicesAsync** method uses hello **Job** framework.</span></span>

<span data-ttu-id="53c3d-148">Mi gondoskodunk hello segítségével **ImportDevicesAsync** metódus, mert a hozzáadása tooprovisioning új eszközöket az a identitásjegyzékhez, is frissíteni és meglévő eszközök törlése.</span><span class="sxs-lookup"><span data-stu-id="53c3d-148">Take care using hello **ImportDevicesAsync** method because in addition tooprovisioning new devices in your identity registry, it can also update and delete existing devices.</span></span>

> [!WARNING]
> <span data-ttu-id="53c3d-149">Az importálási művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="53c3d-149">An import operation cannot be undone.</span></span> <span data-ttu-id="53c3d-150">Mindig készítsen biztonsági másolatot a meglévő adatok hello **ExportDevicesAsync** metódus tooanother blob tároló előtt tömeges módosítása tooyour identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="53c3d-150">Always back up your existing data using hello **ExportDevicesAsync** method tooanother blob container before you make bulk changes tooyour identity registry.</span></span>

<span data-ttu-id="53c3d-151">Hello **ImportDevicesAsync** metódus két paramétereket fogadja:</span><span class="sxs-lookup"><span data-stu-id="53c3d-151">hello **ImportDevicesAsync** method takes two parameters:</span></span>

* <span data-ttu-id="53c3d-152">A *karakterlánc* , amely tartalmazza egy URI-azonosítója egy [Azure Storage](../storage/index.md) blob-tároló toouse, *bemeneti* toohello feladat.</span><span class="sxs-lookup"><span data-stu-id="53c3d-152">A *string* that contains a URI of an [Azure Storage](../storage/index.md) blob container toouse as *input* toohello job.</span></span> <span data-ttu-id="53c3d-153">Ezt az URI toohello tároló olvasási hozzáférést biztosít egy SAS-jogkivonatot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="53c3d-153">This URI must contain a SAS token that grants read access toohello container.</span></span> <span data-ttu-id="53c3d-154">Ez a tároló tartalmaznia kell egy blob hello nevű **devices.txt** szerializált hello eszköz adatok tooimport, amely tartalmazza azokat az identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="53c3d-154">This container must contain a blob with hello name **devices.txt** that contains hello serialized device data tooimport into your identity registry.</span></span> <span data-ttu-id="53c3d-155">hello adatokat importálhat adatokat kell tartalmazniuk eszköz a hello azonos JSON formátumú, hogy hello **ExportImportDevice** feladatot használ, amikor létrehozza a **devices.txt** blob.</span><span class="sxs-lookup"><span data-stu-id="53c3d-155">hello import data must contain device information in hello same JSON format that hello **ExportImportDevice** job uses when it creates a **devices.txt** blob.</span></span> <span data-ttu-id="53c3d-156">hello SAS-jogkivonat tartalmaznia kell azokat az engedélyeket:</span><span class="sxs-lookup"><span data-stu-id="53c3d-156">hello SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Read
   ```
* <span data-ttu-id="53c3d-157">A *karakterlánc* , amely tartalmazza egy URI-azonosítója egy [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob-tároló toouse, *kimeneti* hello feladatból.</span><span class="sxs-lookup"><span data-stu-id="53c3d-157">A *string* that contains a URI of an [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob container toouse as *output* from hello job.</span></span> <span data-ttu-id="53c3d-158">hello feladatot hoz létre a blokkblob a tároló toostore bármely hibainformációk befejeződött hello importálásból **feladat**.</span><span class="sxs-lookup"><span data-stu-id="53c3d-158">hello job creates a block blob in this container toostore any error information from hello completed import **Job**.</span></span> <span data-ttu-id="53c3d-159">hello SAS-jogkivonat tartalmaznia kell azokat az engedélyeket:</span><span class="sxs-lookup"><span data-stu-id="53c3d-159">hello SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> <span data-ttu-id="53c3d-160">a két hello paraméterek mutathat toohello blob tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="53c3d-160">hello two parameters can point toohello same blob container.</span></span> <span data-ttu-id="53c3d-161">hello külön paraméterek egyszerűen további szabályozásának engedélyezése a adatok hello kimeneti tároló jogosultságokra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="53c3d-161">hello separate parameters simply enable more control over your data as hello output container requires additional permissions.</span></span>

<span data-ttu-id="53c3d-162">a következő C# hello hogyan code kódrészletet mutat be egy importálási feladat tooinitiate:</span><span class="sxs-lookup"><span data-stu-id="53c3d-162">hello following C# code snippet shows how tooinitiate an import job:</span></span>

```csharp
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

<span data-ttu-id="53c3d-163">Ez a módszer is hello eszköz iker használt tooimport hello adatait.</span><span class="sxs-lookup"><span data-stu-id="53c3d-163">This method can also be used tooimport hello data for hello device twin.</span></span> <span data-ttu-id="53c3d-164">hello adatbevitel hello formátuma van hello azonos hello látható hello formátumban **ExportDevicesAsync** szakasz.</span><span class="sxs-lookup"><span data-stu-id="53c3d-164">hello format for hello data input is hello same as hello format shown in hello **ExportDevicesAsync** section.</span></span> <span data-ttu-id="53c3d-165">Ezzel a módszerrel importálja újra hello exportált adatokat.</span><span class="sxs-lookup"><span data-stu-id="53c3d-165">In this way, you can reimport hello exported data.</span></span> <span data-ttu-id="53c3d-166">Hello **$metadata** nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="53c3d-166">hello **$metadata** is optional.</span></span>

## <a name="import-behavior"></a><span data-ttu-id="53c3d-167">Importálás viselkedése</span><span class="sxs-lookup"><span data-stu-id="53c3d-167">Import behavior</span></span>

<span data-ttu-id="53c3d-168">Használhatja a hello **ImportDevicesAsync** metódus tooperform hello következő tömeges a identitásjegyzékhez műveletek:</span><span class="sxs-lookup"><span data-stu-id="53c3d-168">You can use hello **ImportDevicesAsync** method tooperform hello following bulk operations in your identity registry:</span></span>

* <span data-ttu-id="53c3d-169">Az új eszközök tömeges regisztrálása</span><span class="sxs-lookup"><span data-stu-id="53c3d-169">Bulk registration of new devices</span></span>
* <span data-ttu-id="53c3d-170">Meglévő eszközök tömeges törlése</span><span class="sxs-lookup"><span data-stu-id="53c3d-170">Bulk deletions of existing devices</span></span>
* <span data-ttu-id="53c3d-171">Tömeges állapotmódosítások (engedélyezése vagy letiltása az eszközök)</span><span class="sxs-lookup"><span data-stu-id="53c3d-171">Bulk status changes (enable or disable devices)</span></span>
* <span data-ttu-id="53c3d-172">Új eszköz hitelesítési kulcsokat tömeges hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="53c3d-172">Bulk assignment of new device authentication keys</span></span>
* <span data-ttu-id="53c3d-173">Tömeges eszköz hitelesítési kulcsokat, automatikus-újragenerálása</span><span class="sxs-lookup"><span data-stu-id="53c3d-173">Bulk auto-regeneration of device authentication keys</span></span>
* <span data-ttu-id="53c3d-174">Tömeges frissítés iker adatok</span><span class="sxs-lookup"><span data-stu-id="53c3d-174">Bulk update of twin data</span></span>

<span data-ttu-id="53c3d-175">Hajthat végre műveletek egy megelőző hello kombinációja **ImportDevicesAsync** hívható meg.</span><span class="sxs-lookup"><span data-stu-id="53c3d-175">You can perform any combination of hello preceding operations within a single **ImportDevicesAsync** call.</span></span> <span data-ttu-id="53c3d-176">Például új eszközöket regisztrálni és törölheti vagy frissítheti a meglévő eszközöket: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="53c3d-176">For example, you can register new devices and delete or update existing devices at hello same time.</span></span> <span data-ttu-id="53c3d-177">Hello együtt használva **ExportDevicesAsync** módszer, egy IoT hub tooanother teljesen az eszközök telepíthet át.</span><span class="sxs-lookup"><span data-stu-id="53c3d-177">When used along with hello **ExportDevicesAsync** method, you can completely migrate all your devices from one IoT hub tooanother.</span></span>

<span data-ttu-id="53c3d-178">Ha hello importfájl iker metaadatokat tartalmaz, a metaadatok hello meglévő iker metaadatok felülírja.</span><span class="sxs-lookup"><span data-stu-id="53c3d-178">If hello import file includes twin metadata, then this metadata overwrites hello existing twin metadata.</span></span> <span data-ttu-id="53c3d-179">Ha hello importfájl nem tartalmaz iker metaadatokat, majd csak hello `lastUpdateTime` metaadatok frissül hello segítségével a jelenlegi időpontnál.</span><span class="sxs-lookup"><span data-stu-id="53c3d-179">If hello import file does not include twin metadata, then only hello `lastUpdateTime` metadata is updated using hello current time.</span></span>

<span data-ttu-id="53c3d-180">Választható használata hello **amelyben a importMode** hello a szerializálási adatok importálása az egyes eszközök toocontrol hello importálási folyamat eszközönkénti tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="53c3d-180">Use hello optional **importMode** property in hello import serialization data for each device toocontrol hello import process per-device.</span></span> <span data-ttu-id="53c3d-181">Hello **amelyben a importMode** tulajdonságnak hello a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="53c3d-181">hello **importMode** property has hello following options:</span></span>

| <span data-ttu-id="53c3d-182">amelyben a importMode</span><span class="sxs-lookup"><span data-stu-id="53c3d-182">importMode</span></span> | <span data-ttu-id="53c3d-183">Leírás</span><span class="sxs-lookup"><span data-stu-id="53c3d-183">Description</span></span> |
| --- | --- |
| <span data-ttu-id="53c3d-184">**createOrUpdate**</span><span class="sxs-lookup"><span data-stu-id="53c3d-184">**createOrUpdate**</span></span> |<span data-ttu-id="53c3d-185">Ha egy eszköz nem létezik a megadott hello **azonosító**, újonnan regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="53c3d-185">If a device does not exist with hello specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="53c3d-186">Ha hello eszköz már létezik, a megadott bemeneti adatok nélkül legutóbb toohello hello felülírja a meglévő adatokat **ETag** érték.</span><span class="sxs-lookup"><span data-stu-id="53c3d-186">If hello device already exists, existing information is overwritten with hello provided input data without regard toohello **ETag** value.</span></span> <br> <span data-ttu-id="53c3d-187">hello felhasználó nem kötelezően megadhatja a kettős adatok hello eszköz adatokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-187">hello user can optionally specify twin data along with hello device data.</span></span> <span data-ttu-id="53c3d-188">hello iker etag, ha meg van adva, akkor feldolgozott egymástól függetlenül hello eszköz etag.</span><span class="sxs-lookup"><span data-stu-id="53c3d-188">hello twin’s etag, if specified, is processed independently from hello device’s etag.</span></span> <span data-ttu-id="53c3d-189">Ha a hello meglévő iker etag nem egyezik, egy hiba kerül toohello naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-189">If there is a mismatch with hello existing twin’s etag, an error is written toohello log file.</span></span> |
| <span data-ttu-id="53c3d-190">**létrehozás**</span><span class="sxs-lookup"><span data-stu-id="53c3d-190">**create**</span></span> |<span data-ttu-id="53c3d-191">Ha egy eszköz nem létezik a megadott hello **azonosító**, újonnan regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="53c3d-191">If a device does not exist with hello specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="53c3d-192">Már létezik hello eszköz, ha egy hiba kerül toohello naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-192">If hello device already exists, an error is written toohello log file.</span></span> <br> <span data-ttu-id="53c3d-193">hello felhasználó nem kötelezően megadhatja a kettős adatok hello eszköz adatokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-193">hello user can optionally specify twin data along with hello device data.</span></span> <span data-ttu-id="53c3d-194">hello iker etag, ha meg van adva, akkor feldolgozott egymástól függetlenül hello eszköz etag.</span><span class="sxs-lookup"><span data-stu-id="53c3d-194">hello twin’s etag, if specified, is processed independently from hello device’s etag.</span></span> <span data-ttu-id="53c3d-195">Ha a hello meglévő iker etag nem egyezik, egy hiba kerül toohello naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-195">If there is a mismatch with hello existing twin’s etag, an error is written toohello log file.</span></span> |
| <span data-ttu-id="53c3d-196">**frissítés**</span><span class="sxs-lookup"><span data-stu-id="53c3d-196">**update**</span></span> |<span data-ttu-id="53c3d-197">Ha egy eszköz már létezik a megadott hello **azonosító**, felülírja a meglévő adatokat a megadott bemeneti adatok nélkül legutóbb toohello hello **ETag** érték.</span><span class="sxs-lookup"><span data-stu-id="53c3d-197">If a device already exists with hello specified **id**, existing information is overwritten with hello provided input data without regard toohello **ETag** value.</span></span> <br/><span data-ttu-id="53c3d-198">Ha hello eszköz nem létezik, egy hiba kerül toohello naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-198">If hello device does not exist, an error is written toohello log file.</span></span> |
| <span data-ttu-id="53c3d-199">**updateIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="53c3d-199">**updateIfMatchETag**</span></span> |<span data-ttu-id="53c3d-200">Ha egy eszköz már létezik a megadott hello **azonosító**, csak akkor, ha nincs a megadott bemeneti adatok hello felülírja a meglévő adatokat egy **ETag** felel meg.</span><span class="sxs-lookup"><span data-stu-id="53c3d-200">If a device already exists with hello specified **id**, existing information is overwritten with hello provided input data only if there is an **ETag** match.</span></span> <br/><span data-ttu-id="53c3d-201">Ha hello eszköz nem létezik, egy hiba kerül toohello naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-201">If hello device does not exist, an error is written toohello log file.</span></span> <br/><span data-ttu-id="53c3d-202">Ha egy **ETag** eltérés, egy hiba kerül toohello naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-202">If there is an **ETag** mismatch, an error is written toohello log file.</span></span> |
| <span data-ttu-id="53c3d-203">**createOrUpdateIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="53c3d-203">**createOrUpdateIfMatchETag**</span></span> |<span data-ttu-id="53c3d-204">Ha egy eszköz nem létezik a megadott hello **azonosító**, újonnan regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="53c3d-204">If a device does not exist with hello specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="53c3d-205">Ha hello eszköz már létezik, csak akkor, ha nincs a megadott bemeneti adatok hello felülírja a meglévő adatokat egy **ETag** felel meg.</span><span class="sxs-lookup"><span data-stu-id="53c3d-205">If hello device already exists, existing information is overwritten with hello provided input data only if there is an **ETag** match.</span></span> <br/><span data-ttu-id="53c3d-206">Ha egy **ETag** eltérés, egy hiba kerül toohello naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-206">If there is an **ETag** mismatch, an error is written toohello log file.</span></span> <br> <span data-ttu-id="53c3d-207">hello felhasználó nem kötelezően megadhatja a kettős adatok hello eszköz adatokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-207">hello user can optionally specify twin data along with hello device data.</span></span> <span data-ttu-id="53c3d-208">hello iker etag, ha meg van adva, akkor feldolgozott egymástól függetlenül hello eszköz etag.</span><span class="sxs-lookup"><span data-stu-id="53c3d-208">hello twin’s etag, if specified, is processed independently from hello device’s etag.</span></span> <span data-ttu-id="53c3d-209">Ha a hello meglévő iker etag nem egyezik, egy hiba kerül toohello naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-209">If there is a mismatch with hello existing twin’s etag, an error is written toohello log file.</span></span> |
| <span data-ttu-id="53c3d-210">**törlés**</span><span class="sxs-lookup"><span data-stu-id="53c3d-210">**delete**</span></span> |<span data-ttu-id="53c3d-211">Ha egy eszköz már létezik a megadott hello **azonosító**, figyelembe véve toohello nélkül törli **ETag** érték.</span><span class="sxs-lookup"><span data-stu-id="53c3d-211">If a device already exists with hello specified **id**, it is deleted without regard toohello **ETag** value.</span></span> <br/><span data-ttu-id="53c3d-212">Ha hello eszköz nem létezik, egy hiba kerül toohello naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-212">If hello device does not exist, an error is written toohello log file.</span></span> |
| <span data-ttu-id="53c3d-213">**deleteIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="53c3d-213">**deleteIfMatchETag**</span></span> |<span data-ttu-id="53c3d-214">Ha egy eszköz már létezik a megadott hello **azonosító**, törlése, csak akkor, ha van egy **ETag** felel meg.</span><span class="sxs-lookup"><span data-stu-id="53c3d-214">If a device already exists with hello specified **id**, it is deleted only if there is an **ETag** match.</span></span> <span data-ttu-id="53c3d-215">Ha hello eszköz nem létezik, egy hiba kerül toohello naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-215">If hello device does not exist, an error is written toohello log file.</span></span> <br/><span data-ttu-id="53c3d-216">Ha az egy ETag nem egyezik, egy hiba kerül toohello naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="53c3d-216">If there is an ETag mismatch, an error is written toohello log file.</span></span> |

> [!NOTE]
> <span data-ttu-id="53c3d-217">Ha hello szerializációs adatokból nem definiálhat explicit módon egy **amelyben a importMode** jelző egy eszközhöz, alapértelmezés szerint túl**createOrUpdate** hello során az importálási művelet.</span><span class="sxs-lookup"><span data-stu-id="53c3d-217">If hello serialization data does not explicitly define an **importMode** flag for a device, it defaults too**createOrUpdate** during hello import operation.</span></span>

## <a name="import-devices-example--bulk-device-provisioning"></a><span data-ttu-id="53c3d-218">Eszközök például importálása – tömeges eszköz kiépítése</span><span class="sxs-lookup"><span data-stu-id="53c3d-218">Import devices example – bulk device provisioning</span></span>

<span data-ttu-id="53c3d-219">hello alábbi C# mintakód bemutatja, hogyan toogenerate több eszközt identitás, amely:</span><span class="sxs-lookup"><span data-stu-id="53c3d-219">hello following C# code sample illustrates how toogenerate multiple device identities that:</span></span>

* <span data-ttu-id="53c3d-220">Például a hitelesítési kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="53c3d-220">Include authentication keys.</span></span>
* <span data-ttu-id="53c3d-221">Adott eszköz információk tooa blokkblob írása.</span><span class="sxs-lookup"><span data-stu-id="53c3d-221">Write that device information tooa block blob.</span></span>
* <span data-ttu-id="53c3d-222">Hello eszközök importálása hello identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="53c3d-222">Import hello devices into hello identity registry.</span></span>

```csharp
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
  // Create a new ExportImportDevice
  // CryptoKeyGenerator is in hello Microsoft.Azure.Devices.Common namespace
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device toohello list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write hello list toohello blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using hello blob tooadd new devices
// Log information related toohello job is written toohello same container
// This normally takes 1 minute per 100 devices
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a><span data-ttu-id="53c3d-223">Eszközök például – tömeges törlésének importálása</span><span class="sxs-lookup"><span data-stu-id="53c3d-223">Import devices example – bulk deletion</span></span>

<span data-ttu-id="53c3d-224">hello következő mintakód bemutatja, hogyan használatával hozzáadott toodelete hello eszközök hello előző példakód:</span><span class="sxs-lookup"><span data-stu-id="53c3d-224">hello following code sample shows you how toodelete hello devices you added using hello previous code sample:</span></span>

```csharp
// Step 1: Update each device's ImportMode toobe Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back tooan ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write hello new import data back toohello block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using hello same blob toodelete all devices
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="get-hello-container-sas-uri"></a><span data-ttu-id="53c3d-225">Hello tároló SAS URI-JÁNAK beolvasása</span><span class="sxs-lookup"><span data-stu-id="53c3d-225">Get hello container SAS URI</span></span>

<span data-ttu-id="53c3d-226">hello következő mintakód bemutatja, hogyan toogenerate egy [SAS URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) az olvasási, írási és törlési engedélyek egy blob-tároló:</span><span class="sxs-lookup"><span data-stu-id="53c3d-226">hello following code sample shows you how toogenerate a [SAS URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) with read, write, and delete permissions for a blob container:</span></span>

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set hello expiry time and permissions for hello container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate hello shared access signature on hello container,
  // setting hello constraints directly on hello signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return hello URI string for hello container,
  // including hello SAS token.
  return container.Uri + sasContainerToken;
}
```

## <a name="next-steps"></a><span data-ttu-id="53c3d-227">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="53c3d-227">Next steps</span></span>

<span data-ttu-id="53c3d-228">Ebben a cikkben megtanulta, hogyan tooperform tömeges hello identitásjegyzékhez az IoT-központ a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="53c3d-228">In this article, you learned how tooperform bulk operations against hello identity registry in an IoT hub.</span></span> <span data-ttu-id="53c3d-229">Kövesse az alábbi hivatkozások toolearn Azure IoT Hub kezelésével kapcsolatos további:</span><span class="sxs-lookup"><span data-stu-id="53c3d-229">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="53c3d-230">[Az IoT-központ metrikák][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="53c3d-230">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="53c3d-231">[Figyelési műveletek][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="53c3d-231">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="53c3d-232">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="53c3d-232">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="53c3d-233">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="53c3d-233">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="53c3d-234">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="53c3d-234">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
