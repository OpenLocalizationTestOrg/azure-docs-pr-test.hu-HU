---
title: "Importálás és exportálás Azure IoT Hub eszköz identitások |} Microsoft Docs"
description: "Hogyan használható az Azure IoT szolgáltatás SDK szemben az identitásjegyzékhez történő importálására és exportálására eszköz identitások tömeges műveletek végrehajtásához. Importálási műveletek lehetővé teszik létrehozása, frissítése és törlése eszköz identitások egyszerre."
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
ms.openlocfilehash: ad2c6d585eef5450f7f0912ffa4753fe80d86b37
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a><span data-ttu-id="3063e-104">Az IoT Hub eszköz identitásai tömeges kezelése</span><span class="sxs-lookup"><span data-stu-id="3063e-104">Manage your IoT Hub device identities in bulk</span></span>

<span data-ttu-id="3063e-105">Minden egyes IoT-központ rendelkezik egy identitásjegyzékhez eszközönkénti erőforrásokat létrehozni a szolgáltatás segítségével.</span><span class="sxs-lookup"><span data-stu-id="3063e-105">Each IoT hub has an identity registry you can use to create per-device resources in the service.</span></span> <span data-ttu-id="3063e-106">Az identitásjegyzékhez is lehetővé teszi az eszköz felé néző végpontok való hozzáférés vezérlése érdekében.</span><span class="sxs-lookup"><span data-stu-id="3063e-106">The identity registry also enables you to control access to the device-facing endpoints.</span></span> <span data-ttu-id="3063e-107">Ez a cikk ismerteti, hogyan importálhat és exportálhat eszköz identitások tömeges irányuló és onnan az identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="3063e-107">This article describes how to import and export device identities in bulk to and from an identity registry.</span></span>

<span data-ttu-id="3063e-108">Importálás és exportálás zajlanak környezetében *feladatok* , amelyek lehetővé teszik az IoT-központ szolgáltatás tömeges műveleteket végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="3063e-108">Import and export operations take place in the context of *Jobs* that enable you to execute bulk service operations against an IoT hub.</span></span>

<span data-ttu-id="3063e-109">A **RegistryManager** osztály tartalmazza a **ExportDevicesAsync** és **ImportDevicesAsync** módszereket, amelyek a **feladat** keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="3063e-109">The **RegistryManager** class includes the **ExportDevicesAsync** and **ImportDevicesAsync** methods that use the **Job** framework.</span></span> <span data-ttu-id="3063e-110">Ezek a módszerek lehetővé teszik a exportálása, importálása és az IoT hub identitásjegyzékhez a a teljes szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="3063e-110">These methods enable you to export, import, and synchronize the entirety of an IoT hub identity registry.</span></span>

## <a name="what-are-jobs"></a><span data-ttu-id="3063e-111">Mik azok a feladatok?</span><span class="sxs-lookup"><span data-stu-id="3063e-111">What are jobs?</span></span>

<span data-ttu-id="3063e-112">Identitás kapcsolatos műveletek használata a **feladat** rendszer Ha a művelet:</span><span class="sxs-lookup"><span data-stu-id="3063e-112">Identity registry operations use the **Job** system when the operation:</span></span>

* <span data-ttu-id="3063e-113">Potenciálisan hosszú végrehajtási időt képest van szabványos futásidejű műveletek.</span><span class="sxs-lookup"><span data-stu-id="3063e-113">Has a potentially long execution time compared to standard run-time operations.</span></span>
* <span data-ttu-id="3063e-114">A felhasználó egy nagy mennyiségű adatot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="3063e-114">Returns a large amount of data to the user.</span></span>

<span data-ttu-id="3063e-115">Egyetlen API-hívással Várakozás vagy blokkolja-e a művelet eredményét, helyett a művelet aszinkron módon létrehoz egy **feladat** , hogy az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3063e-115">Instead of a single API call waiting or blocking on the result of the operation, the operation asynchronously creates a **Job** for that IoT hub.</span></span> <span data-ttu-id="3063e-116">A műveletet, majd azonnal értéket ad vissza egy **JobProperties** objektum.</span><span class="sxs-lookup"><span data-stu-id="3063e-116">The operation then immediately returns a **JobProperties** object.</span></span>

