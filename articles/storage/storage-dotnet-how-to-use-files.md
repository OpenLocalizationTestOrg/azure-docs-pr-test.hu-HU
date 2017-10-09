---
title: az Azure File storage a .NET aaaDevelop |} Microsoft Docs
description: "Ismerje meg, hogyan toodevelop .NET alkalmazások és szolgáltatások, Azure File storage toostore használó fájladatok."
services: storage
documentationcenter: .net
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6a889ee1-1e60-46ec-a592-ae854f9fb8b6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: aa8f84f1c93249055370e2a2cb33d7118b972be1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-net"></a>Fejlesztés az Azure File Storage szolgáltatáshoz a .NET keretrendszerrel 
> [!NOTE]
> Ez a cikk bemutatja, hogyan toomanage Azure File storage a .NET-kódot. További információ az Azure File storage toolearn tekintse meg a hello [a File storage bemutatása tooAzure](storage-files-introduction.md).
>

[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

## <a name="about-this-tutorial"></a>Az oktatóanyag ismertetése
Ez az oktatóanyag mutatni hello használata a .NET toodevelop alkalmazásokhoz vagy szolgáltatásokhoz, használja az Azure File storage toostore fájladatok alapjait. Ebben az oktatóanyagban a rendszer létrehoz egy egyszerű konzolalkalmazásként és megjelenítése hogyan tooperform alapszintű műveleteket az .NET és az Azure File storage szolgáltatással:

* Hello fájl tartalmának beolvasása
* Hello fájlmegosztás hello kvótájának (maximális méretének) beállítása.
* Hozzon létre egy megosztott elérési házirendet hello megosztáson definiált használó fájl a közös hozzáférésű jogosultságkód (SAS-kulcsot).
* Fájl tooanother fájl másolatát hello ugyanazt a tárfiókot.
* Egy fájl tooa blob másolása hello ugyanazt a tárfiókot.
* Hibaelhárítás az Azure Storage Metrics segítségével

> [!Note]  
> Előfordulhat, hogy Azure fájltároló SMB-n keresztül érhető el, mert lehetséges toowrite hello Azure fájlmegosztás fájl i/o hello szabványos System.IO osztály használatával elérhető egyszerű alkalmazásokat is. Ez a cikk ismerteti, hogyan toowrite használó alkalmazások hello Azure Storage .NET SDK-t, hello használó [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure fájlok tárolására. 


## <a name="create-hello-console-application-and-obtain-hello-assembly"></a>Hello Konzolalkalmazás létrehozása és hello szerelvény beszerzése
Hozzon létre egy új Windows-konzolalkalmazást a Visual Studióban. hello lépések bemutatják, hogyan toocreate egy konzolalkalmazást a Visual Studio 2017, azonban hello lépések hasonlóak a Visual Studio más verzióiban.

1. Válassza a **File** (Fájl) > **New** (Új) > **Project** (Projekt) lehetőséget.
2. Válassza az **Installed** (Telepítve) > **Templates** (Sablonok) > **Visual C#** > **Windows Classic Desktop** (Windows klasszikus asztal) lehetőséget
3. Válassza a **Console App (.NET Framework)** (Konzolalkalmazás (.NET keretrendszer) lehetőséget
4. Adjon meg egy nevet az alkalmazásnak hello **Name:** mező
5. Kattintson az **OK** gombra.

Minden ebben az oktatóanyagban szereplő példák hozzáadhatók toohello `Main()` a Konzolalkalmazás metódusában `Program.cs` fájlt.

Hello Azure Storage ügyféloldali kódtárat bármilyen típusú .NET-alkalmazás, beleértve az Azure felhőalapú szolgáltatás, vagy a webes alkalmazás, és az asztali és mobil alkalmazások használható. Ebben az útmutatóban az egyszerűség kedvéért egy konzolalkalmazást használunk.

## <a name="use-nuget-tooinstall-hello-required-packages"></a>NuGet tooinstall szükséges hello csomagok használata
Nincsenek a projekt toocomplete ebben az oktatóanyagban szükséges tooreference két csomagok:

* [A Microsoft Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): Ez a csomag programozott hozzáférést biztosít a tárfiókban lévő toodata erőforrásokat.
* [A Microsoft Azure Configuration Manager könyvtár a .NET-hez](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): Ez a csomag egy osztályt biztosít a konfigurációs fájlban található kapcsolati karakterlánc elemzéséhez, függetlenül attól, hogy az alkalmazás hol fut.

Használhat NuGet tooobtain mindkét csomagot. Kövesse az alábbi lépéseket:

1. Kattintson a jobb gombbal a projektjére a **Megoldáskezelőben**, és válassza a **Manage NuGet Packages** (NuGet-csomagok kezelése) lehetőséget.
2. Keresse rá az interneten a "windowsazure.Storage"kifejezésre, és kattintson a **telepítése** tooinstall hello a Storage ügyféloldali kódtára és annak függőségeit.
3. Keresse rá az interneten a "WindowsAzure.ConfigurationManager", és kattintson a **telepítése** tooinstall hello Azure Configuration Manager.

## <a name="save-your-storage-account-credentials-toohello-appconfig-file"></a>A tárolási fiók hitelesítő adatait toohello app.config fájl mentése
Mentse el a hitelesítő adatokat a projekt app.config fájljába. Hello app.config fájl szerkesztése, úgy, hogy a következő példa, hogy hasonló toohello `myaccount` rendelkező a tárfiók nevére, és `mykey` rendelkező a tárfiók kulcsára.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=StorageAccountKeyEndingIn==" />
    </appSettings>
</configuration>
```

> [!NOTE]
> hello hello az Azure storage emulator legújabb verziója nem támogatja az Azure File storage. A kapcsolati karakterláncot kell használnia célként egy Azure Storage-fiókot hello felhő toowork az Azure File storage szolgáltatással.

## <a name="add-using-directives"></a>Hozzáadás irányelvekkel
Nyissa meg hello `Program.cs` fájlt a Megoldáskezelőből, és adja hozzá a következő hello irányelvek toohello felső hello fájl használatával.

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Azure Blobs
using Microsoft.WindowsAzure.Storage.File; // Namespace for Azure File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="access-hello-file-share-programmatically"></a>Hozzáférés hello fájlmegosztás szoftveres
Ezután adja hozzá a következő kód toohello hello `Main()` (után hello a fent látható kód) metódus tooretrieve hello kapcsolati karakterláncot. Ezzel a kóddal lekérdezi egy referencia-toohello korábban létrehozott fájlra, és kiírja a tartalom toohello console ablakban.

```csharp
// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    // Get a reference toohello root directory for hello share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference toohello directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that hello directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference toohello file we created previously.
        CloudFile file = sampleDir.GetFileReference("Log1.txt");

        // Ensure that hello file exists.
        if (file.Exists())
        {
            // Write hello contents of hello file toohello console window.
            Console.WriteLine(file.DownloadTextAsync().Result);
        }
    }
}
```

Hello console application toosee hello kimeneti futtatásához.

## <a name="set-hello-maximum-size-for-a-file-share"></a>Fájlmegosztás hello maximális méretének beállítása
Verziójától hello Azure Storage ügyféloldali kódtár 5.x-es, állíthatja be hello kvótát (vagy maximális méretet) egy fájlmegosztáshoz GB-ban. Mennyi adatot hello megosztáson tárolt toosee is ellenőrizheti.

Által beállítás hello kvótát egy megosztáshoz korlátozhatja a hello hello tárolják hello fájlok összesített mérete. Ha hello hello megosztáson található fájlok összesített mérete meghaladja hello kvóta hello megosztáson, akkor az ügyfelek meglévő fájlok mérete nem lehet tooincrease hello vagy fogja új fájlok létrehozása, kivéve azokat, amelyek üresek.

hello az alábbi példa bemutatja, hogyan toocheck hello egy megosztás aktuális kihasználását, és hogyan tooset hello hello fájlmegosztás kvótája.

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    // Check current usage stats for hello share.
    // Note that hello ShareStats object is part of hello protocol layer for hello File service.
    Microsoft.WindowsAzure.Storage.File.Protocol.ShareStats stats = share.GetStats();
    Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

    // Specify hello maximum size of hello share, in GB.
    // This line sets hello quota toobe 10 GB greater than hello current usage of hello share.
    share.Properties.Quota = 10 + stats.Usage;
    share.SetProperties();

    // Now check hello quota for hello share. Call FetchAttributes() toopopulate hello share's properties.
    share.FetchAttributes();
    Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
}
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Közös hozzáférésű jogosultságkód létrehozása egy fájlhoz vagy fájlmegosztáshoz
Verziójától 5.x-es hello Azure Storage ügyféloldali kódtár, létrehozhat egy közös hozzáférésű jogosultságkód (SAS) egy fájlmegosztáshoz vagy fájlhoz. Is létrehozhat egy megosztott elérési házirendet egy fájlmegosztási toomanage megosztott hozzáférést aláírások. Megosztott hozzáférési házirend létrehozása az ajánlott, mivel hello SAS visszavonása, ha kell sérülhet módszert biztosít.

a következő példa hello hoz létre egy megosztott elérési házirendet egy megosztáson, és akkor használja, hogy használ-e tooprovide hello Házirendmegkötések tartozó SAS korlátozására található hello fájlhoz.

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

    // Create a new shared access policy and define its constraints.
    SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
        };

    // Get existing permissions for hello share.
    FileSharePermissions permissions = share.GetPermissions();

    // Add hello shared access policy toohello share's policies. Note that each policy must have a unique name.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    share.SetPermissions(permissions);

    // Generate a SAS for a file in hello share and associate this access policy with it.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
    CloudFile file = sampleDir.GetFileReference("Log1.txt");
    string sasToken = file.GetSharedAccessSignature(null, policyName);
    Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

    // Create a new CloudFile object from hello SAS, and write some text toohello file.
    CloudFile fileSas = new CloudFile(fileSasUri);
    fileSas.UploadText("This write operation is authenticated via SAS.");
    Console.WriteLine(fileSas.DownloadText());
}
```

További információk a közös hozzáférésű jogosultságkód létrehozásáról és használatáról: [Közös hozzáférésű jogosultságkódok (SAS)](storage-dotnet-shared-access-signature-part-1.md) és [SAS létrehozása az Azure-blobok segítségével](storage-dotnet-shared-access-signature-part-2.md).

## <a name="copy-files"></a>Fájlok másolása
Verziójától 5.x-es hello Azure Storage ügyféloldali kódtár, egy fájl tooanother, egy fájl tooa blob vagy egy blob tooa fájl másolhatja. Hello következő szakaszokban bemutatjuk, hogyan tooperform ezek másolása műveletek programozott módon.

AzCopy toocopy egy fájl tooanother vagy toocopy egy blob tooa fájlba vagy fordítva fordítva is használható. Lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md).

