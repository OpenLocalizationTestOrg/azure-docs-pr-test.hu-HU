---
title: "aaaCreate és a közös hozzáférésű jogosultságkód (SAS) használata az Azure Blob storage szolgáltatással |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan toocreate Blob-tároló megosztott hozzáférési aláírásokkal való használatra, és hogyan tooconsume őket az ügyfél alkalmazásokban."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 491e0b3c-76d4-4149-9a80-bbbd683b1f3e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/15/2017
ms.author: marsma
ms.openlocfilehash: 629f5c0aee3f41115a0d514a2010d8cc0187126d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a><span data-ttu-id="45a50-103">Közös hozzáférésű Jogosultságkód, 2. rész: Létrehozása és SAS-kód használata a Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="45a50-103">Shared Access Signatures, Part 2: Create and use a SAS with Blob storage</span></span>

<span data-ttu-id="45a50-104">[1. rész](storage-dotnet-shared-access-signature-part-1.md) felfedezte az oktatóanyag a közös hozzáférésű jogosultságkód (SAS), és azok használatának ajánlott eljárásai alapján.</span><span class="sxs-lookup"><span data-stu-id="45a50-104">[Part 1](storage-dotnet-shared-access-signature-part-1.md) of this tutorial explored shared access signatures (SAS) and explained best practices for using them.</span></span> <span data-ttu-id="45a50-105">2. rész bemutatja, hogyan toogenerate, majd használja megosztott hozzáférési aláírásokkal rendelkező Blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="45a50-105">Part 2 shows you how toogenerate and then use shared access signatures with Blob storage.</span></span> <span data-ttu-id="45a50-106">hello példák C# nyelven íródtak, és használja a hello Azure Storage ügyféloldali kódtára a .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="45a50-106">hello examples are written in C# and use hello Azure Storage Client Library for .NET.</span></span> <span data-ttu-id="45a50-107">az oktatóanyagban szereplő példák hello:</span><span class="sxs-lookup"><span data-stu-id="45a50-107">hello examples in this tutorial:</span></span>

* <span data-ttu-id="45a50-108">A közös hozzáférésű jogosultságkód egy tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="45a50-108">Generate a shared access signature on a container</span></span>
* <span data-ttu-id="45a50-109">A blob megosztott hozzáférési aláírást létrehozni</span><span class="sxs-lookup"><span data-stu-id="45a50-109">Generate a shared access signature on a blob</span></span>
* <span data-ttu-id="45a50-110">Létrehoz egy tárolt hozzáférési házirend toomanage aláírások egy tároló-erőforrások</span><span class="sxs-lookup"><span data-stu-id="45a50-110">Create a stored access policy toomanage signatures on a container's resources</span></span>
* <span data-ttu-id="45a50-111">Egy ügyfélalkalmazás hello megosztott hozzáférési aláírásokkal tesztelése</span><span class="sxs-lookup"><span data-stu-id="45a50-111">Test hello shared access signatures in a client application</span></span>

## <a name="about-this-tutorial"></a><span data-ttu-id="45a50-112">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="45a50-112">About this tutorial</span></span>
<span data-ttu-id="45a50-113">Ebben az oktatóanyagban létrehozhatunk két konzol alkalmazások, amelyek bemutatják, létrehozása és a tárolók és blobok közös hozzáférésű jogosultságkód használata:</span><span class="sxs-lookup"><span data-stu-id="45a50-113">In this tutorial, we create two console applications that demonstrate creating and using shared access signatures for containers and blobs:</span></span>

<span data-ttu-id="45a50-114">**1 alkalmazás**: hello felügyeleti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="45a50-114">**Application 1**: hello management application.</span></span> <span data-ttu-id="45a50-115">A tároló és a blob egy közös hozzáférésű jogosultságkódot állít elő.</span><span class="sxs-lookup"><span data-stu-id="45a50-115">Generates a shared access signature for a container and a blob.</span></span> <span data-ttu-id="45a50-116">Forráskód hello tárfiók_elérési_kulcsa tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="45a50-116">Includes hello storage account access key in source code.</span></span>

<span data-ttu-id="45a50-117">**Alkalmazás 2**: hello ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="45a50-117">**Application 2**: hello client application.</span></span> <span data-ttu-id="45a50-118">Hozzáférések tároló és a blob erőforrások hello megosztott hozzáférési aláírásokkal hello első alkalmazással létrehozott használatával.</span><span class="sxs-lookup"><span data-stu-id="45a50-118">Accesses container and blob resources using hello shared access signatures created with hello first application.</span></span> <span data-ttu-id="45a50-119">Felhasználási csak hello közös hozzáférésű aláírások tooaccess tároló és a blob-erőforrások – az hajtja végre *nem* hello tárfiók_elérési_kulcsa tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="45a50-119">Uses only hello shared access signatures tooaccess container and blob resources--it does *not* include hello storage account access key.</span></span>

