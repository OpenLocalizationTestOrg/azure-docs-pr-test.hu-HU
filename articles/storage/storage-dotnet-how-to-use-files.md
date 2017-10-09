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
# <a name="develop-for-azure-file-storage-with-net"></a><span data-ttu-id="5eb1f-103">Fejlesztés az Azure File Storage szolgáltatáshoz a .NET keretrendszerrel</span><span class="sxs-lookup"><span data-stu-id="5eb1f-103">Develop for Azure File storage with .NET</span></span> 
> [!NOTE]
> <span data-ttu-id="5eb1f-104">Ez a cikk bemutatja, hogyan toomanage Azure File storage a .NET-kódot.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-104">This article shows how toomanage Azure File storage with .NET code.</span></span> <span data-ttu-id="5eb1f-105">További információ az Azure File storage toolearn tekintse meg a hello [a File storage bemutatása tooAzure](storage-files-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5eb1f-105">toolearn more about Azure File storage, please see hello [Introduction tooAzure File storage](storage-files-introduction.md).</span></span>
>

[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="5eb1f-106">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="5eb1f-106">About this tutorial</span></span>
<span data-ttu-id="5eb1f-107">Ez az oktatóanyag mutatni hello használata a .NET toodevelop alkalmazásokhoz vagy szolgáltatásokhoz, használja az Azure File storage toostore fájladatok alapjait.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-107">This tutorial will demonstrate hello basics of using .NET toodevelop applications or services that use Azure File storage toostore file data.</span></span> <span data-ttu-id="5eb1f-108">Ebben az oktatóanyagban a rendszer létrehoz egy egyszerű konzolalkalmazásként és megjelenítése hogyan tooperform alapszintű műveleteket az .NET és az Azure File storage szolgáltatással:</span><span class="sxs-lookup"><span data-stu-id="5eb1f-108">In this tutorial, we will create a simple console application and show how tooperform basic actions with .NET and Azure File storage:</span></span>

* <span data-ttu-id="5eb1f-109">Hello fájl tartalmának beolvasása</span><span class="sxs-lookup"><span data-stu-id="5eb1f-109">Get hello contents of a file</span></span>
* <span data-ttu-id="5eb1f-110">Hello fájlmegosztás hello kvótájának (maximális méretének) beállítása.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-110">Set hello quota (maximum size) for hello file share.</span></span>
* <span data-ttu-id="5eb1f-111">Hozzon létre egy megosztott elérési házirendet hello megosztáson definiált használó fájl a közös hozzáférésű jogosultságkód (SAS-kulcsot).</span><span class="sxs-lookup"><span data-stu-id="5eb1f-111">Create a shared access signature (SAS key) for a file that uses a shared access policy defined on hello share.</span></span>
* <span data-ttu-id="5eb1f-112">Fájl tooanother fájl másolatát hello ugyanazt a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-112">Copy a file tooanother file in hello same storage account.</span></span>
* <span data-ttu-id="5eb1f-113">Egy fájl tooa blob másolása hello ugyanazt a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-113">Copy a file tooa blob in hello same storage account.</span></span>
* <span data-ttu-id="5eb1f-114">Hibaelhárítás az Azure Storage Metrics segítségével</span><span class="sxs-lookup"><span data-stu-id="5eb1f-114">Use Azure Storage Metrics for troubleshooting</span></span>

> [!Note]  
> <span data-ttu-id="5eb1f-115">Előfordulhat, hogy Azure fájltároló SMB-n keresztül érhető el, mert lehetséges toowrite hello Azure fájlmegosztás fájl i/o hello szabványos System.IO osztály használatával elérhető egyszerű alkalmazásokat is.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-115">Because Azure File storage may be accessed over SMB, it is possible toowrite simple applications that access hello Azure File share using hello standard System.IO classes for File I/O.</span></span> <span data-ttu-id="5eb1f-116">Ez a cikk ismerteti, hogyan toowrite használó alkalmazások hello Azure Storage .NET SDK-t, hello használó [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure fájlok tárolására.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-116">This article will describe how toowrite applications that use hello Azure Storage .NET SDK, which uses hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span></span> 


## <a name="create-hello-console-application-and-obtain-hello-assembly"></a><span data-ttu-id="5eb1f-117">Hello Konzolalkalmazás létrehozása és hello szerelvény beszerzése</span><span class="sxs-lookup"><span data-stu-id="5eb1f-117">Create hello console application and obtain hello assembly</span></span>
<span data-ttu-id="5eb1f-118">Hozzon létre egy új Windows-konzolalkalmazást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-118">In Visual Studio, create a new Windows console application.</span></span> <span data-ttu-id="5eb1f-119">hello lépések bemutatják, hogyan toocreate egy konzolalkalmazást a Visual Studio 2017, azonban hello lépések hasonlóak a Visual Studio más verzióiban.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-119">hello following steps show you how toocreate a console application in Visual Studio 2017, however, hello steps are similar in other versions of Visual Studio.</span></span>

1. <span data-ttu-id="5eb1f-120">Válassza a **File** (Fájl) > **New** (Új) > **Project** (Projekt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-120">Select **File** > **New** > **Project**</span></span>
2. <span data-ttu-id="5eb1f-121">Válassza az **Installed** (Telepítve) > **Templates** (Sablonok) > **Visual C#** > **Windows Classic Desktop** (Windows klasszikus asztal) lehetőséget</span><span class="sxs-lookup"><span data-stu-id="5eb1f-121">Select **Installed** > **Templates** > **Visual C#** > **Windows Classic Desktop**</span></span>
3. <span data-ttu-id="5eb1f-122">Válassza a **Console App (.NET Framework)** (Konzolalkalmazás (.NET keretrendszer) lehetőséget</span><span class="sxs-lookup"><span data-stu-id="5eb1f-122">Select **Console App (.NET Framework)**</span></span>
4. <span data-ttu-id="5eb1f-123">Adjon meg egy nevet az alkalmazásnak hello **Name:** mező</span><span class="sxs-lookup"><span data-stu-id="5eb1f-123">Enter a name for your application in hello **Name:** field</span></span>
5. <span data-ttu-id="5eb1f-124">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-124">Select **OK**</span></span>

<span data-ttu-id="5eb1f-125">Minden ebben az oktatóanyagban szereplő példák hozzáadhatók toohello `Main()` a Konzolalkalmazás metódusában `Program.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-125">All code examples in this tutorial can be added toohello `Main()` method of your console application's `Program.cs` file.</span></span>

<span data-ttu-id="5eb1f-126">Hello Azure Storage ügyféloldali kódtárat bármilyen típusú .NET-alkalmazás, beleértve az Azure felhőalapú szolgáltatás, vagy a webes alkalmazás, és az asztali és mobil alkalmazások használható.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-126">You can use hello Azure Storage Client Library in any type of .NET application, including an Azure cloud service or web app, and desktop and mobile applications.</span></span> <span data-ttu-id="5eb1f-127">Ebben az útmutatóban az egyszerűség kedvéért egy konzolalkalmazást használunk.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-127">In this guide, we use a console application for simplicity.</span></span>

## <a name="use-nuget-tooinstall-hello-required-packages"></a><span data-ttu-id="5eb1f-128">NuGet tooinstall szükséges hello csomagok használata</span><span class="sxs-lookup"><span data-stu-id="5eb1f-128">Use NuGet tooinstall hello required packages</span></span>
<span data-ttu-id="5eb1f-129">Nincsenek a projekt toocomplete ebben az oktatóanyagban szükséges tooreference két csomagok:</span><span class="sxs-lookup"><span data-stu-id="5eb1f-129">There are two packages you need tooreference in your project toocomplete this tutorial:</span></span>

* <span data-ttu-id="5eb1f-130">[A Microsoft Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): Ez a csomag programozott hozzáférést biztosít a tárfiókban lévő toodata erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-130">[Microsoft Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): This package provides programmatic access toodata resources in your storage account.</span></span>
* <span data-ttu-id="5eb1f-131">[A Microsoft Azure Configuration Manager könyvtár a .NET-hez](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): Ez a csomag egy osztályt biztosít a konfigurációs fájlban található kapcsolati karakterlánc elemzéséhez, függetlenül attól, hogy az alkalmazás hol fut.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-131">[Microsoft Azure Configuration Manager library for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): This package provides a class for parsing a connection string in a configuration file, regardless of where your application is running.</span></span>

<span data-ttu-id="5eb1f-132">Használhat NuGet tooobtain mindkét csomagot.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-132">You can use NuGet tooobtain both packages.</span></span> <span data-ttu-id="5eb1f-133">Kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5eb1f-133">Follow these steps:</span></span>

1. <span data-ttu-id="5eb1f-134">Kattintson a jobb gombbal a projektjére a **Megoldáskezelőben**, és válassza a **Manage NuGet Packages** (NuGet-csomagok kezelése) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-134">Right-click your project in **Solution Explorer** and choose **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="5eb1f-135">Keresse rá az interneten a "windowsazure.Storage"kifejezésre, és kattintson a **telepítése** tooinstall hello a Storage ügyféloldali kódtára és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-135">Search online for "WindowsAzure.Storage" and click **Install** tooinstall hello Storage Client Library and its dependencies.</span></span>
3. <span data-ttu-id="5eb1f-136">Keresse rá az interneten a "WindowsAzure.ConfigurationManager", és kattintson a **telepítése** tooinstall hello Azure Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-136">Search online for "WindowsAzure.ConfigurationManager" and click **Install** tooinstall hello Azure Configuration Manager.</span></span>

## <a name="save-your-storage-account-credentials-toohello-appconfig-file"></a><span data-ttu-id="5eb1f-137">A tárolási fiók hitelesítő adatait toohello app.config fájl mentése</span><span class="sxs-lookup"><span data-stu-id="5eb1f-137">Save your storage account credentials toohello app.config file</span></span>
<span data-ttu-id="5eb1f-138">Mentse el a hitelesítő adatokat a projekt app.config fájljába.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-138">Next, save your credentials in your project's app.config file.</span></span> <span data-ttu-id="5eb1f-139">Hello app.config fájl szerkesztése, úgy, hogy a következő példa, hogy hasonló toohello `myaccount` rendelkező a tárfiók nevére, és `mykey` rendelkező a tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-139">Edit hello app.config file so that it appears similar toohello following example, replacing `myaccount` with your storage account name, and `mykey` with your storage account key.</span></span>

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
> hello hello az Azure storage emulator legújabb verziója nem támogatja az Azure File storage. <span data-ttu-id="5eb1f-141">A kapcsolati karakterláncot kell használnia célként egy Azure Storage-fiókot hello felhő toowork az Azure File storage szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-141">Your connection string must target an Azure Storage Account in hello cloud toowork with Azure File storage.</span></span>

## <a name="add-using-directives"></a><span data-ttu-id="5eb1f-142">Hozzáadás irányelvekkel</span><span class="sxs-lookup"><span data-stu-id="5eb1f-142">Add using directives</span></span>
<span data-ttu-id="5eb1f-143">Nyissa meg hello `Program.cs` fájlt a Megoldáskezelőből, és adja hozzá a következő hello irányelvek toohello felső hello fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-143">Open hello `Program.cs` file from Solution Explorer, and add hello following using directives toohello top of hello file.</span></span>

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Azure Blobs
using Microsoft.WindowsAzure.Storage.File; // Namespace for Azure File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="access-hello-file-share-programmatically"></a><span data-ttu-id="5eb1f-144">Hozzáférés hello fájlmegosztás szoftveres</span><span class="sxs-lookup"><span data-stu-id="5eb1f-144">Access hello file share programmatically</span></span>
<span data-ttu-id="5eb1f-145">Ezután adja hozzá a következő kód toohello hello `Main()` (után hello a fent látható kód) metódus tooretrieve hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-145">Next, add hello following code toohello `Main()` method (after hello code shown above) tooretrieve hello connection string.</span></span> <span data-ttu-id="5eb1f-146">Ezzel a kóddal lekérdezi egy referencia-toohello korábban létrehozott fájlra, és kiírja a tartalom toohello console ablakban.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-146">This code gets a reference toohello file we created earlier and outputs its contents toohello console window.</span></span>

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

<span data-ttu-id="5eb1f-147">Hello console application toosee hello kimeneti futtatásához.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-147">Run hello console application toosee hello output.</span></span>

## <a name="set-hello-maximum-size-for-a-file-share"></a><span data-ttu-id="5eb1f-148">Fájlmegosztás hello maximális méretének beállítása</span><span class="sxs-lookup"><span data-stu-id="5eb1f-148">Set hello maximum size for a file share</span></span>
<span data-ttu-id="5eb1f-149">Verziójától hello Azure Storage ügyféloldali kódtár 5.x-es, állíthatja be hello kvótát (vagy maximális méretet) egy fájlmegosztáshoz GB-ban.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-149">Beginning with version 5.x of hello Azure Storage Client Library, you can set set hello quota (or maximum size) for a file share, in gigabytes.</span></span> <span data-ttu-id="5eb1f-150">Mennyi adatot hello megosztáson tárolt toosee is ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-150">You can also check toosee how much data is currently stored on hello share.</span></span>

<span data-ttu-id="5eb1f-151">Által beállítás hello kvótát egy megosztáshoz korlátozhatja a hello hello tárolják hello fájlok összesített mérete.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-151">By setting hello quota for a share, you can limit hello total size of hello files stored on hello share.</span></span> <span data-ttu-id="5eb1f-152">Ha hello hello megosztáson található fájlok összesített mérete meghaladja hello kvóta hello megosztáson, akkor az ügyfelek meglévő fájlok mérete nem lehet tooincrease hello vagy fogja új fájlok létrehozása, kivéve azokat, amelyek üresek.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-152">If hello total size of files on hello share exceeds hello quota set on hello share, then clients will be unable tooincrease hello size of existing files or create new files, unless those files are empty.</span></span>

<span data-ttu-id="5eb1f-153">hello az alábbi példa bemutatja, hogyan toocheck hello egy megosztás aktuális kihasználását, és hogyan tooset hello hello fájlmegosztás kvótája.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-153">hello example below shows how toocheck hello current usage for a share and how tooset hello quota for hello share.</span></span>

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

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a><span data-ttu-id="5eb1f-154">Közös hozzáférésű jogosultságkód létrehozása egy fájlhoz vagy fájlmegosztáshoz</span><span class="sxs-lookup"><span data-stu-id="5eb1f-154">Generate a shared access signature for a file or file share</span></span>
<span data-ttu-id="5eb1f-155">Verziójától 5.x-es hello Azure Storage ügyféloldali kódtár, létrehozhat egy közös hozzáférésű jogosultságkód (SAS) egy fájlmegosztáshoz vagy fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-155">Beginning with version 5.x of hello Azure Storage Client Library, you can generate a shared access signature (SAS) for a file share or for an individual file.</span></span> <span data-ttu-id="5eb1f-156">Is létrehozhat egy megosztott elérési házirendet egy fájlmegosztási toomanage megosztott hozzáférést aláírások.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-156">You can also create a shared access policy on a file share toomanage shared access signatures.</span></span> <span data-ttu-id="5eb1f-157">Megosztott hozzáférési házirend létrehozása az ajánlott, mivel hello SAS visszavonása, ha kell sérülhet módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-157">Creating a shared access policy is recommended, as it provides a means of revoking hello SAS if it should be compromised.</span></span>

<span data-ttu-id="5eb1f-158">a következő példa hello hoz létre egy megosztott elérési házirendet egy megosztáson, és akkor használja, hogy használ-e tooprovide hello Házirendmegkötések tartozó SAS korlátozására található hello fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-158">hello following example creates a shared access policy on a share, and then uses that policy tooprovide hello constraints for a SAS on a file in hello share.</span></span>

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

<span data-ttu-id="5eb1f-159">További információk a közös hozzáférésű jogosultságkód létrehozásáról és használatáról: [Közös hozzáférésű jogosultságkódok (SAS)](storage-dotnet-shared-access-signature-part-1.md) és [SAS létrehozása az Azure-blobok segítségével](storage-dotnet-shared-access-signature-part-2.md).</span><span class="sxs-lookup"><span data-stu-id="5eb1f-159">For more information about creating and using shared access signatures, see [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) and [Create and use a SAS with Azure Blobs](storage-dotnet-shared-access-signature-part-2.md).</span></span>

## <a name="copy-files"></a><span data-ttu-id="5eb1f-160">Fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="5eb1f-160">Copy files</span></span>
<span data-ttu-id="5eb1f-161">Verziójától 5.x-es hello Azure Storage ügyféloldali kódtár, egy fájl tooanother, egy fájl tooa blob vagy egy blob tooa fájl másolhatja.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-161">Beginning with version 5.x of hello Azure Storage Client Library, you can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="5eb1f-162">Hello következő szakaszokban bemutatjuk, hogyan tooperform ezek másolása műveletek programozott módon.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-162">In hello next sections, we demonstrate how tooperform these copy operations programmatically.</span></span>

<span data-ttu-id="5eb1f-163">AzCopy toocopy egy fájl tooanother vagy toocopy egy blob tooa fájlba vagy fordítva fordítva is használható.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-163">You can also use AzCopy toocopy one file tooanother or toocopy a blob tooa file or vice versa.</span></span> <span data-ttu-id="5eb1f-164">Lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="5eb1f-164">See [Transfer data with hello AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

> [!NOTE]
> <span data-ttu-id="5eb1f-165">Egy blob tooa fájlt, vagy a fájl tooa blob másolása, még akkor is, ha a másolt hello belül azonos kell használnia egy közös hozzáférésű jogosultságkód (SAS) tooauthenticate hello adatforrás-objektum, a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-165">If you are copying a blob tooa file, or a file tooa blob, you must use a shared access signature (SAS) tooauthenticate hello source object, even if you are copying within hello same storage account.</span></span>
> 
> 

<span data-ttu-id="5eb1f-166">**Másolja át a fájlt tooanother fájlt** hello alábbi példa átmásolja a terjesztendő egy fájl tooanother hello ugyanennek a megosztásnak.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-166">**Copy a file tooanother file** hello following example copies a file tooanother file in hello same share.</span></span> <span data-ttu-id="5eb1f-167">Mivel a másolási művelet másolja a fájlokat hello ugyanazt a tárfiókot, a megosztott kulcsos hitelesítést tooperform hello másolatot használja.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-167">Because this copy operation copies between files in hello same storage account, you can use Shared Key authentication tooperform hello copy.</span></span>

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

<span data-ttu-id="5eb1f-168">**Másolja a fájlt tooa blob** hello alábbi példa létrehoz egy fájlt, és átmásolja tooa blob hello belül ugyanazt a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-168">**Copy a file tooa blob** hello following example creates a file and copies it tooa blob within hello same storage account.</span></span> <span data-ttu-id="5eb1f-169">hello példa létrehoz egy SAS-hello forrás fájl, mely hello szolgáltatás használja tooauthenticate hozzáférés toohello forrásfájl hello másolási művelet során.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-169">hello example creates a SAS for hello source file, which hello service uses tooauthenticate access toohello source file during hello copy operation.</span></span>

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

<span data-ttu-id="5eb1f-170">A blob tooa fájl másolhatja a hello azonos módon.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-170">You can copy a blob tooa file in hello same way.</span></span> <span data-ttu-id="5eb1f-171">Ha hello Forrásobjektum egy blob, majd létre egy SAS tooauthenticate hozzáférés toothat blobot hello másolási művelet során.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-171">If hello source object is a blob, then create a SAS tooauthenticate access toothat blob during hello copy operation.</span></span>

## <a name="troubleshooting-azure-file-storage-using-metrics"></a><span data-ttu-id="5eb1f-172">Az Azure File Storage hibaelhárítása mérőszámok segítségével</span><span class="sxs-lookup"><span data-stu-id="5eb1f-172">Troubleshooting Azure File storage using metrics</span></span>
<span data-ttu-id="5eb1f-173">Az Azure Storage Analytics mostantól az Azure File Storage esetén is támogatja a mérőszámok használatát.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-173">Azure Storage Analytics now supports metrics for Azure File storage.</span></span> <span data-ttu-id="5eb1f-174">A metrikai adatok segítségével nyomon követheti a kéréseket, és diagnosztizálhatja a problémákat.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-174">With metrics data, you can trace requests and diagnose issues.</span></span>


<span data-ttu-id="5eb1f-175">Engedélyezheti a metrikák hello Azure File Storage [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5eb1f-175">You can enable metrics for Azure File storage from hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="5eb1f-176">Programozott módon által hívó hello beállítása File Service Properties művelet hello REST API-n keresztül, vagy a Storage ügyféloldali kódtár hello kódtárában egyik metrikák is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-176">You can also enable metrics programmatically by calling hello Set File Service Properties operation via hello REST API, or one of its analogues in hello Storage Client Library.</span></span>


<span data-ttu-id="5eb1f-177">a következő példakód hello bemutatja, hogyan toouse hello a Storage ügyféloldali kódtára a .NET tooenable Azure File storage mérőszámait.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-177">hello following code example shows how toouse hello Storage Client Library for .NET tooenable metrics for Azure File storage.</span></span>

<span data-ttu-id="5eb1f-178">Először adja hozzá a hello következő `using` irányelvek tooyour `Program.cs` , továbbá korábban ismertetett módon hozzáadott toothose fájlt:</span><span class="sxs-lookup"><span data-stu-id="5eb1f-178">First, add hello following `using` directives tooyour `Program.cs` file, in addition toothose you added above:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage.File.Protocol;
using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

<span data-ttu-id="5eb1f-179">Vegye figyelembe, hogy Azure BLOB, Azure-tábla és Azure várólisták megosztott hello használata `ServiceProperties` hello típusának `Microsoft.WindowsAzure.Storage.Shared.Protocol` névtér, Azure File storage a saját típust használja, hello `FileServiceProperties` hello típusának `Microsoft.WindowsAzure.Storage.File.Protocol` névtér.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-179">Note that while Azure Blobs, Azure Table, and Azure Queues use hello shared `ServiceProperties` type in hello `Microsoft.WindowsAzure.Storage.Shared.Protocol` namespace, Azure File storage uses its own type, hello `FileServiceProperties` type in hello `Microsoft.WindowsAzure.Storage.File.Protocol` namespace.</span></span> <span data-ttu-id="5eb1f-180">Mindkét névtérre hivatkozni kell a felhasználói kódból, azonban a következő kód toocompile hello.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-180">Both namespaces must be referenced from your code, however, for hello following code toocompile.</span></span>

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

<span data-ttu-id="5eb1f-181">Emellett olvassa el a túl[Azure File storage hibaelhárítási cikk](storage-troubleshoot-file-connection-problems.md) végpont hibaelhárítási útmutatót.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-181">Also, you can refer too[Azure File storage Troubleshooting Article](storage-troubleshoot-file-connection-problems.md) for end-to-end troubleshooting guidance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5eb1f-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5eb1f-182">Next steps</span></span>
<span data-ttu-id="5eb1f-183">Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.</span><span class="sxs-lookup"><span data-stu-id="5eb1f-183">See these links for more information about Azure File storage.</span></span>

### <a name="conceptual-articles-and-videos"></a><span data-ttu-id="5eb1f-184">Elméleti cikkek és videók</span><span class="sxs-lookup"><span data-stu-id="5eb1f-184">Conceptual articles and videos</span></span>
* [<span data-ttu-id="5eb1f-185">Azure File Storage: zökkenőmentes felhőalapú SMB fájlrendszer Windows és Linux rendszerekhez</span><span class="sxs-lookup"><span data-stu-id="5eb1f-185">Azure File storage: a frictionless cloud SMB file system for Windows and Linux</span></span>](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [<span data-ttu-id="5eb1f-186">Hogyan toouse Azure File storage Linux</span><span class="sxs-lookup"><span data-stu-id="5eb1f-186">How toouse Azure File storage with Linux</span></span>](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a><span data-ttu-id="5eb1f-187">File Storage-eszköztámogatás</span><span class="sxs-lookup"><span data-stu-id="5eb1f-187">Tooling support for File storage</span></span>
* [<span data-ttu-id="5eb1f-188">Using Azure PowerShell with Azure Storage (Az Azure PowerShell és az Azure Storage együttes használata)</span><span class="sxs-lookup"><span data-stu-id="5eb1f-188">Using Azure PowerShell with Azure Storage</span></span>](storage-powershell-guide-full.md)
* [<span data-ttu-id="5eb1f-189">Hogyan toouse Microsoft Azure Storage AzCopy</span><span class="sxs-lookup"><span data-stu-id="5eb1f-189">How toouse AzCopy with Microsoft Azure Storage</span></span>](storage-use-azcopy.md)
* [<span data-ttu-id="5eb1f-190">Az Azure Storage hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="5eb1f-190">Using hello Azure CLI with Azure Storage</span></span>](storage-azure-cli.md#create-and-manage-file-shares)
* [<span data-ttu-id="5eb1f-191">Az Azure File Storage-problémák hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="5eb1f-191">Troubleshooting Azure File storage problems</span></span>](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a><span data-ttu-id="5eb1f-192">Referencia</span><span class="sxs-lookup"><span data-stu-id="5eb1f-192">Reference</span></span>
* [<span data-ttu-id="5eb1f-193">Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia</span><span class="sxs-lookup"><span data-stu-id="5eb1f-193">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [<span data-ttu-id="5eb1f-194">Referencia a fájlszolgáltatás REST API-jához</span><span class="sxs-lookup"><span data-stu-id="5eb1f-194">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a><span data-ttu-id="5eb1f-195">Blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="5eb1f-195">Blog posts</span></span>
* [<span data-ttu-id="5eb1f-196">Azure File storage is now generally available (Mostantól általánosan elérhető az Azure File Storage)</span><span class="sxs-lookup"><span data-stu-id="5eb1f-196">Azure File storage is now generally available</span></span>](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [<span data-ttu-id="5eb1f-197">Az Azure File Storage ismertetése</span><span class="sxs-lookup"><span data-stu-id="5eb1f-197">Inside Azure File storage</span></span>](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [<span data-ttu-id="5eb1f-198">Introducing Microsoft Azure File Service (A Microsoft Azure File szolgáltatás bemutatása)</span><span class="sxs-lookup"><span data-stu-id="5eb1f-198">Introducing Microsoft Azure File Service</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [<span data-ttu-id="5eb1f-199">Persisting kapcsolatok tooMicrosoft Azure File storage</span><span class="sxs-lookup"><span data-stu-id="5eb1f-199">Persisting connections tooMicrosoft Azure File storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)