> [!NOTE]
> Egy blob tooa fájlt, vagy a fájl tooa blob másolása, még akkor is, ha a másolt hello belül azonos kell használnia egy közös hozzáférésű jogosultságkód (SAS) tooauthenticate hello adatforrás-objektum, a tárfiók.
> 
> 

**Másolja át a fájlt tooanother fájlt** hello alábbi példa átmásolja a terjesztendő egy fájl tooanother hello ugyanennek a megosztásnak. Mivel a másolási művelet másolja a fájlokat hello ugyanazt a tárfiókot, a megosztott kulcsos hitelesítést tooperform hello másolatot használja.

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    // Get a reference toohello root directory for hello share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference toohello directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that hello directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference toohello file we created previously.
        CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

        // Ensure that hello source file exists.
        if (sourceFile.Exists())
        {
            // Get a reference toohello destination file.
            CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

            // Start hello copy operation.
            destFile.StartCopy(sourceFile);

            // Write hello contents of hello destination file toohello console window.
            Console.WriteLine(destFile.DownloadText());
        }
    }
}
```

**Másolja a fájlt tooa blob** hello alábbi példa létrehoz egy fájlt, és átmásolja tooa blob hello belül ugyanazt a tárfiókot. hello példa létrehoz egy SAS-hello forrás fájl, mely hello szolgáltatás használja tooauthenticate hozzáférés toohello forrásfájl hello másolási művelet során.

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Create a new file share, if it does not already exist.
CloudFileShare share = fileClient.GetShareReference("sample-share");
share.CreateIfNotExists();

// Create a new file in hello root directory.
CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
sourceFile.UploadText("A sample file in hello root directory.");

// Get a reference toohello blob toowhich hello file will be copied.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();
CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

// Create a SAS for hello file that's valid for 24 hours.
// Note that when you are copying a file tooa blob, or a blob tooa file, you must use a SAS
// tooauthenticate access toohello source object, even if you are copying within hello same
// storage account.
string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
{
    // Only read permissions are required for hello source file.
    Permissions = SharedAccessFilePermissions.Read,
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
});

// Construct hello URI toohello source file, including hello SAS token.
Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

// Copy hello file toohello blob.
destBlob.StartCopy(fileSasUri);

// Write hello contents of hello file toohello console window.
Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

A blob tooa fájl másolhatja a hello azonos módon. Ha hello Forrásobjektum egy blob, majd létre egy SAS tooauthenticate hozzáférés toothat blobot hello másolási művelet során.

## <a name="troubleshooting-azure-file-storage-using-metrics"></a>Az Azure File Storage hibaelhárítása mérőszámok segítségével
Az Azure Storage Analytics mostantól az Azure File Storage esetén is támogatja a mérőszámok használatát. A metrikai adatok segítségével nyomon követheti a kéréseket, és diagnosztizálhatja a problémákat.


Engedélyezheti a metrikák hello Azure File Storage [Azure Portal](https://portal.azure.com). Programozott módon által hívó hello beállítása File Service Properties művelet hello REST API-n keresztül, vagy a Storage ügyféloldali kódtár hello kódtárában egyik metrikák is engedélyezheti.


a következő példakód hello bemutatja, hogyan toouse hello a Storage ügyféloldali kódtára a .NET tooenable Azure File storage mérőszámait.

Először adja hozzá a hello következő `using` irányelvek tooyour `Program.cs` , továbbá korábban ismertetett módon hozzáadott toothose fájlt:

```csharp
using Microsoft.WindowsAzure.Storage.File.Protocol;
using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