## <a name="part-1-create-a-console-application-toogenerate-shared-access-signatures"></a><span data-ttu-id="45a50-120">1. rész: Létrehoz konzol alkalmazás toogenerate megosztott hozzáférési aláírásokkal</span><span class="sxs-lookup"><span data-stu-id="45a50-120">Part 1: Create a console application toogenerate shared access signatures</span></span>
<span data-ttu-id="45a50-121">Először győződjön meg róla, hogy rendelkezik-e hello Azure Storage ügyféloldali kódtára a .NET telepítése.</span><span class="sxs-lookup"><span data-stu-id="45a50-121">First, ensure that you have hello Azure Storage Client Library for .NET installed.</span></span> <span data-ttu-id="45a50-122">Hello telepítése [NuGet-csomag](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet-csomag") tartalmazó hello tartozó hello ügyféloldali kódtár legújabb szerelvényeket.</span><span class="sxs-lookup"><span data-stu-id="45a50-122">You can install hello [NuGet package](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet package") containing hello most up-to-date assemblies for hello client library.</span></span> <span data-ttu-id="45a50-123">Ez az ajánlott módszer, amellyel biztosítható, hogy rendelkezik-e hello legújabb javításokat hello.</span><span class="sxs-lookup"><span data-stu-id="45a50-123">This is hello recommended method for ensuring that you have hello most recent fixes.</span></span> <span data-ttu-id="45a50-124">Hello ügyféloldali kódtár legújabb verziójának hello hello részeként is letölthető [Azure SDK for .NET](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="45a50-124">You can also download hello client library as part of hello most recent version of hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/).</span></span>