<span data-ttu-id="3063e-117">A következő C# kódrészletet exportálási feladat létrehozását szemlélteti:</span><span class="sxs-lookup"><span data-stu-id="3063e-117">The following C# code snippet shows how to create an export job:</span></span>

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> <span data-ttu-id="3063e-118">Használatához a **RegistryManager** osztály a C#-kódban, adja hozzá a **Microsoft.Azure.Devices** NuGet-csomagot a projekthez.</span><span class="sxs-lookup"><span data-stu-id="3063e-118">To use the **RegistryManager** class in your C# code, add the **Microsoft.Azure.Devices** NuGet package to your project.</span></span> <span data-ttu-id="3063e-119">A **RegistryManager** osztály a **Microsoft.Azure.Devices** névtér.</span><span class="sxs-lookup"><span data-stu-id="3063e-119">The **RegistryManager** class is in the **Microsoft.Azure.Devices** namespace.</span></span>

<span data-ttu-id="3063e-120">Használhatja a **RegistryManager** osztály állapotának lekérdezése a **feladat** használatával a visszaadott **JobProperties** metaadatok.</span><span class="sxs-lookup"><span data-stu-id="3063e-120">You can use the **RegistryManager** class to query the state of the **Job** using the returned **JobProperties** metadata.</span></span>

<span data-ttu-id="3063e-121">A következő C# kódrészletet öt másodpercenként kérdezze le a megjelenítéséhez, ha a feladat végrehajtása befejeződött mutatja be:</span><span class="sxs-lookup"><span data-stu-id="3063e-121">The following C# code snippet shows how to poll every five seconds to see if the job has finished executing:</span></span>

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

## <a name="export-devices"></a><span data-ttu-id="3063e-122">Eszközök exportálása</span><span class="sxs-lookup"><span data-stu-id="3063e-122">Export devices</span></span>

