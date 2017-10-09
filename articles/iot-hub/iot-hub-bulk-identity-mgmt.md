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
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a>Az IoT Hub eszköz identitásai tömeges kezelése

Minden egyes IoT-központ rendelkezik egy identitásjegyzékhez toocreate eszközönkénti erőforrások hello szolgáltatásban is használhatja. hello identitásjegyzékhez is lehetővé teszi toocontrol hozzáférés toohello eszköz felé néző végpontok. Ez a cikk ismerteti, hogyan tooimport és export eszköz identitásokat a tömeges tooand egy identitás beállításjegyzékből.

Importálás és exportálás zajlanak hello környezetében *feladatok* , amelyek tooexecute tömeges szolgáltatás műveleteket az IoT-központ lehetővé teszik.

Hello **RegistryManager** osztályba tartozik hello **ExportDevicesAsync** és **ImportDevicesAsync** módszereket, amelyek hello **feladat** keretrendszer. Ezek a módszerek lehetővé teszik a tooexport, az importálás, és az IoT hub identitásjegyzékhez hello a teljes szinkronizálás.

## <a name="what-are-jobs"></a>Mik azok a feladatok?

Identitás kapcsolatos műveletek használata hello **feladat** rendszer amikor hello a művelethez:

* Rendelkezik potenciálisan hosszú végrehajtási időt toostandard futásidejű műveletek képest.
* Nagy mennyiségű adatok toohello felhasználói adja vissza.

Egyetlen API-hívással Várakozás vagy blokkolja a hello hello művelet eredményét, helyett hello művelet aszinkron módon létrehoz egy **feladat** , hogy az IoT hub. hello műveletet, majd azonnal értéket ad vissza egy **JobProperties** objektum.

következő C# hello code kódrészletet mutat be, hogyan toocreate exportálási feladat:

```csharp
// Call an export job on hello IoT Hub tooretrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> toouse hello **RegistryManager** osztály a C#-kódban, vegye fel a hello **Microsoft.Azure.Devices** NuGet csomag tooyour projekt. Hello **RegistryManager** osztály szerepel hello **Microsoft.Azure.Devices** névtér.

Használhatja a hello **RegistryManager** tooquery hello állapotának hello osztály **feladat** visszaadott hello segítségével **JobProperties** metaadatok.

hello következő C# kódrészletet bemutatja, hogyan toopoll minden öt másodpercenként toosee Ha hello feladat végrehajtása befejeződött:

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

## <a name="export-devices"></a>Eszközök exportálása

Használjon hello **ExportDevicesAsync** metódus tooexport hello a teljes az IoT hub identitás beállításjegyzék tooan [Azure Storage](../storage/index.md) blob tároló használata egy [közös hozzáférésű Jogosultságkód](../storage/common/storage-security-guide.md#data-plane-security).

Ez a módszer lehetővé teszi az eszköz adatait egy blob-tároló, amelyek toocreate megbízható másolatait.

Hello **ExportDevicesAsync** módszernél két paramétert:

* A *karakterlánc* , amely tartalmazza a blob-tároló URI. Ezt az URI, amely engedélyezi az írási hozzáférést toohello tároló SAS-jogkivonat kell tartalmaznia. hello feldolgozás egy blokkblob a tároló toostore szerializált hello export eszköz adatokat hoz létre. hello SAS-jogkivonat tartalmaznia kell azokat az engedélyeket:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

* A *logikai* , amely azt jelzi, ha azt szeretné, tooexclude hitelesítési kulcsokat az exportálás adatok alapján. Ha **hamis**, hitelesítési kulcsokat exportálás kimeneti szerepelnek. Ellenkező esetben kulcsok exportálása **null**.

hello következő C# kódrészletet bemutatja, hogyan tooinitiate hello eszköz hitelesítési kulcsokat tartalmazó exportálási feladat adatok exportálása és majd kérdezze le az Befejezés:

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

hello feladat tárolja a kimeneti blob-tárolóban megadott hello a blokkblob hello nevű **devices.txt**. soronként egy eszközzel szerializált JSON eszközadatok, hello kimeneti adatok állnak.

hello alábbi példa bemutatja hello kimeneti adatokat:

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Ha egy eszköz iker adatokat, majd hello iker adatok is exportált hello eszközadatok együtt. hello következő példa bemutatja, ezt a formátumot. Adatok teljes hello "twinETag" sor hello végéig olyan iker adatok.

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

Ha kód toothis adatokat kell használni, akkor könnyen deszerializálni a ezeket az adatokat hello **ExportImportDevice** osztály. hello következő C# kódrészletet bemutatja, hogyan tooread eszközadatokat, de a korábban exportált tooa blokkblob:

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
> Is használhatja a hello **GetDevicesAsync** hello metódusában **RegistryManager** osztály toofetch az eszközök listáját. Azonban ez a megközelítés azzal 1000 rögzített cap visszaadott eszközobjektumok hello számára. hello várható használati eset a hello **GetDevicesAsync** metódus fejlesztési forgatókönyveket tooaid hibakeresési, és nem ajánlott a termelési számítási feladatokhoz.

## <a name="import-devices"></a>Eszközök importálása

Hello **ImportDevicesAsync** metódus a hello **RegistryManager** osztály tooperform tömeges importálása és szinkronizálás az IoT hub identitásjegyzékhez a műveletek segítségével. Például a hello **ExportDevicesAsync** metódust, hello **ImportDevicesAsync** metódusnak hello **feladat** keretrendszer.

Mi gondoskodunk hello segítségével **ImportDevicesAsync** metódus, mert a hozzáadása tooprovisioning új eszközöket az a identitásjegyzékhez, is frissíteni és meglévő eszközök törlése.

> [!WARNING]
> Az importálási művelet nem vonható vissza. Mindig készítsen biztonsági másolatot a meglévő adatok hello **ExportDevicesAsync** metódus tooanother blob tároló előtt tömeges módosítása tooyour identitásjegyzékhez.

Hello **ImportDevicesAsync** metódus két paramétereket fogadja:

* A *karakterlánc* , amely tartalmazza egy URI-azonosítója egy [Azure Storage](../storage/index.md) blob-tároló toouse, *bemeneti* toohello feladat. Ezt az URI toohello tároló olvasási hozzáférést biztosít egy SAS-jogkivonatot kell tartalmaznia. Ez a tároló tartalmaznia kell egy blob hello nevű **devices.txt** szerializált hello eszköz adatok tooimport, amely tartalmazza azokat az identitásjegyzékhez. hello adatokat importálhat adatokat kell tartalmazniuk eszköz a hello azonos JSON formátumú, hogy hello **ExportImportDevice** feladatot használ, amikor létrehozza a **devices.txt** blob. hello SAS-jogkivonat tartalmaznia kell azokat az engedélyeket:

   ```csharp
   SharedAccessBlobPermissions.Read
   ```
* A *karakterlánc* , amely tartalmazza egy URI-azonosítója egy [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob-tároló toouse, *kimeneti* hello feladatból. hello feladatot hoz létre a blokkblob a tároló toostore bármely hibainformációk befejeződött hello importálásból **feladat**. hello SAS-jogkivonat tartalmaznia kell azokat az engedélyeket:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> a két hello paraméterek mutathat toohello blob tárolóhoz. hello külön paraméterek egyszerűen további szabályozásának engedélyezése a adatok hello kimeneti tároló jogosultságokra van szüksége.

a következő C# hello hogyan code kódrészletet mutat be egy importálási feladat tooinitiate:

```csharp
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