<span data-ttu-id="45a50-125">A Visual Studio, hozzon létre egy új Windows-konzolalkalmazást, és adjon neki nevet **GenerateSharedAccessSignatures**.</span><span class="sxs-lookup"><span data-stu-id="45a50-125">In Visual Studio, create a new Windows console application and name it **GenerateSharedAccessSignatures**.</span></span> <span data-ttu-id="45a50-126">Hivatkozás hozzáadása túl[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) és [windowsazure.Storage kifejezésre](https://www.nuget.org/packages/WindowsAzure.Storage/) hello a következő módszerek egyikével:</span><span class="sxs-lookup"><span data-stu-id="45a50-126">Add references too[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) and [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) by using one of hello following approaches:</span></span>

* <span data-ttu-id="45a50-127">Használjon hello [NuGet-Csomagkezelő](https://docs.nuget.org/consume/installing-nuget) a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="45a50-127">Use hello [NuGet package manager](https://docs.nuget.org/consume/installing-nuget) in Visual Studio.</span></span> <span data-ttu-id="45a50-128">Válassza ki **projekt** > **NuGet-csomagok kezelése**, keresse rá az interneten csomagonként (Microsoft.WindowsAzure.ConfigurationManager és windowsazure.Storage kifejezésre), és a telepítést.</span><span class="sxs-lookup"><span data-stu-id="45a50-128">Select **Project** > **Manage NuGet Packages**, search online for each package (Microsoft.WindowsAzure.ConfigurationManager and WindowsAzure.Storage) and install them.</span></span>
* <span data-ttu-id="45a50-129">Keresse meg a szerelvények hello Azure SDK telepítése, és hivatkozások toothem hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="45a50-129">Alternatively, locate these assemblies in your installation of hello Azure SDK and add references toothem:</span></span>
  * <span data-ttu-id="45a50-130">Microsoft.WindowsAzure.Configuration.dll</span><span class="sxs-lookup"><span data-stu-id="45a50-130">Microsoft.WindowsAzure.Configuration.dll</span></span>
  * <span data-ttu-id="45a50-131">Microsoft.WindowsAzure.Storage.dll</span><span class="sxs-lookup"><span data-stu-id="45a50-131">Microsoft.WindowsAzure.Storage.dll</span></span>

<span data-ttu-id="45a50-132">A Program.cs fájl hello hello tetején adja hozzá a hello következő **használatával** irányelveket:</span><span class="sxs-lookup"><span data-stu-id="45a50-132">At hello top of hello Program.cs file, add hello following **using** directives:</span></span>

```csharp
using System.IO;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="45a50-133">Hello app.config fájl szerkesztése, hogy a konfigurációs beállításokat egy olyan kapcsolati karakterlánccal, amely tooyour tárfiók mutat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="45a50-133">Edit hello app.config file so that it contains a configuration setting with a connection string that points tooyour storage account.</span></span> <span data-ttu-id="45a50-134">Az app.config fájlt egy hasonló toothis kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="45a50-134">Your app.config file should look similar toothis one:</span></span>

```xml
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
  </startup>
  <appSettings>
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
  </appSettings>
</configuration>
```

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a><span data-ttu-id="45a50-135">A közös hozzáférésű jogosultságkód URI egy tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="45a50-135">Generate a shared access signature URI for a container</span></span>
<span data-ttu-id="45a50-136">a toobegin, jelenleg felvenni egy metódus toogenerate egy közös hozzáférésű jogosultságkódot új tárolóba.</span><span class="sxs-lookup"><span data-stu-id="45a50-136">toobegin with, we add a method toogenerate a shared access signature on a new container.</span></span> <span data-ttu-id="45a50-137">Ebben az esetben hello aláírás nincs társítva egy tárolt házirend, így hello URI hello információt, amely jelzi, az a lejárati dátum- és hello engedélyeket, az általa ad.</span><span class="sxs-lookup"><span data-stu-id="45a50-137">In this case, hello signature is not associated with a stored access policy, so it carries on hello URI hello information indicating its expiry time and hello permissions it grants.</span></span>

<span data-ttu-id="45a50-138">Először adja hozzá a kódot toohello **Main()** metódus tooauthenticate tooyour tárfiók eléréséhez, és hozzon létre egy új tároló:</span><span class="sxs-lookup"><span data-stu-id="45a50-138">First, add code toohello **Main()** method tooauthenticate access tooyour storage account and create a new container:</span></span>

```csharp
static void Main(string[] args)
{
    //Parse hello connection string and return a reference toohello storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create hello blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference tooa container toouse for hello sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Insert calls toohello methods created below here...

    //Require user input before closing hello console window.
    Console.ReadLine();
}
```

<span data-ttu-id="45a50-139">Ezután adja hozzá a közös hozzáférésű jogosultságkódot hello hello tároló hoz létre és hello aláírás URI metódus:</span><span class="sxs-lookup"><span data-stu-id="45a50-139">Next, add a method that generates hello shared access signature for hello container and returns hello signature URI:</span></span>

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
    //Set hello expiry time and permissions for hello container.
    //In this case no start time is specified, so hello shared access signature becomes valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Write;

    //Generate hello shared access signature on hello container, setting hello constraints directly on hello signature.
    string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

    //Return hello URI string for hello container, including hello SAS token.
    return container.Uri + sasContainerToken;
}
```

<span data-ttu-id="45a50-140">Adja hozzá az alábbi hello hello alján hello **Main()** módszer, mielőtt hello hívása túl**Console.ReadLine()**, toocall **GetContainerSasUri()** és hello írása aláírás URI toohello konzolablak:</span><span class="sxs-lookup"><span data-stu-id="45a50-140">Add hello following lines at hello bottom of hello **Main()** method, before hello call too**Console.ReadLine()**, toocall **GetContainerSasUri()** and write hello signature URI toohello console window:</span></span>

```csharp
//Generate a SAS URI for hello container, without a stored access policy.
Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
Console.WriteLine();
```

<span data-ttu-id="45a50-141">Fordítsa le, és futtassa a toooutput hello közös hozzáférésű jogosultságkódot URI hello új tároló.</span><span class="sxs-lookup"><span data-stu-id="45a50-141">Compile and run toooutput hello shared access signature URI for hello new container.</span></span> <span data-ttu-id="45a50-142">hello URI hasonló toohello következő lesz:</span><span class="sxs-lookup"><span data-stu-id="45a50-142">hello URI will be similar toohello following:</span></span>

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D
```

<span data-ttu-id="45a50-143">Hello kód futtatása után hello közös hozzáférésű jogosultságkódot hello tároló létrehozott érvényes lesz hello 24 órán át.</span><span class="sxs-lookup"><span data-stu-id="45a50-143">Once you have run hello code, hello shared access signature you created for hello container will be valid for hello next 24 hours.</span></span> <span data-ttu-id="45a50-144">hello aláírás biztosít egy ügyfél engedélyt toolist blobokat hello tárolóhoz és annak toowrite új blobok toohello tároló.</span><span class="sxs-lookup"><span data-stu-id="45a50-144">hello signature grants a client permission toolist blobs in hello container and toowrite new blobs toohello container.</span></span>

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a><span data-ttu-id="45a50-145">A közös hozzáférésű jogosultságkód URI egy BLOB készítése</span><span class="sxs-lookup"><span data-stu-id="45a50-145">Generate a shared access signature URI for a blob</span></span>
<span data-ttu-id="45a50-146">Ezután azt írási hasonló kód toocreate egy új blob hello tárolóban, és egy közös hozzáférésű jogosultságkódot létrehozni hozzá.</span><span class="sxs-lookup"><span data-stu-id="45a50-146">Next, we write similar code toocreate a new blob within hello container and generate a shared access signature for it.</span></span> <span data-ttu-id="45a50-147">A közös hozzáférésű jogosultságkódot nincs társítva egy tárolt hozzáférési házirend, hello URI hello kezdési ideje, a lejárat időpontjának és a jogosultsági információ tartalmazza azt.</span><span class="sxs-lookup"><span data-stu-id="45a50-147">This shared access signature is not associated with a stored access policy, so it includes hello start time, expiry time, and permission information in hello URI.</span></span>

<span data-ttu-id="45a50-148">Adja hozzá egy új hoz létre egy új blob és néhány szöveg tooit ír majd állít elő, a közös hozzáférésű jogosultságkód és metódus hello aláírás URI:</span><span class="sxs-lookup"><span data-stu-id="45a50-148">Add a new method that creates a new blob and writes some text tooit, then generates a shared access signature and returns hello signature URI:</span></span>

```csharp
static string GetBlobSasUri(CloudBlobContainer container)
{
    //Get a reference tooa blob within hello container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

    //Upload text toohello blob. If hello blob does not yet exist, it will be created.
    //If hello blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible tooclients via a shared access signature (SAS).";
    blob.UploadText(blobContent);

    //Set hello expiry time and permissions for hello blob.
    //In this case, hello start time is specified as a few minutes in hello past, toomitigate clock skew.
    //hello shared access signature will be valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

    //Generate hello shared access signature on hello blob, setting hello constraints directly on hello signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```

<span data-ttu-id="45a50-149">Hello hello alján **Main()** módszer, adja hozzá a következő sorokat toocall hello **GetBlobSasUri()**, mielőtt hello hívása túl**Console.ReadLine()**, és megosztott hello írása hozzáférési aláírás URI toohello konzolablak:</span><span class="sxs-lookup"><span data-stu-id="45a50-149">At hello bottom of hello **Main()** method, add hello following lines toocall **GetBlobSasUri()**, before hello call too**Console.ReadLine()**, and write hello shared access signature URI toohello console window:</span></span>

```csharp
//Generate a SAS URI for a blob within hello container, without a stored access policy.
Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
Console.WriteLine();
```

<span data-ttu-id="45a50-150">Fordítsa le, és futtassa a toooutput hello közös hozzáférésű jogosultságkódot URI hello új blob.</span><span class="sxs-lookup"><span data-stu-id="45a50-150">Compile and run toooutput hello shared access signature URI for hello new blob.</span></span> <span data-ttu-id="45a50-151">hello URI hasonló toohello következő lesz:</span><span class="sxs-lookup"><span data-stu-id="45a50-151">hello URI will be similar toohello following:</span></span>

```
https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D
```

### <a name="create-a-stored-access-policy-on-hello-container"></a><span data-ttu-id="45a50-152">Hello tárolóban tárolt hozzáférési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="45a50-152">Create a stored access policy on hello container</span></span>
<span data-ttu-id="45a50-153">Most hozzon létre egy tárolt házirend hello tárolóra, amelyek meghatározzák hello megkötéseit minden megosztott hozzáférési aláírásokkal, amely hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="45a50-153">Now let's create a stored access policy on hello container, which will define hello constraints for any shared access signatures that are associated with it.</span></span>

<span data-ttu-id="45a50-154">Az előző példákban hello azt megadott hello kezdési ideje, (implicit vagy explicit módon), hello lejárati idejének és hello hello engedélyeinek megosztott hozzáférési aláírás URI magát.</span><span class="sxs-lookup"><span data-stu-id="45a50-154">In hello previous examples, we specified hello start time (implicitly or explicitly), hello expiry time, and hello permissions on hello shared access signature URI itself.</span></span> <span data-ttu-id="45a50-155">A következő példák hello adtuk meg ezek tárolt hello hozzáférési házirend, nem a hello közös hozzáférésű jogosultságkóddal.</span><span class="sxs-lookup"><span data-stu-id="45a50-155">In hello following examples, we specify these on hello stored access policy, not on hello shared access signature.</span></span> <span data-ttu-id="45a50-156">Ez igen lehetővé teszi, hogy velünk ezek a megkötések nélkül hello hosszkorlátját megosztott toochange aláírás érhető el.</span><span class="sxs-lookup"><span data-stu-id="45a50-156">Doing so enables us toochange these constraints without reissuing hello shared access signature.</span></span>

<span data-ttu-id="45a50-157">Egy vagy több hello korlátozza a hello közös hozzáférésű jogosultságkódot, és a hozzáférési házirendben tárolt hello hello fennmaradó lehetséges toohave.</span><span class="sxs-lookup"><span data-stu-id="45a50-157">It's possible toohave one or more of hello constraints on hello shared access signature, and hello remainder on hello stored access policy.</span></span> <span data-ttu-id="45a50-158">Azonban csak megadhat hello kezdési ideje, a lejárat időpontjának és az engedélyek egy hely vagy más hello.</span><span class="sxs-lookup"><span data-stu-id="45a50-158">However, you can only specify hello start time, expiry time, and permissions in one place or hello other.</span></span> <span data-ttu-id="45a50-159">Például nem hello közös hozzáférésű jogosultságkódot engedélyeket ad meg, és is adja meg azokat hello tárolt házirend.</span><span class="sxs-lookup"><span data-stu-id="45a50-159">For example, you can't specify permissions on hello shared access signature and also specify them on hello stored access policy.</span></span>

<span data-ttu-id="45a50-160">Amikor egy tárolt hozzáférés-tooa tároló, hello tároló meglévő engedélyeket szerezni, hello új házirend hozzáadása, és ezután a hello tároló engedélyek beállítása.</span><span class="sxs-lookup"><span data-stu-id="45a50-160">When you add a stored access policy tooa container, you must get hello container's existing permissions, add hello new access policy, and then set hello container's permissions.</span></span>

<span data-ttu-id="45a50-161">Adja hozzá egy új módszer, amely létrehoz egy új tárolt házirend tárolóba, és hello házirend hello nevét adja vissza:</span><span class="sxs-lookup"><span data-stu-id="45a50-161">Add a new method that creates a new stored access policy on a container and returns hello name of hello policy:</span></span>

```csharp
static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
    string policyName)
{
    //Get hello container's existing permissions.
    BlobContainerPermissions permissions = container.GetPermissions();

    //Create a new shared access policy and define its constraints.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
    };

    //Add hello new policy toohello container's permissions, and set hello container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    container.SetPermissions(permissions);
}
```

<span data-ttu-id="45a50-162">Hello hello alján **Main()** módszer, mielőtt hello hívása túl**Console.ReadLine()**, adja hozzá a következő hello sorok toofirst törölje a meglévő hozzáférési házirendek és hívhatja hello  **CreateSharedAccessPolicy()** módszert:</span><span class="sxs-lookup"><span data-stu-id="45a50-162">At hello bottom of hello **Main()** method, before hello call too**Console.ReadLine()**, add hello following lines toofirst clear any existing access policies, and then call hello **CreateSharedAccessPolicy()** method:</span></span>

```csharp
//Clear any existing access policies on container.
BlobContainerPermissions perms = container.GetPermissions();
perms.SharedAccessPolicies.Clear();
container.SetPermissions(perms);

//Create a new access policy on hello container, which may be optionally used tooprovide constraints for
//shared access signatures on hello container and hello blob.
string sharedAccessPolicyName = "tutorialpolicy";
CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);
```

<span data-ttu-id="45a50-163">Ha törli a hello hozzáférési házirendek tárolóba, kell először hello tároló meglévő engedélyeket, majd törölje a jelet hello engedélyek lekérése, majd újra hello engedélyek beállítása.</span><span class="sxs-lookup"><span data-stu-id="45a50-163">When you clear hello access policies on a container, you must first get hello container's existing permissions, then clear hello permissions, then set hello permissions again.</span></span>

### <a name="generate-a-shared-access-signature-uri-on-hello-container-that-uses-an-access-policy"></a><span data-ttu-id="45a50-164">A közös hozzáférésű jogosultságkód URI hello tárolóra által használt a hozzáférési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="45a50-164">Generate a shared access signature URI on hello container that uses an access policy</span></span>
<span data-ttu-id="45a50-165">A következő létrehozhatunk egy másik közös hozzáférésű jogosultságkódot, amely a korábban létrehozott hello tároló, de ezúttal azt társítsa hello aláírás hello előző példában létrehozott hello tárolt házirend.</span><span class="sxs-lookup"><span data-stu-id="45a50-165">Next, we create another shared access signature for hello container that we created earlier, but this time we associate hello signature with hello stored access policy we created in hello previous example.</span></span>

<span data-ttu-id="45a50-166">Adja hozzá egy új módszer toogenerate egy másik közös hozzáférésű jogosultságkódot hello tároló:</span><span class="sxs-lookup"><span data-stu-id="45a50-166">Add a new method toogenerate another shared access signature for hello container:</span></span>

```csharp
static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Generate hello shared access signature on hello container. In this case, all of hello constraints for the
    //shared access signature are specified on hello stored access policy.
    string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

    //Return hello URI string for hello container, including hello SAS token.
    return container.Uri + sasContainerToken;
}
```

<span data-ttu-id="45a50-167">Hello hello alján **Main()** módszer, mielőtt hello hívása túl**Console.ReadLine()**, adja hozzá a következő sorokat toocall hello hello **GetContainerSasUriWithPolicy** módszer :</span><span class="sxs-lookup"><span data-stu-id="45a50-167">At hello bottom of hello **Main()** method, before hello call too**Console.ReadLine()**, add hello following lines toocall hello **GetContainerSasUriWithPolicy** method:</span></span>

```csharp
//Generate a SAS URI for hello container, using a stored access policy tooset constraints on hello SAS.
Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

### <a name="generate-a-shared-access-signature-uri-on-hello-blob-that-uses-an-access-policy"></a><span data-ttu-id="45a50-168">Megosztott hozzáférési aláírást URI Azonosítójának generálása hello Blob, hogy használja a hozzáférési házirend</span><span class="sxs-lookup"><span data-stu-id="45a50-168">Generate a Shared Access Signature URI on hello Blob That Uses an Access Policy</span></span>
<span data-ttu-id="45a50-169">Végül azt egy hasonló metódus toocreate hozzáadása egy másik blob, és tárolt hozzáférési házirenddel társított megosztott hozzáférési aláírást létrehozni.</span><span class="sxs-lookup"><span data-stu-id="45a50-169">Finally, we add a similar method toocreate another blob and generate a shared access signature that's associated with a stored access policy.</span></span>

<span data-ttu-id="45a50-170">Adja hozzá egy új módszer toocreate blob, és egy közös hozzáférésű jogosultságkódot létrehozni:</span><span class="sxs-lookup"><span data-stu-id="45a50-170">Add a new method toocreate a blob and generate a shared access signature:</span></span>

```csharp
static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Get a reference tooa blob within hello container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

    //Upload text toohello blob. If hello blob does not yet exist, it will be created.
    //If hello blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible tooclients via a shared access signature. " +
    "A stored access policy defines hello constraints for hello signature.";
    MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    ms.Position = 0;
    using (ms)
    {
        blob.UploadFromStream(ms);
    }

    //Generate hello shared access signature on hello blob.
    string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

    //Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```

<span data-ttu-id="45a50-171">Hello hello alján **Main()** módszer, mielőtt hello hívása túl**Console.ReadLine()**, adja hozzá a következő sorokat toocall hello hello **GetBlobSasUriWithPolicy** módszert:</span><span class="sxs-lookup"><span data-stu-id="45a50-171">At hello bottom of hello **Main()** method, before hello call too**Console.ReadLine()**, add hello following lines toocall hello **GetBlobSasUriWithPolicy** method:</span></span>

```csharp
//Generate a SAS URI for a blob within hello container, using a stored access policy tooset constraints on hello SAS.
Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

<span data-ttu-id="45a50-172">Hello **Main()** metódus most példához hasonló egészében.</span><span class="sxs-lookup"><span data-stu-id="45a50-172">hello **Main()** method should now look like this in its entirety.</span></span> <span data-ttu-id="45a50-173">Toowrite hello közös hozzáférésű jogosultságkódot URI-azonosítók toohello konzolablakot, majd futtassa másolhat és illessze be egy szövegfájlt hello a második rész a jelen oktatóanyag használható.</span><span class="sxs-lookup"><span data-stu-id="45a50-173">Run it toowrite hello shared access signature URIs toohello console window, then copy and paste them into a text file for use in hello second part of this tutorial.</span></span>

```csharp
static void Main(string[] args)
{
    //Parse hello connection string and return a reference toohello storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create hello blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference tooa container toouse for hello sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Generate a SAS URI for hello container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

    //Generate a SAS URI for a blob within hello container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on hello container, which may be optionally used tooprovide constraints for
    //shared access signatures on hello container and hello blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

    //Generate a SAS URI for hello container, using a stored access policy tooset constraints on hello SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    //Generate a SAS URI for a blob within hello container, using a stored access policy tooset constraints on hello SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    Console.ReadLine();
}
```

<span data-ttu-id="45a50-174">Hello GenerateSharedAccessSignatures Konzolalkalmazás futtatásakor kimeneti hasonló toohello következő láthatja.</span><span class="sxs-lookup"><span data-stu-id="45a50-174">When you run hello GenerateSharedAccessSignatures console application, you'll see output similar toohello following.</span></span> <span data-ttu-id="45a50-175">Ezek a hello megosztott hozzáférési aláírásokkal használhatja az hello oktatóanyag 2.</span><span class="sxs-lookup"><span data-stu-id="45a50-175">These are hello shared access signatures you use in Part 2 of hello tutorial.</span></span>

```
Container SAS URI: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=pFlEZD%2F6sJTNLxD%2FQ26Hh85j%2FzYPxZav6mP1KJwnvJE%3D&se=2017-05-16T16%3A16%3A47Z&sp=wl

Blob SAS URI: https://storagesample.blob.core.windows.net/sascontainer/sasblob.txt?sv=2016-05-31&sr=b&sig=%2FiBWAZbXESzCMvRcm7JwJBK0gT0BtPSWEq4pRwmlBRI%3D&st=2017-05-15T16%3A11%3A48Z&se=2017-05-16T16%3A16%3A48Z&sp=rw

Container SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&si=tutorialpolicy&sig=aMb6rKDvvpfiGVsZI2rCmyUra6ZPpq%2BZ%2FLyTgAeec%2Bk%3D

Blob SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer/sasblobpolicy.txt?sv=2016-05-31&sr=b&si=tutorialpolicy&sig=%2FkTWkT23SS45%2FoF4bK2mqXkN%2BPKs%2FyHuzkfQ4GFoZVU%3D
```

## <a name="part-2-create-a-console-application-tootest-hello-shared-access-signatures"></a><span data-ttu-id="45a50-176">2. lépés: Létrehoz konzol alkalmazás tootest hello megosztott hozzáférési aláírásokkal</span><span class="sxs-lookup"><span data-stu-id="45a50-176">Part 2: Create a console application tootest hello shared access signatures</span></span>
<span data-ttu-id="45a50-177">tootest hello közös hozzáférésű jogosultságkód hello előző példákban létrehozott, a Microsoft hello tároló és a blob hello aláírások tooperform műveletek használó második Konzolalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="45a50-177">tootest hello shared access signatures created in hello previous examples, we create a second console application that uses hello signatures tooperform operations on hello container and on a blob.</span></span>

> [!NOTE]
> <span data-ttu-id="45a50-178">Ha több mint 24 órája eltelt óta hello oktatóanyag első részét hello elvégezte, hello aláírások akkor jönnek létre már nem érvényesek.</span><span class="sxs-lookup"><span data-stu-id="45a50-178">If more than 24 hours have passed since you completed hello first part of hello tutorial, hello signatures you generated will no longer be valid.</span></span> <span data-ttu-id="45a50-179">Ebben az esetben kell futtatni hello kód hello első console application toogenerate friss közös hozzáférésű jogosultságkód hello hello oktatóanyag második része legyen.</span><span class="sxs-lookup"><span data-stu-id="45a50-179">In this case, you should run hello code in hello first console application toogenerate fresh shared access signatures for use in hello second part of hello tutorial.</span></span>
>

<span data-ttu-id="45a50-180">A Visual Studio, hozzon létre egy új Windows-konzolalkalmazást, és adjon neki nevet **ConsumeSharedAccessSignatures**.</span><span class="sxs-lookup"><span data-stu-id="45a50-180">In Visual Studio, create a new Windows console application and name it **ConsumeSharedAccessSignatures**.</span></span> <span data-ttu-id="45a50-181">Hivatkozás hozzáadása túl[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) és [windowsazure.Storage kifejezésre](https://www.nuget.org/packages/WindowsAzure.Storage/), mint korábban.</span><span class="sxs-lookup"><span data-stu-id="45a50-181">Add references too[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) and [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), as you did previously.</span></span>

<span data-ttu-id="45a50-182">A Program.cs fájl hello hello tetején adja hozzá a hello következő **használatával** irányelveket:</span><span class="sxs-lookup"><span data-stu-id="45a50-182">At hello top of hello Program.cs file, add hello following **using** directives:</span></span>

```csharp
using System.IO;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="45a50-183">A hello hello törzsét **Main()** módszer, adja hozzá a következő karakterlánc állandók, az értékek toohello megosztott hozzáférési aláírásokkal hello oktatóanyag 1 részében létrehozott módosítása hello.</span><span class="sxs-lookup"><span data-stu-id="45a50-183">In hello body of hello **Main()** method, add hello following string constants, changing their values toohello shared access signatures you generated in part 1 of hello tutorial.</span></span>

```csharp
static void Main(string[] args)
{
    const string containerSAS = "<your container SAS>";
    const string blobSAS = "<your blob SAS>";
    const string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    const string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
}
```

### <a name="add-a-method-tootry-container-operations-using-a-shared-access-signature"></a><span data-ttu-id="45a50-184">A metódus tootry tároló műveletek használatával a közös hozzáférésű jogosultságkód hozzáadása</span><span class="sxs-lookup"><span data-stu-id="45a50-184">Add a method tootry container operations using a shared access signature</span></span>
<span data-ttu-id="45a50-185">Ezután azt adja hozzá a módszere, amely az egyes tároló műveletek használatával a közös hozzáférésű jogosultságkód hello tároló teszteli.</span><span class="sxs-lookup"><span data-stu-id="45a50-185">Next, we add a method that tests some container operations using a shared access signature for hello container.</span></span> <span data-ttu-id="45a50-186">hello közös hozzáférésű jogosultságkódot hivatkozás toohello tárolója, hello aláírás önmagában alapuló hozzáférés toohello tároló hitelesítéséhez használt tooreturn.</span><span class="sxs-lookup"><span data-stu-id="45a50-186">hello shared access signature is used tooreturn a reference toohello container, authenticating access toohello container based on hello signature alone.</span></span>

<span data-ttu-id="45a50-187">Adja hozzá a következő metódus tooProgram.cs hello:</span><span class="sxs-lookup"><span data-stu-id="45a50-187">Add hello following method tooProgram.cs:</span></span>

```csharp
static void UseContainerSAS(string sas)
{
    //Try performing container operations with hello SAS provided.

    //Return a reference toohello container using hello SAS URI.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

    //Create a list toostore blob URIs returned by a listing operation on hello container.
    List<ICloudBlob> blobList = new List<ICloudBlob>();

    //Write operation: write a new blob toohello container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
        string blobContent = "This blob was created with a shared access signature granting write permissions toohello container. ";
        blob.UploadText(blobContent);

        Console.WriteLine("Write operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Write operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //List operation: List hello blobs in hello container.
    try
    {
        foreach (ICloudBlob blob in container.ListBlobs())
        {
            blobList.Add(blob);
        }
        Console.WriteLine("List operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("List operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Read operation: Get a reference tooone of hello blobs in hello container and read it.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
        MemoryStream msRead = new MemoryStream();
        msRead.Position = 0;
        using (msRead)
        {
            blob.DownloadToStream(msRead);
            Console.WriteLine(msRead.Length);
        }
        Console.WriteLine("Read operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Read operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    Console.WriteLine();

    //Delete operation: Delete a blob in hello container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
        blob.Delete();
        Console.WriteLine("Delete operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Delete operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="45a50-188">Frissítés hello **Main()** metódus toocall **UseContainerSAS()** mindkét hello megosztott hozzáférési aláírásokkal hello tárolójához létrehozott:</span><span class="sxs-lookup"><span data-stu-id="45a50-188">Update hello **Main()** method toocall **UseContainerSAS()** with both of hello shared access signatures you created on hello container:</span></span>

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call hello test methods with hello shared access signatures created on hello container, with and without hello access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    Console.ReadLine();
}
```

### <a name="add-a-method-tootry-blob-operations-using-a-shared-access-signature"></a><span data-ttu-id="45a50-189">A metódus tootry blob műveletek használatával a közös hozzáférésű jogosultságkód hozzáadása</span><span class="sxs-lookup"><span data-stu-id="45a50-189">Add a method tootry blob operations using a shared access signature</span></span>
<span data-ttu-id="45a50-190">Végül azt adja hozzá egy módszert, amelyet hello blob néhány blob-műveletekbe egy közös hozzáférésű jogosultságkódot teszteli.</span><span class="sxs-lookup"><span data-stu-id="45a50-190">Finally, we add a method that tests some blob operations using a shared access signature on hello blob.</span></span> <span data-ttu-id="45a50-191">Ebben az esetben használjuk hello konstruktor **CloudBlockBlob(String)**, a hello közös hozzáférésű jogosultságkódot, tooreturn hivatkozás toohello blob tompított.</span><span class="sxs-lookup"><span data-stu-id="45a50-191">In this case, we use hello constructor **CloudBlockBlob(String)**, passing in hello shared access signature, tooreturn a reference toohello blob.</span></span> <span data-ttu-id="45a50-192">Nincs más hitelesítés szükség; hello aláírás önmagában alapul.</span><span class="sxs-lookup"><span data-stu-id="45a50-192">No other authentication is required; it's based on hello signature alone.</span></span>

<span data-ttu-id="45a50-193">Adja hozzá a következő metódus tooProgram.cs hello:</span><span class="sxs-lookup"><span data-stu-id="45a50-193">Add hello following method tooProgram.cs:</span></span>

```csharp
static void UseBlobSAS(string sas)
{
    //Try performing blob operations using hello SAS provided.

    //Return a reference toohello blob using hello SAS URI.
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

    //Write operation: Write a new blob toohello container.
    try
    {
        string blobContent = "This blob was created with a shared access signature granting write permissions toohello blob. ";
        MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        msWrite.Position = 0;
        using (msWrite)
        {
            blob.UploadFromStream(msWrite);
        }
        Console.WriteLine("Write operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Write operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Read operation: Read hello contents of hello blob.
    try
    {
        MemoryStream msRead = new MemoryStream();
        using (msRead)
        {
            blob.DownloadToStream(msRead);
            msRead.Position = 0;
            using (StreamReader reader = new StreamReader(msRead, true))
            {
                string line;
                while ((line = reader.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
        Console.WriteLine("Read operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Read operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Delete operation: Delete hello blob.
    try
    {
        blob.Delete();
        Console.WriteLine("Delete operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Delete operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="45a50-194">Frissítés hello **Main()** metódus toocall **UseBlobSAS()** mindkét hello megosztott hozzáférési aláírásokkal, amelyet a hello blob hozott létre:</span><span class="sxs-lookup"><span data-stu-id="45a50-194">Update hello **Main()** method toocall **UseBlobSAS()** with both of hello shared access signatures that you created on hello blob:</span></span>

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call hello test methods with hello shared access signatures created on hello container, with and without hello access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    //Call hello test methods with hello shared access signatures created on hello blob, with and without hello access policy.
    UseBlobSAS(blobSAS);
    UseBlobSAS(blobSASWithAccessPolicy);

    Console.ReadLine();
}
```

<span data-ttu-id="45a50-195">Futtassa a konzolalkalmazást hello, és tekintse meg az hello kimeneti toosee mely aláírások milyen műveletek engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="45a50-195">Run hello console application and observe hello output toosee which operations are permitted for which signatures.</span></span> <span data-ttu-id="45a50-196">hello kimeneti hello konzolablakban hasonló toohello következő fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="45a50-196">hello output in hello console window will look similar toohello following:</span></span>

```
Write operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

List operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

Read operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: hello remote server returned an error: (403) Forbidden.

Delete operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: hello remote server returned an error: (403) Forbidden.

...
```

## <a name="next-steps"></a><span data-ttu-id="45a50-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="45a50-197">Next Steps</span></span>

* [<span data-ttu-id="45a50-198">Közös hozzáférésű Jogosultságkód, 1. rész: Hello SAS-modell ismertetése</span><span class="sxs-lookup"><span data-stu-id="45a50-198">Shared Access Signatures, Part 1: Understanding hello SAS Model</span></span>](storage-dotnet-shared-access-signature-part-1.md)
* [<span data-ttu-id="45a50-199">Kezelheti a névtelen olvasási hozzáférés toocontainers és blobok</span><span class="sxs-lookup"><span data-stu-id="45a50-199">Manage anonymous read access toocontainers and blobs</span></span>](storage-manage-access-to-resources.md)
* [<span data-ttu-id="45a50-200">A közös hozzáférésű jogosultságkód (REST API-t) hozzáférés delegálása</span><span class="sxs-lookup"><span data-stu-id="45a50-200">Delegating access with a shared access signature (REST API)</span></span>](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [<span data-ttu-id="45a50-201">Tábla- és várólista SAS bemutatása</span><span class="sxs-lookup"><span data-stu-id="45a50-201">Introducing Table and Queue SAS</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