<span data-ttu-id="3063e-123">Használja a **ExportDevicesAsync** a a teljes az IoT hub identitásjegyzékhez történő exportálására egy [Azure Storage](../storage/index.md) blob tároló használata egy [közös hozzáférésű Jogosultságkód](../storage/common/storage-security-guide.md#data-plane-security).</span><span class="sxs-lookup"><span data-stu-id="3063e-123">Use the **ExportDevicesAsync** method to export the entirety of an IoT hub identity registry to an [Azure Storage](../storage/index.md) blob container using a [Shared Access Signature](../storage/common/storage-security-guide.md#data-plane-security).</span></span>

<span data-ttu-id="3063e-124">Ez a módszer lehetővé teszi a személyes eszköz adatok megbízható biztonsági mentések létrehozását egy blob a tárolóban, amelyek.</span><span class="sxs-lookup"><span data-stu-id="3063e-124">This method enables you to create reliable backups of your device information in a blob container that you control.</span></span>

<span data-ttu-id="3063e-125">A **ExportDevicesAsync** módszernél két paramétert:</span><span class="sxs-lookup"><span data-stu-id="3063e-125">The **ExportDevicesAsync** method requires two parameters:</span></span>

* <span data-ttu-id="3063e-126">A *karakterlánc* , amely tartalmazza a blob-tároló URI.</span><span class="sxs-lookup"><span data-stu-id="3063e-126">A *string* that contains a URI of a blob container.</span></span> <span data-ttu-id="3063e-127">Ezt az URI írási hozzáférést a tároló SAS-jogkivonatot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="3063e-127">This URI must contain a SAS token that grants write access to the container.</span></span> <span data-ttu-id="3063e-128">A feladat a blokkblob ebben a tárolóban a szerializált export eszköz adatainak tárolására hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3063e-128">The job creates a block blob in this container to store the serialized export device data.</span></span> <span data-ttu-id="3063e-129">A SAS-jogkivonat tartalmaznia kell azokat az engedélyeket:</span><span class="sxs-lookup"><span data-stu-id="3063e-129">The SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

* <span data-ttu-id="3063e-130">A *logikai* , amely azt jelzi, hogy szeretné-e a hitelesítési kulcsokat kizárása az adatok exportálása.</span><span class="sxs-lookup"><span data-stu-id="3063e-130">A *boolean* that indicates if you want to exclude authentication keys from your export data.</span></span> <span data-ttu-id="3063e-131">Ha **hamis**, hitelesítési kulcsokat exportálás kimeneti szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="3063e-131">If **false**, authentication keys are included in export output.</span></span> <span data-ttu-id="3063e-132">Ellenkező esetben kulcsok exportálása **null**.</span><span class="sxs-lookup"><span data-stu-id="3063e-132">Otherwise, keys are exported as **null**.</span></span>

<span data-ttu-id="3063e-133">A következő C# kódrészletet mutatja be, amely tartalmazza az eszköz hitelesítési kulcsokat az adatok exportálása az exportálási feladat kezdeményezése, és majd kérdezze le az Befejezés:</span><span class="sxs-lookup"><span data-stu-id="3063e-133">The following C# code snippet shows how to initiate an export job that includes device authentication keys in the export data and then poll for completion:</span></span>

```csharp
// Call an export job on the IoT Hub to retrieve all devices
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

<span data-ttu-id="3063e-134">A feladat tárolja a kimenetet a megadott blob-tárolóban egy blokkblob nevű **devices.txt**.</span><span class="sxs-lookup"><span data-stu-id="3063e-134">The job stores its output in the provided blob container as a block blob with the name **devices.txt**.</span></span> <span data-ttu-id="3063e-135">A kimeneti adatok JSON szerializált eszközadatok, soronként egy eszközzel áll.</span><span class="sxs-lookup"><span data-stu-id="3063e-135">The output data consists of JSON serialized device data, with one device per line.</span></span>

<span data-ttu-id="3063e-136">A következő példa bemutatja a kimeneti adatokat:</span><span class="sxs-lookup"><span data-stu-id="3063e-136">The following example shows the output data:</span></span>

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Ha egy eszköz iker adatokat, majd a kettős adatok is exportált együtt az eszközadatok. A következő példa bemutatja, ezt a formátumot. <span data-ttu-id="3063e-139">A "twinETag" sor végéig minden adatát iker adatok.</span><span class="sxs-lookup"><span data-stu-id="3063e-139">All data from the "twinETag" line until the end are twin data.</span></span>

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

<span data-ttu-id="3063e-140">Ha az adatokhoz, a kódban van szüksége, is könnyen deszerializálása ezen adatok segítségével a **ExportImportDevice** osztály.</span><span class="sxs-lookup"><span data-stu-id="3063e-140">If you need access to this data in code, you can easily deserialize this data using the **ExportImportDevice** class.</span></span> <span data-ttu-id="3063e-141">A következő C# kódrészletet olvassa el a korábban exportált blokkblobba eszközadatokat mutatja be:</span><span class="sxs-lookup"><span data-stu-id="3063e-141">The following C# code snippet shows how to read device information that was previously exported to a block blob:</span></span>

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
> <span data-ttu-id="3063e-142">Használhatja a **GetDevicesAsync** metódusában a **RegistryManager** osztály az eszközök listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="3063e-142">You can also use the **GetDevicesAsync** method of the **RegistryManager** class to fetch a list of your devices.</span></span> <span data-ttu-id="3063e-143">Azonban ez a megközelítés azzal az 1000 merevlemez maximális visszaadott eszköz objektumok száma.</span><span class="sxs-lookup"><span data-stu-id="3063e-143">However, this approach has a hard cap of 1000 on the number of device objects that are returned.</span></span> <span data-ttu-id="3063e-144">A várható használati eset a **GetDevicesAsync** metódus hibakeresés elősegítésére fejlesztési forgatókönyvre vonatkozó, és nem ajánlott a termelési számítási feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="3063e-144">The expected use case for the **GetDevicesAsync** method is for development scenarios to aid debugging and is not recommended for production workloads.</span></span>

## <a name="import-devices"></a><span data-ttu-id="3063e-145">Eszközök importálása</span><span class="sxs-lookup"><span data-stu-id="3063e-145">Import devices</span></span>

<span data-ttu-id="3063e-146">A **ImportDevicesAsync** metódust a **RegistryManager** osztály lehetővé teszi az IoT hub identitásjegyzékhez tömeges importálása és szinkronizálás műveleteinek elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="3063e-146">The **ImportDevicesAsync** method in the **RegistryManager** class enables you to perform bulk import and synchronization operations in an IoT hub identity registry.</span></span> <span data-ttu-id="3063e-147">Például a **ExportDevicesAsync** metódus, a **ImportDevicesAsync** módszert használ a **feladat** keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="3063e-147">Like the **ExportDevicesAsync** method, the **ImportDevicesAsync** method uses the **Job** framework.</span></span>

<span data-ttu-id="3063e-148">Mi gondoskodunk használatával a **ImportDevicesAsync** metódus mert használt dinamikus kiosztásnál a identitásjegyzékhez új eszközök mellett is frissíteni és törölje a meglévő eszközökön.</span><span class="sxs-lookup"><span data-stu-id="3063e-148">Take care using the **ImportDevicesAsync** method because in addition to provisioning new devices in your identity registry, it can also update and delete existing devices.</span></span>

> [!WARNING]
> <span data-ttu-id="3063e-149">Az importálási művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="3063e-149">An import operation cannot be undone.</span></span> <span data-ttu-id="3063e-150">Mindig készítsen biztonsági másolatot a meglévő adatok a **ExportDevicesAsync** egy másik blob-tároló előtt tömeges módszert biztosít az identitásjegyzékhez változik.</span><span class="sxs-lookup"><span data-stu-id="3063e-150">Always back up your existing data using the **ExportDevicesAsync** method to another blob container before you make bulk changes to your identity registry.</span></span>

<span data-ttu-id="3063e-151">A **ImportDevicesAsync** metódus két paramétereket fogadja:</span><span class="sxs-lookup"><span data-stu-id="3063e-151">The **ImportDevicesAsync** method takes two parameters:</span></span>

* <span data-ttu-id="3063e-152">A *karakterlánc* , amely tartalmazza egy URI-azonosítója egy [Azure Storage](../storage/index.md) kívánja használni, mint a blobtárolót *bemeneti* a feladathoz.</span><span class="sxs-lookup"><span data-stu-id="3063e-152">A *string* that contains a URI of an [Azure Storage](../storage/index.md) blob container to use as *input* to the job.</span></span> <span data-ttu-id="3063e-153">Ezt az URI tartalmaznia kell egy SAS-jogkivonatot, amely olvasási hozzáférést biztosít a tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="3063e-153">This URI must contain a SAS token that grants read access to the container.</span></span> <span data-ttu-id="3063e-154">Ez a tároló tartalmaznia kell egy blob nevű **devices.txt** , amely tartalmazza a szerializált eszközadatok a identitásjegyzékhez rendszerbe való importálás érdekében.</span><span class="sxs-lookup"><span data-stu-id="3063e-154">This container must contain a blob with the name **devices.txt** that contains the serialized device data to import into your identity registry.</span></span> <span data-ttu-id="3063e-155">Az importálási adatok tartalmaznia kell az eszközinformáció ugyanazon JSON formátumban a **ExportImportDevice** feladatot használ, amikor létrehozza a **devices.txt** blob.</span><span class="sxs-lookup"><span data-stu-id="3063e-155">The import data must contain device information in the same JSON format that the **ExportImportDevice** job uses when it creates a **devices.txt** blob.</span></span> <span data-ttu-id="3063e-156">A SAS-jogkivonat tartalmaznia kell azokat az engedélyeket:</span><span class="sxs-lookup"><span data-stu-id="3063e-156">The SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Read
   ```
* <span data-ttu-id="3063e-157">A *karakterlánc* , amely tartalmazza egy URI-azonosítója egy [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) kívánja használni, mint a blobtárolót *kimeneti* a feladatból.</span><span class="sxs-lookup"><span data-stu-id="3063e-157">A *string* that contains a URI of an [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob container to use as *output* from the job.</span></span> <span data-ttu-id="3063e-158">A feladat létrehoz egy blokkblob ebben a tárolóban, az importálás befejezése hiba adatok tárolására **feladat**.</span><span class="sxs-lookup"><span data-stu-id="3063e-158">The job creates a block blob in this container to store any error information from the completed import **Job**.</span></span> <span data-ttu-id="3063e-159">A SAS-jogkivonat tartalmaznia kell azokat az engedélyeket:</span><span class="sxs-lookup"><span data-stu-id="3063e-159">The SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> <span data-ttu-id="3063e-160">A két paraméter blob tárolóhoz is mutat.</span><span class="sxs-lookup"><span data-stu-id="3063e-160">The two parameters can point to the same blob container.</span></span> <span data-ttu-id="3063e-161">A különböző paraméterek egyszerűen további szabályozásának engedélyezése az adatok a kimeneti tároló jogosultságokra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3063e-161">The separate parameters simply enable more control over your data as the output container requires additional permissions.</span></span>

<span data-ttu-id="3063e-162">A következő C# kódrészletet jeleníti meg az importálási feladat indítása:</span><span class="sxs-lookup"><span data-stu-id="3063e-162">The following C# code snippet shows how to initiate an import job:</span></span>

```csharp
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

<span data-ttu-id="3063e-163">Ez a módszer az adatok importálása az eszköz iker is használható.</span><span class="sxs-lookup"><span data-stu-id="3063e-163">This method can also be used to import the data for the device twin.</span></span> <span data-ttu-id="3063e-164">A bemeneti adatok formátuma látható formátuma megegyezik a **ExportDevicesAsync** szakasz.</span><span class="sxs-lookup"><span data-stu-id="3063e-164">The format for the data input is the same as the format shown in the **ExportDevicesAsync** section.</span></span> <span data-ttu-id="3063e-165">Ezzel a módszerrel importálja újra az exportált adatokat.</span><span class="sxs-lookup"><span data-stu-id="3063e-165">In this way, you can reimport the exported data.</span></span> <span data-ttu-id="3063e-166">A **$metadata** nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="3063e-166">The **$metadata** is optional.</span></span>

## <a name="import-behavior"></a><span data-ttu-id="3063e-167">Importálás viselkedése</span><span class="sxs-lookup"><span data-stu-id="3063e-167">Import behavior</span></span>

<span data-ttu-id="3063e-168">Használhatja a **ImportDevicesAsync** metódus a következő tömeges műveleteinek elvégzéséhez a identitásjegyzékhez:</span><span class="sxs-lookup"><span data-stu-id="3063e-168">You can use the **ImportDevicesAsync** method to perform the following bulk operations in your identity registry:</span></span>

* <span data-ttu-id="3063e-169">Az új eszközök tömeges regisztrálása</span><span class="sxs-lookup"><span data-stu-id="3063e-169">Bulk registration of new devices</span></span>
* <span data-ttu-id="3063e-170">Meglévő eszközök tömeges törlése</span><span class="sxs-lookup"><span data-stu-id="3063e-170">Bulk deletions of existing devices</span></span>
* <span data-ttu-id="3063e-171">Tömeges állapotmódosítások (engedélyezése vagy letiltása az eszközök)</span><span class="sxs-lookup"><span data-stu-id="3063e-171">Bulk status changes (enable or disable devices)</span></span>
* <span data-ttu-id="3063e-172">Új eszköz hitelesítési kulcsokat tömeges hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3063e-172">Bulk assignment of new device authentication keys</span></span>
* <span data-ttu-id="3063e-173">Tömeges eszköz hitelesítési kulcsokat, automatikus-újragenerálása</span><span class="sxs-lookup"><span data-stu-id="3063e-173">Bulk auto-regeneration of device authentication keys</span></span>
* <span data-ttu-id="3063e-174">Tömeges frissítés iker adatok</span><span class="sxs-lookup"><span data-stu-id="3063e-174">Bulk update of twin data</span></span>

<span data-ttu-id="3063e-175">Végezheti el az előző művelet egy bármely kombinációja **ImportDevicesAsync** hívható meg.</span><span class="sxs-lookup"><span data-stu-id="3063e-175">You can perform any combination of the preceding operations within a single **ImportDevicesAsync** call.</span></span> <span data-ttu-id="3063e-176">Például új eszközöket regisztrálni és törölheti vagy frissítheti a meglévő eszközöket egy időben.</span><span class="sxs-lookup"><span data-stu-id="3063e-176">For example, you can register new devices and delete or update existing devices at the same time.</span></span> <span data-ttu-id="3063e-177">Mentén használata esetén a **ExportDevicesAsync** módszer, teljesen át lehet az összes eszközt egy IoT-központot egy másikra.</span><span class="sxs-lookup"><span data-stu-id="3063e-177">When used along with the **ExportDevicesAsync** method, you can completely migrate all your devices from one IoT hub to another.</span></span>

<span data-ttu-id="3063e-178">Ha az importált fájl iker metaadatokat tartalmaz, a metaadatok felülírja a meglévő iker metaadatokat.</span><span class="sxs-lookup"><span data-stu-id="3063e-178">If the import file includes twin metadata, then this metadata overwrites the existing twin metadata.</span></span> <span data-ttu-id="3063e-179">Ha az importált fájl nem tartalmaz iker metaadatokat, majd csak a `lastUpdateTime` metaadatok frissül, az aktuális idő.</span><span class="sxs-lookup"><span data-stu-id="3063e-179">If the import file does not include twin metadata, then only the `lastUpdateTime` metadata is updated using the current time.</span></span>

<span data-ttu-id="3063e-180">Használja az opcionális **amelyben a importMode** tulajdonság szerializációs adatok importálása az egyes eszközök az importálási folyamat eszközönkénti szabályozására.</span><span class="sxs-lookup"><span data-stu-id="3063e-180">Use the optional **importMode** property in the import serialization data for each device to control the import process per-device.</span></span> <span data-ttu-id="3063e-181">A **amelyben a importMode** tulajdonságnak a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="3063e-181">The **importMode** property has the following options:</span></span>

| <span data-ttu-id="3063e-182">amelyben a importMode</span><span class="sxs-lookup"><span data-stu-id="3063e-182">importMode</span></span> | <span data-ttu-id="3063e-183">Leírás</span><span class="sxs-lookup"><span data-stu-id="3063e-183">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3063e-184">**createOrUpdate**</span><span class="sxs-lookup"><span data-stu-id="3063e-184">**createOrUpdate**</span></span> |<span data-ttu-id="3063e-185">Ha egy eszköz nem létezik a megadott **azonosító**, újonnan regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="3063e-185">If a device does not exist with the specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="3063e-186">Ha az eszköz már létezik, a megadott bemeneti adatok nélkül tekintettel felülírja a meglévő adatokat a **ETag** érték.</span><span class="sxs-lookup"><span data-stu-id="3063e-186">If the device already exists, existing information is overwritten with the provided input data without regard to the **ETag** value.</span></span> <br> <span data-ttu-id="3063e-187">A felhasználó megadja a két adatok az eszköz adatokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="3063e-187">The user can optionally specify twin data along with the device data.</span></span> <span data-ttu-id="3063e-188">A kettős etag, ha meg van adva, akkor feldolgozott egymástól függetlenül az eszköz etag.</span><span class="sxs-lookup"><span data-stu-id="3063e-188">The twin’s etag, if specified, is processed independently from the device’s etag.</span></span> <span data-ttu-id="3063e-189">Ha eltérést okoz a meglévő iker etag, egy hiba kerül a naplófájlba írást.</span><span class="sxs-lookup"><span data-stu-id="3063e-189">If there is a mismatch with the existing twin’s etag, an error is written to the log file.</span></span> |
| <span data-ttu-id="3063e-190">**létrehozás**</span><span class="sxs-lookup"><span data-stu-id="3063e-190">**create**</span></span> |<span data-ttu-id="3063e-191">Ha egy eszköz nem létezik a megadott **azonosító**, újonnan regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="3063e-191">If a device does not exist with the specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="3063e-192">Ha az eszköz már létezik, egy hiba kerül a naplófájlba írást.</span><span class="sxs-lookup"><span data-stu-id="3063e-192">If the device already exists, an error is written to the log file.</span></span> <br> <span data-ttu-id="3063e-193">A felhasználó megadja a két adatok az eszköz adatokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="3063e-193">The user can optionally specify twin data along with the device data.</span></span> <span data-ttu-id="3063e-194">A kettős etag, ha meg van adva, akkor feldolgozott egymástól függetlenül az eszköz etag.</span><span class="sxs-lookup"><span data-stu-id="3063e-194">The twin’s etag, if specified, is processed independently from the device’s etag.</span></span> <span data-ttu-id="3063e-195">Ha eltérést okoz a meglévő iker etag, egy hiba kerül a naplófájlba írást.</span><span class="sxs-lookup"><span data-stu-id="3063e-195">If there is a mismatch with the existing twin’s etag, an error is written to the log file.</span></span> |
| <span data-ttu-id="3063e-196">**frissítés**</span><span class="sxs-lookup"><span data-stu-id="3063e-196">**update**</span></span> |<span data-ttu-id="3063e-197">Ha egy eszköz már létezik a megadott **azonosító**, a megadott bemeneti adatok nélkül tekintettel felülírja a meglévő adatokat a **ETag** érték.</span><span class="sxs-lookup"><span data-stu-id="3063e-197">If a device already exists with the specified **id**, existing information is overwritten with the provided input data without regard to the **ETag** value.</span></span> <br/><span data-ttu-id="3063e-198">Ha az eszköz nem létezik, egy hiba kerül a naplófájlba írást.</span><span class="sxs-lookup"><span data-stu-id="3063e-198">If the device does not exist, an error is written to the log file.</span></span> |
| <span data-ttu-id="3063e-199">**updateIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="3063e-199">**updateIfMatchETag**</span></span> |<span data-ttu-id="3063e-200">Ha egy eszköz már létezik a megadott **azonosító**, meglévő információt felülírja a megadott bemeneti adatok csak akkor, ha van egy **ETag** felel meg.</span><span class="sxs-lookup"><span data-stu-id="3063e-200">If a device already exists with the specified **id**, existing information is overwritten with the provided input data only if there is an **ETag** match.</span></span> <br/><span data-ttu-id="3063e-201">Ha az eszköz nem létezik, egy hiba kerül a naplófájlba írást.</span><span class="sxs-lookup"><span data-stu-id="3063e-201">If the device does not exist, an error is written to the log file.</span></span> <br/><span data-ttu-id="3063e-202">Ha egy **ETag** eltérés, hiba ír a naplófájlba írást.</span><span class="sxs-lookup"><span data-stu-id="3063e-202">If there is an **ETag** mismatch, an error is written to the log file.</span></span> |
| <span data-ttu-id="3063e-203">**createOrUpdateIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="3063e-203">**createOrUpdateIfMatchETag**</span></span> |<span data-ttu-id="3063e-204">Ha egy eszköz nem létezik a megadott **azonosító**, újonnan regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="3063e-204">If a device does not exist with the specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="3063e-205">Ha az eszköz már létezik, meglévő információt felülírja a megadott bemeneti adatok csak akkor, ha van egy **ETag** felel meg.</span><span class="sxs-lookup"><span data-stu-id="3063e-205">If the device already exists, existing information is overwritten with the provided input data only if there is an **ETag** match.</span></span> <br/><span data-ttu-id="3063e-206">Ha egy **ETag** eltérés, hiba ír a naplófájlba írást.</span><span class="sxs-lookup"><span data-stu-id="3063e-206">If there is an **ETag** mismatch, an error is written to the log file.</span></span> <br> <span data-ttu-id="3063e-207">A felhasználó megadja a két adatok az eszköz adatokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="3063e-207">The user can optionally specify twin data along with the device data.</span></span> <span data-ttu-id="3063e-208">A kettős etag, ha meg van adva, akkor feldolgozott egymástól függetlenül az eszköz etag.</span><span class="sxs-lookup"><span data-stu-id="3063e-208">The twin’s etag, if specified, is processed independently from the device’s etag.</span></span> <span data-ttu-id="3063e-209">Ha eltérést okoz a meglévő iker etag, egy hiba kerül a naplófájlba írást.</span><span class="sxs-lookup"><span data-stu-id="3063e-209">If there is a mismatch with the existing twin’s etag, an error is written to the log file.</span></span> |
| <span data-ttu-id="3063e-210">**törlés**</span><span class="sxs-lookup"><span data-stu-id="3063e-210">**delete**</span></span> |<span data-ttu-id="3063e-211">Ha egy eszköz már létezik a megadott **azonosító**, nélkül tekintettel törlődik a **ETag** érték.</span><span class="sxs-lookup"><span data-stu-id="3063e-211">If a device already exists with the specified **id**, it is deleted without regard to the **ETag** value.</span></span> <br/><span data-ttu-id="3063e-212">Ha az eszköz nem létezik, egy hiba kerül a naplófájlba írást.</span><span class="sxs-lookup"><span data-stu-id="3063e-212">If the device does not exist, an error is written to the log file.</span></span> |
| <span data-ttu-id="3063e-213">**deleteIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="3063e-213">**deleteIfMatchETag**</span></span> |<span data-ttu-id="3063e-214">Ha egy eszköz már létezik a megadott **azonosító**, törlése, csak akkor, ha van egy **ETag** felel meg.</span><span class="sxs-lookup"><span data-stu-id="3063e-214">If a device already exists with the specified **id**, it is deleted only if there is an **ETag** match.</span></span> <span data-ttu-id="3063e-215">Ha az eszköz nem létezik, egy hiba kerül a naplófájlba írást.</span><span class="sxs-lookup"><span data-stu-id="3063e-215">If the device does not exist, an error is written to the log file.</span></span> <br/><span data-ttu-id="3063e-216">Ha az egy ETag nem egyezik, egy hiba kerül a naplófájlba írást.</span><span class="sxs-lookup"><span data-stu-id="3063e-216">If there is an ETag mismatch, an error is written to the log file.</span></span> |

> [!NOTE]
> <span data-ttu-id="3063e-217">Ha a szerializációs adatok nem definiálhat explicit módon egy **amelyben a importMode** jelző egy eszközhöz, alapértelmezés szerint a **createOrUpdate** az importálási művelet során.</span><span class="sxs-lookup"><span data-stu-id="3063e-217">If the serialization data does not explicitly define an **importMode** flag for a device, it defaults to **createOrUpdate** during the import operation.</span></span>

## <a name="import-devices-example--bulk-device-provisioning"></a><span data-ttu-id="3063e-218">Eszközök például importálása – tömeges eszköz kiépítése</span><span class="sxs-lookup"><span data-stu-id="3063e-218">Import devices example – bulk device provisioning</span></span>

<span data-ttu-id="3063e-219">A következő C# kódminta bemutatja, hogyan hozható létre több eszközt identitás, amely:</span><span class="sxs-lookup"><span data-stu-id="3063e-219">The following C# code sample illustrates how to generate multiple device identities that:</span></span>

* <span data-ttu-id="3063e-220">Például a hitelesítési kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="3063e-220">Include authentication keys.</span></span>
* <span data-ttu-id="3063e-221">Adott eszköz kapcsolatos adatokat ír az blokkblobba.</span><span class="sxs-lookup"><span data-stu-id="3063e-221">Write that device information to a block blob.</span></span>
* <span data-ttu-id="3063e-222">Az eszközök importálnia kell az identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="3063e-222">Import the devices into the identity registry.</span></span>

```csharp
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
  // Create a new ExportImportDevice
  // CryptoKeyGenerator is in the Microsoft.Azure.Devices.Common namespace
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

  // Add device to the list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write the list to the blob
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

// Call import using the blob to add new devices
// Log information related to the job is written to the same container
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

## <a name="import-devices-example--bulk-deletion"></a><span data-ttu-id="3063e-223">Eszközök például – tömeges törlésének importálása</span><span class="sxs-lookup"><span data-stu-id="3063e-223">Import devices example – bulk deletion</span></span>

<span data-ttu-id="3063e-224">A következő példakód bemutatja, hogyan használja az előző példakód hozzáadott eszközök törlése:</span><span class="sxs-lookup"><span data-stu-id="3063e-224">The following code sample shows you how to delete the devices you added using the previous code sample:</span></span>

```csharp
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
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

// Step 3: Call import using the same blob to delete all devices
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

## <a name="get-the-container-sas-uri"></a><span data-ttu-id="3063e-225">A tároló SAS URI-JÁNAK beolvasása</span><span class="sxs-lookup"><span data-stu-id="3063e-225">Get the container SAS URI</span></span>

<span data-ttu-id="3063e-226">A következő példakód bemutatja, hogyan létrehozni egy [SAS URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) az olvasási, írási és törlési engedélyek egy blob-tároló:</span><span class="sxs-lookup"><span data-stu-id="3063e-226">The following code sample shows you how to generate a [SAS URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) with read, write, and delete permissions for a blob container:</span></span>

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}
```

## <a name="next-steps"></a><span data-ttu-id="3063e-227">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3063e-227">Next steps</span></span>

<span data-ttu-id="3063e-228">Ebben a cikkben megtanulta, hogyan hajthat végre az identitásjegyzékhez tömeges műveleteket a az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="3063e-228">In this article, you learned how to perform bulk operations against the identity registry in an IoT hub.</span></span> <span data-ttu-id="3063e-229">Az alábbi hivatkozásokból tudhat meg többet az Azure IoT Hub kezelése:</span><span class="sxs-lookup"><span data-stu-id="3063e-229">Follow these links to learn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="3063e-230">[Az IoT-központ metrikák][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="3063e-230">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="3063e-231">[Figyelési műveletek][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="3063e-231">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="3063e-232">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="3063e-232">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="3063e-233">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="3063e-233">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="3063e-234">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="3063e-234">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