Vegye figyelembe, hogy Azure BLOB, Azure-tábla és Azure várólisták megosztott hello használata `ServiceProperties` hello típusának `Microsoft.WindowsAzure.Storage.Shared.Protocol` névtér, Azure File storage a saját típust használja, hello `FileServiceProperties` hello típusának `Microsoft.WindowsAzure.Storage.File.Protocol` névtér. Mindkét névtérre hivatkozni kell a felhasználói kódból, azonban a következő kód toocompile hello.

```csharp
// Parse your storage connection string from your application's configuration file.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
// Create hello File service client.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Set metrics properties for File service.
// Note that hello File service currently uses its own service properties type,
// available in hello Microsoft.WindowsAzure.Storage.File.Protocol namespace.
fileClient.SetServiceProperties(new FileServiceProperties()
{
    // Set hour metrics
    HourMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 14,
        Version = "1.0"
    },
    // Set minute metrics
    MinuteMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 7,
        Version = "1.0"
    }
});

// Read hello metrics properties we just set.
FileServiceProperties serviceProperties = fileClient.GetServiceProperties();
Console.WriteLine("Hour metrics:");
Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
Console.WriteLine(serviceProperties.HourMetrics.Version);
Console.WriteLine();
Console.WriteLine("Minute metrics:");
Console.WriteLine(serviceProperties.MinuteMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.MinuteMetrics.RetentionDays);
Console.WriteLine(serviceProperties.MinuteMetrics.Version);
```