Ez a módszer is hello eszköz iker használt tooimport hello adatait. hello adatbevitel hello formátuma van hello azonos hello látható hello formátumban **ExportDevicesAsync** szakasz. Ezzel a módszerrel importálja újra hello exportált adatokat. Hello **$metadata** nem kötelező megadni.

## <a name="import-behavior"></a>Importálás viselkedése

Használhatja a hello **ImportDevicesAsync** metódus tooperform hello következő tömeges a identitásjegyzékhez műveletek:

* Az új eszközök tömeges regisztrálása
* Meglévő eszközök tömeges törlése
* Tömeges állapotmódosítások (engedélyezése vagy letiltása az eszközök)
* Új eszköz hitelesítési kulcsokat tömeges hozzárendelése
* Tömeges eszköz hitelesítési kulcsokat, automatikus-újragenerálása
* Tömeges frissítés iker adatok

Hajthat végre műveletek egy megelőző hello kombinációja **ImportDevicesAsync** hívható meg. Például új eszközöket regisztrálni és törölheti vagy frissítheti a meglévő eszközöket: hello ugyanannyi időt vesz igénybe. Hello együtt használva **ExportDevicesAsync** módszer, egy IoT hub tooanother teljesen az eszközök telepíthet át.

Ha hello importfájl iker metaadatokat tartalmaz, a metaadatok hello meglévő iker metaadatok felülírja. Ha hello importfájl nem tartalmaz iker metaadatokat, majd csak hello `lastUpdateTime` metaadatok frissül hello segítségével a jelenlegi időpontnál.

Választható használata hello **amelyben a importMode** hello a szerializálási adatok importálása az egyes eszközök toocontrol hello importálási folyamat eszközönkénti tulajdonságot. Hello **amelyben a importMode** tulajdonságnak hello a következő beállításokat:

| amelyben a importMode | Leírás |
| --- | --- |
| **createOrUpdate** |Ha egy eszköz nem létezik a megadott hello **azonosító**, újonnan regisztrálva van. <br/>Ha hello eszköz már létezik, a megadott bemeneti adatok nélkül legutóbb toohello hello felülírja a meglévő adatokat **ETag** érték. <br> hello felhasználó nem kötelezően megadhatja a kettős adatok hello eszköz adatokkal együtt. hello iker etag, ha meg van adva, akkor feldolgozott egymástól függetlenül hello eszköz etag. Ha a hello meglévő iker etag nem egyezik, egy hiba kerül toohello naplófájlt. |
| **létrehozás** |Ha egy eszköz nem létezik a megadott hello **azonosító**, újonnan regisztrálva van. <br/>Már létezik hello eszköz, ha egy hiba kerül toohello naplófájlt. <br> hello felhasználó nem kötelezően megadhatja a kettős adatok hello eszköz adatokkal együtt. hello iker etag, ha meg van adva, akkor feldolgozott egymástól függetlenül hello eszköz etag. Ha a hello meglévő iker etag nem egyezik, egy hiba kerül toohello naplófájlt. |
| **frissítés** |Ha egy eszköz már létezik a megadott hello **azonosító**, felülírja a meglévő adatokat a megadott bemeneti adatok nélkül legutóbb toohello hello **ETag** érték. <br/>Ha hello eszköz nem létezik, egy hiba kerül toohello naplófájlt. |
| **updateIfMatchETag** |Ha egy eszköz már létezik a megadott hello **azonosító**, csak akkor, ha nincs a megadott bemeneti adatok hello felülírja a meglévő adatokat egy **ETag** felel meg. <br/>Ha hello eszköz nem létezik, egy hiba kerül toohello naplófájlt. <br/>Ha egy **ETag** eltérés, egy hiba kerül toohello naplófájlt. |
| **createOrUpdateIfMatchETag** |Ha egy eszköz nem létezik a megadott hello **azonosító**, újonnan regisztrálva van. <br/>Ha hello eszköz már létezik, csak akkor, ha nincs a megadott bemeneti adatok hello felülírja a meglévő adatokat egy **ETag** felel meg. <br/>Ha egy **ETag** eltérés, egy hiba kerül toohello naplófájlt. <br> hello felhasználó nem kötelezően megadhatja a kettős adatok hello eszköz adatokkal együtt. hello iker etag, ha meg van adva, akkor feldolgozott egymástól függetlenül hello eszköz etag. Ha a hello meglévő iker etag nem egyezik, egy hiba kerül toohello naplófájlt. |
| **törlés** |Ha egy eszköz már létezik a megadott hello **azonosító**, figyelembe véve toohello nélkül törli **ETag** érték. <br/>Ha hello eszköz nem létezik, egy hiba kerül toohello naplófájlt. |
| **deleteIfMatchETag** |Ha egy eszköz már létezik a megadott hello **azonosító**, törlése, csak akkor, ha van egy **ETag** felel meg. Ha hello eszköz nem létezik, egy hiba kerül toohello naplófájlt. <br/>Ha az egy ETag nem egyezik, egy hiba kerül toohello naplófájlt. |

> [!NOTE]
> Ha hello szerializációs adatokból nem definiálhat explicit módon egy **amelyben a importMode** jelző egy eszközhöz, alapértelmezés szerint túl**createOrUpdate** hello során az importálási művelet.

## <a name="import-devices-example--bulk-device-provisioning"></a>Eszközök például importálása – tömeges eszköz kiépítése

hello alábbi C# mintakód bemutatja, hogyan toogenerate több eszközt identitás, amely:

* Például a hitelesítési kulcsokat.
* Adott eszköz információk tooa blokkblob írása.
* Hello eszközök importálása hello identitásjegyzékhez.

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

## <a name="import-devices-example--bulk-deletion"></a>Eszközök például – tömeges törlésének importálása

hello következő mintakód bemutatja, hogyan használatával hozzáadott toodelete hello eszközök hello előző példakód:

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

## <a name="get-hello-container-sas-uri"></a>Hello tároló SAS URI-JÁNAK beolvasása

hello következő mintakód bemutatja, hogyan toogenerate egy [SAS URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) az olvasási, írási és törlési engedélyek egy blob-tároló:

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

## <a name="next-steps"></a>Következő lépések

Ebben a cikkben megtanulta, hogyan tooperform tömeges hello identitásjegyzékhez az IoT-központ a műveleteket. Kövesse az alábbi hivatkozások toolearn Azure IoT Hub kezelésével kapcsolatos további:

* [Az IoT-központ metrikák][lnk-metrics]
* [Figyelési műveletek][lnk-monitor]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [IoT Hub fejlesztői útmutató][lnk-devguide]
* [Egy eszköz szimulálva IoT oldala][lnk-iotedge]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