Emellett olvassa el a túl[Azure File storage hibaelhárítási cikk](storage-troubleshoot-file-connection-problems.md) végpont hibaelhárítási útmutatót.

## <a name="next-steps"></a>Következő lépések
Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.

### <a name="conceptual-articles-and-videos"></a>Elméleti cikkek és videók
* [Azure File Storage: zökkenőmentes felhőalapú SMB fájlrendszer Windows és Linux rendszerekhez](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Hogyan toouse Azure File storage Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>File Storage-eszköztámogatás
* [Using Azure PowerShell with Azure Storage (Az Azure PowerShell és az Azure Storage együttes használata)](storage-powershell-guide-full.md)
* [Hogyan toouse Microsoft Azure Storage AzCopy](storage-use-azcopy.md)
* [Az Azure Storage hello Azure parancssori felület használatával](storage-azure-cli.md#create-and-manage-file-shares)
* [Az Azure File Storage-problémák hibaelhárítása](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a>Referencia
* [Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Referencia a fájlszolgáltatás REST API-jához](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a>Blogbejegyzések
* [Azure File storage is now generally available (Mostantól általánosan elérhető az Azure File Storage)](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Az Azure File Storage ismertetése](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introducing Microsoft Azure File Service (A Microsoft Azure File szolgáltatás bemutatása)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Persisting kapcsolatok tooMicrosoft Azure File storage](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)
