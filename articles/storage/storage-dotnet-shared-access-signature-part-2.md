---
title: "Létrehozhat és használhat egy közös hozzáférésű jogosultságkód (SAS) az Azure Blob storage szolgáltatással |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan használható közös hozzáférésű jogosultságkód létrehozása a Blob-tároló, és hogyan kell felhasználni azokat az ügyfél alkalmazásokban."
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
ms.openlocfilehash: ba78dd2bbcc68ffffeba59b1623891126baf656f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a><span data-ttu-id="c2fb9-103">Közös hozzáférésű Jogosultságkód, 2. rész: Létrehozása és SAS-kód használata a Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="c2fb9-103">Shared Access Signatures, Part 2: Create and use a SAS with Blob storage</span></span>

<span data-ttu-id="c2fb9-104">[1. rész](storage-dotnet-shared-access-signature-part-1.md) felfedezte az oktatóanyag a közös hozzáférésű jogosultságkód (SAS), és azok használatának ajánlott eljárásai alapján.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-104">[Part 1](storage-dotnet-shared-access-signature-part-1.md) of this tutorial explored shared access signatures (SAS) and explained best practices for using them.</span></span> <span data-ttu-id="c2fb9-105">2. rész bemutatja, hogyan hozhat létre, és a Blob storage közös hozzáférésű jogosultságkód majd használni.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-105">Part 2 shows you how to generate and then use shared access signatures with Blob storage.</span></span> <span data-ttu-id="c2fb9-106">A példák C# nyelven íródtak, és használja az Azure Storage ügyféloldali kódtára a .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-106">The examples are written in C# and use the Azure Storage Client Library for .NET.</span></span> <span data-ttu-id="c2fb9-107">Az oktatóanyagban szereplő példák:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-107">The examples in this tutorial:</span></span>

* <span data-ttu-id="c2fb9-108">A közös hozzáférésű jogosultságkód egy tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2fb9-108">Generate a shared access signature on a container</span></span>
* <span data-ttu-id="c2fb9-109">A blob megosztott hozzáférési aláírást létrehozni</span><span class="sxs-lookup"><span data-stu-id="c2fb9-109">Generate a shared access signature on a blob</span></span>
* <span data-ttu-id="c2fb9-110">Egy tároló-erőforrások aláírások kezeléséhez tárolt hozzáférési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2fb9-110">Create a stored access policy to manage signatures on a container's resources</span></span>
* <span data-ttu-id="c2fb9-111">A közös hozzáférésű jogosultságkód egy ügyfél-alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="c2fb9-111">Test the shared access signatures in a client application</span></span>

## <a name="about-this-tutorial"></a><span data-ttu-id="c2fb9-112">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="c2fb9-112">About this tutorial</span></span>
<span data-ttu-id="c2fb9-113">Ebben az oktatóanyagban létrehozhatunk két konzol alkalmazások, amelyek bemutatják, létrehozása és a tárolók és blobok közös hozzáférésű jogosultságkód használata:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-113">In this tutorial, we create two console applications that demonstrate creating and using shared access signatures for containers and blobs:</span></span>

<span data-ttu-id="c2fb9-114">**1 alkalmazás**: A felügyeleti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-114">**Application 1**: The management application.</span></span> <span data-ttu-id="c2fb9-115">A tároló és a blob egy közös hozzáférésű jogosultságkódot állít elő.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-115">Generates a shared access signature for a container and a blob.</span></span> <span data-ttu-id="c2fb9-116">A tárfiók elérési kulcsának forráskód tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-116">Includes the storage account access key in source code.</span></span>

<span data-ttu-id="c2fb9-117">**Alkalmazás 2**: az ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-117">**Application 2**: The client application.</span></span> <span data-ttu-id="c2fb9-118">Hozzáférések tároló és a blob erőforrások a közös hozzáférésű jogosultságkód létre az első alkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-118">Accesses container and blob resources using the shared access signatures created with the first application.</span></span> <span data-ttu-id="c2fb9-119">Csak a közös hozzáférésű jogosultságkód hozzáférést tároló és a blob-erőforrások – használja az hajtja végre *nem* tartalmazza a tárfiók elérési kulcsának.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-119">Uses only the shared access signatures to access container and blob resources--it does *not* include the storage account access key.</span></span>

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a><span data-ttu-id="c2fb9-120">1. lépés: Hozzon létre egy konzolalkalmazást közös hozzáférésű jogosultságkód létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2fb9-120">Part 1: Create a console application to generate shared access signatures</span></span>
<span data-ttu-id="c2fb9-121">Először győződjön meg róla, hogy rendelkezik-e az Azure Storage ügyféloldali kódtára a .NET telepítése.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-121">First, ensure that you have the Azure Storage Client Library for .NET installed.</span></span> <span data-ttu-id="c2fb9-122">Telepítheti a [NuGet-csomag](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet-csomag") tartalmazó az ügyféloldali kódtár legújabb szerelvényeket.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-122">You can install the [NuGet package](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet package") containing the most up-to-date assemblies for the client library.</span></span> <span data-ttu-id="c2fb9-123">Ez az az ajánlott módszer annak biztosítására, hogy rendelkezik-e a legújabb javításokat.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-123">This is the recommended method for ensuring that you have the most recent fixes.</span></span> <span data-ttu-id="c2fb9-124">Az ügyféloldali kódtár legújabb verziójának részeként is letöltheti a [Azure SDK for .NET](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c2fb9-124">You can also download the client library as part of the most recent version of the [Azure SDK for .NET](https://azure.microsoft.com/downloads/).</span></span>

<span data-ttu-id="c2fb9-125">A Visual Studio, hozzon létre egy új Windows-konzolalkalmazást, és adjon neki nevet **GenerateSharedAccessSignatures**.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-125">In Visual Studio, create a new Windows console application and name it **GenerateSharedAccessSignatures**.</span></span> <span data-ttu-id="c2fb9-126">Hivatkozásokat adni [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) és [windowsazure.Storage kifejezésre](https://www.nuget.org/packages/WindowsAzure.Storage/) a következő módszerek egyikével:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-126">Add references to [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) and [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) by using one of the following approaches:</span></span>

* <span data-ttu-id="c2fb9-127">Használja a [NuGet-Csomagkezelő](https://docs.nuget.org/consume/installing-nuget) a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-127">Use the [NuGet package manager](https://docs.nuget.org/consume/installing-nuget) in Visual Studio.</span></span> <span data-ttu-id="c2fb9-128">Válassza ki **projekt** > **NuGet-csomagok kezelése**, keresse rá az interneten csomagonként (Microsoft.WindowsAzure.ConfigurationManager és windowsazure.Storage kifejezésre), és a telepítést.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-128">Select **Project** > **Manage NuGet Packages**, search online for each package (Microsoft.WindowsAzure.ConfigurationManager and WindowsAzure.Storage) and install them.</span></span>
* <span data-ttu-id="c2fb9-129">Keresse meg a szerelvényeket az Azure SDK telepítése, és adja hozzá őket mutató hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-129">Alternatively, locate these assemblies in your installation of the Azure SDK and add references to them:</span></span>
  * <span data-ttu-id="c2fb9-130">Microsoft.WindowsAzure.Configuration.dll</span><span class="sxs-lookup"><span data-stu-id="c2fb9-130">Microsoft.WindowsAzure.Configuration.dll</span></span>
  * <span data-ttu-id="c2fb9-131">Microsoft.WindowsAzure.Storage.dll</span><span class="sxs-lookup"><span data-stu-id="c2fb9-131">Microsoft.WindowsAzure.Storage.dll</span></span>

<span data-ttu-id="c2fb9-132">A Program.cs fájl felső részén adja hozzá a következő **használatával** irányelveket:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-132">At the top of the Program.cs file, add the following **using** directives:</span></span>

```csharp
using System.IO;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="c2fb9-133">Szerkessze az app.config fájlt, hogy a konfigurációs beállítások mutat, a tárfiók kapcsolati karakterláncot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-133">Edit the app.config file so that it contains a configuration setting with a connection string that points to your storage account.</span></span> <span data-ttu-id="c2fb9-134">Az app.config fájlt ehhez hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-134">Your app.config file should look similar to this one:</span></span>

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

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a><span data-ttu-id="c2fb9-135">A közös hozzáférésű jogosultságkód URI egy tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2fb9-135">Generate a shared access signature URI for a container</span></span>
<span data-ttu-id="c2fb9-136">Először igazolnia adja hozzá a közös hozzáférésű jogosultságkód egy új tároló létrehozásához egy metódust.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-136">To begin with, we add a method to generate a shared access signature on a new container.</span></span> <span data-ttu-id="c2fb9-137">Ebben az esetben az aláírás nincs társítva egy tárolt hozzáférési házirend, az általa URI-n az adatokat, feltüntetve a lejárat időpontjának és az engedélyeket ad.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-137">In this case, the signature is not associated with a stored access policy, so it carries on the URI the information indicating its expiry time and the permissions it grants.</span></span>

<span data-ttu-id="c2fb9-138">Először adja hozzá a kódot a **Main()** módszer, amellyel hitelesíti a hozzáférést a tárfiókhoz, és hozzon létre egy új tároló:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-138">First, add code to the **Main()** method to authenticate access to your storage account and create a new container:</span></span>

```csharp
static void Main(string[] args)
{
    //Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create the blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container to use for the sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Insert calls to the methods created below here...

    //Require user input before closing the console window.
    Console.ReadLine();
}
```

<span data-ttu-id="c2fb9-139">Ezután adja hozzá a állít elő, a közös hozzáférésű jogosultságkódot, a tároló és az aláírás URI metódus:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-139">Next, add a method that generates the shared access signature for the container and returns the signature URI:</span></span>

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
    //Set the expiry time and permissions for the container.
    //In this case no start time is specified, so the shared access signature becomes valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Write;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

<span data-ttu-id="c2fb9-140">Adja hozzá a következő sorokat alján a **Main()** metódus hívása előtt **Console.ReadLine()**meghívására **GetContainerSasUri()** és az aláírás URI írni a konzolablakban:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-140">Add the following lines at the bottom of the **Main()** method, before the call to **Console.ReadLine()**, to call **GetContainerSasUri()** and write the signature URI to the console window:</span></span>

```csharp
//Generate a SAS URI for the container, without a stored access policy.
Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
Console.WriteLine();
```

<span data-ttu-id="c2fb9-141">Fordítsa le és futtassa a közös hozzáférésű jogosultságkódot URI az új tároló kimeneti.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-141">Compile and run to output the shared access signature URI for the new container.</span></span> <span data-ttu-id="c2fb9-142">Az URI azonosító a következőhöz hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-142">The URI will be similar to the following:</span></span>

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D
```

<span data-ttu-id="c2fb9-143">A kód futtatása után a közös hozzáférésű jogosultságkódot, a tároló létrehozott lesz érvényes a következő 24 órában.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-143">Once you have run the code, the shared access signature you created for the container will be valid for the next 24 hours.</span></span> <span data-ttu-id="c2fb9-144">Az aláírás engedélyt ad egy ügyfél lista BLOB a tárolóban, és a tároló új blobok írni.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-144">The signature grants a client permission to list blobs in the container and to write new blobs to the container.</span></span>

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a><span data-ttu-id="c2fb9-145">A közös hozzáférésű jogosultságkód URI egy BLOB készítése</span><span class="sxs-lookup"><span data-stu-id="c2fb9-145">Generate a shared access signature URI for a blob</span></span>
<span data-ttu-id="c2fb9-146">A következő azt írhatja hasonló kódot hozzon létre egy új blob a tárolóban, és egy közös hozzáférésű jogosultságkódot létrehozni hozzá.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-146">Next, we write similar code to create a new blob within the container and generate a shared access signature for it.</span></span> <span data-ttu-id="c2fb9-147">A közös hozzáférésű jogosultságkódot nincs társítva tárolt hozzáférési házirenddel, így a kezdési ideje, a lejárati idő és a engedély információkat tartalmaz az URI azonosító.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-147">This shared access signature is not associated with a stored access policy, so it includes the start time, expiry time, and permission information in the URI.</span></span>

<span data-ttu-id="c2fb9-148">Adja hozzá egy új hoz létre egy új blob és szöveget abba, majd állít elő, a közös hozzáférésű jogosultságkód és metódus aláírása URI:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-148">Add a new method that creates a new blob and writes some text to it, then generates a shared access signature and returns the signature URI:</span></span>

```csharp
static string GetBlobSasUri(CloudBlobContainer container)
{
    //Get a reference to a blob within the container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

    //Upload text to the blob. If the blob does not yet exist, it will be created.
    //If the blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible to clients via a shared access signature (SAS).";
    blob.UploadText(blobContent);

    //Set the expiry time and permissions for the blob.
    //In this case, the start time is specified as a few minutes in the past, to mitigate clock skew.
    //The shared access signature will be valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

    //Generate the shared access signature on the blob, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

<span data-ttu-id="c2fb9-149">Alján a **Main()** módszer, adja hozzá a következő sorokat hívására **GetBlobSasUri()**, hívása előtt **Console.ReadLine()**, és a közös hozzáférésű jogosultságkódot URI írni a konzolablakban:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-149">At the bottom of the **Main()** method, add the following lines to call **GetBlobSasUri()**, before the call to **Console.ReadLine()**, and write the shared access signature URI to the console window:</span></span>

```csharp
//Generate a SAS URI for a blob within the container, without a stored access policy.
Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
Console.WriteLine();
```

<span data-ttu-id="c2fb9-150">Fordítsa le és futtassa az új BLOB URI közös hozzáférésű jogosultságkódot kimeneti.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-150">Compile and run to output the shared access signature URI for the new blob.</span></span> <span data-ttu-id="c2fb9-151">Az URI azonosító a következőhöz hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-151">The URI will be similar to the following:</span></span>

```
https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D
```

### <a name="create-a-stored-access-policy-on-the-container"></a><span data-ttu-id="c2fb9-152">A tárolóban tárolt hozzáférési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2fb9-152">Create a stored access policy on the container</span></span>
<span data-ttu-id="c2fb9-153">Most hozzon létre egy tárolt házirend a tárolóra, és határozza meg, amely hozzá van rendelve megosztott hozzáférési aláírásokkal korlátait.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-153">Now let's create a stored access policy on the container, which will define the constraints for any shared access signatures that are associated with it.</span></span>

<span data-ttu-id="c2fb9-154">Az előző példákban azt meg a kezdési idő (implicit vagy explicit módon), a lejárati időpont és az engedélyek a közös hozzáférésű jogosultságkódot maga URI.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-154">In the previous examples, we specified the start time (implicitly or explicitly), the expiry time, and the permissions on the shared access signature URI itself.</span></span> <span data-ttu-id="c2fb9-155">Az alábbi példák azt adja meg ezeket a tárolt házirend, nem a közös hozzáférésű jogosultságkóddal.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-155">In the following examples, we specify these on the stored access policy, not on the shared access signature.</span></span> <span data-ttu-id="c2fb9-156">Így lehetővé teszi, hogy ezek a megkötések nélkül a közös hozzáférésű jogosultságkódot hosszkorlátját módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-156">Doing so enables us to change these constraints without reissuing the shared access signature.</span></span>

<span data-ttu-id="c2fb9-157">Lehetséges, hogy egy vagy több, a közös hozzáférésű jogosultságkódot korlátozásaival, valamint a tárolt házirend a többi.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-157">It's possible to have one or more of the constraints on the shared access signature, and the remainder on the stored access policy.</span></span> <span data-ttu-id="c2fb9-158">Azonban csak megadhatja a kezdő időpont, a lejárat időpontjának és az engedélyek egy helyen vagy a másik.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-158">However, you can only specify the start time, expiry time, and permissions in one place or the other.</span></span> <span data-ttu-id="c2fb9-159">Például nem engedélyeket ad meg a közös hozzáférésű jogosultságkódot és is adja meg azokat a tárolt házirend.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-159">For example, you can't specify permissions on the shared access signature and also specify them on the stored access policy.</span></span>

<span data-ttu-id="c2fb9-160">Amikor egy tárolt házirend hozzáadása a tárolóhoz, a tároló meglévő engedélyeket szerezni, adja hozzá az új házirend, és adja meg a tároló engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-160">When you add a stored access policy to a container, you must get the container's existing permissions, add the new access policy, and then set the container's permissions.</span></span>

<span data-ttu-id="c2fb9-161">Adja hozzá egy új módszer, amely létrehoz egy új tárolt házirend tárolóba, és a házirend nevét adja vissza:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-161">Add a new method that creates a new stored access policy on a container and returns the name of the policy:</span></span>

```csharp
static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
    string policyName)
{
    //Get the container's existing permissions.
    BlobContainerPermissions permissions = container.GetPermissions();

    //Create a new shared access policy and define its constraints.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
    };

    //Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    container.SetPermissions(permissions);
}
```

<span data-ttu-id="c2fb9-162">Alján a **Main()** metódus hívása előtt **Console.ReadLine()**adja hozzá a következő sorokat első törölje a meglévő hozzáférési házirendeket, és, majd hívja a **CreateSharedAccessPolicy()** módszert:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-162">At the bottom of the **Main()** method, before the call to **Console.ReadLine()**, add the following lines to first clear any existing access policies, and then call the **CreateSharedAccessPolicy()** method:</span></span>

```csharp
//Clear any existing access policies on container.
BlobContainerPermissions perms = container.GetPermissions();
perms.SharedAccessPolicies.Clear();
container.SetPermissions(perms);

//Create a new access policy on the container, which may be optionally used to provide constraints for
//shared access signatures on the container and the blob.
string sharedAccessPolicyName = "tutorialpolicy";
CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);
```

<span data-ttu-id="c2fb9-163">Ha törli a hozzáférési házirendek tárolóba, kell először lekérdezni a tároló meglévő engedélyeket, majd törölje a jelet az engedélyeket, majd állítsa be újra a engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-163">When you clear the access policies on a container, you must first get the container's existing permissions, then clear the permissions, then set the permissions again.</span></span>

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a><span data-ttu-id="c2fb9-164">A közös hozzáférésű jogosultságkód által használt a hozzáférési házirendek a tárolón URI generálása</span><span class="sxs-lookup"><span data-stu-id="c2fb9-164">Generate a shared access signature URI on the container that uses an access policy</span></span>
<span data-ttu-id="c2fb9-165">A következő létrehozhatunk egy másik közös hozzáférésű jogosultságkódot ahhoz a tárolóhoz, amelybe a korábban, de ezúttal a tárolt házirend az előző példában létrehozott azt társítsa az aláírás létrehozott.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-165">Next, we create another shared access signature for the container that we created earlier, but this time we associate the signature with the stored access policy we created in the previous example.</span></span>

<span data-ttu-id="c2fb9-166">Adja hozzá a tároló egy másik közös hozzáférésű jogosultságkódot létrehozni egy új módszer:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-166">Add a new method to generate another shared access signature for the container:</span></span>

```csharp
static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Generate the shared access signature on the container. In this case, all of the constraints for the
    //shared access signature are specified on the stored access policy.
    string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

    //Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

<span data-ttu-id="c2fb9-167">Alján a **Main()** metódus hívása előtt **Console.ReadLine()**, adja hozzá a következő sorokat hívni a **GetContainerSasUriWithPolicy** módszert:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-167">At the bottom of the **Main()** method, before the call to **Console.ReadLine()**, add the following lines to call the **GetContainerSasUriWithPolicy** method:</span></span>

```csharp
//Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a><span data-ttu-id="c2fb9-168">A közös hozzáférésű Jogosultságkód által használt a hozzáférési házirendek blobot URI generálása</span><span class="sxs-lookup"><span data-stu-id="c2fb9-168">Generate a Shared Access Signature URI on the Blob That Uses an Access Policy</span></span>
<span data-ttu-id="c2fb9-169">Végül azt adja hozzá a hasonló módszert, hozzon létre egy másik blob és tárolt hozzáférési házirenddel társított megosztott hozzáférési aláírást létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-169">Finally, we add a similar method to create another blob and generate a shared access signature that's associated with a stored access policy.</span></span>

<span data-ttu-id="c2fb9-170">Hozzon létre egy blobot, és egy közös hozzáférésű jogosultságkódot létrehozni egy új módszer hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-170">Add a new method to create a blob and generate a shared access signature:</span></span>

```csharp
static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Get a reference to a blob within the container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

    //Upload text to the blob. If the blob does not yet exist, it will be created.
    //If the blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible to clients via a shared access signature. " +
    "A stored access policy defines the constraints for the signature.";
    MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    ms.Position = 0;
    using (ms)
    {
        blob.UploadFromStream(ms);
    }

    //Generate the shared access signature on the blob.
    string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

<span data-ttu-id="c2fb9-171">Alján a **Main()** metódus hívása előtt **Console.ReadLine()**, adja hozzá a következő sorokat hívni a **GetBlobSasUriWithPolicy** módszert:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-171">At the bottom of the **Main()** method, before the call to **Console.ReadLine()**, add the following lines to call the **GetBlobSasUriWithPolicy** method:</span></span>

```csharp
//Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

<span data-ttu-id="c2fb9-172">A **Main()** metódus most példához hasonló egészében.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-172">The **Main()** method should now look like this in its entirety.</span></span> <span data-ttu-id="c2fb9-173">Futtassa a közös hozzáférésű jogosultságkódot URI-azonosítók írni a konzolablakban Ezután másolja és illessze be egy szövegfájlt, ez az oktatóanyag második része legyen.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-173">Run it to write the shared access signature URIs to the console window, then copy and paste them into a text file for use in the second part of this tutorial.</span></span>

```csharp
static void Main(string[] args)
{
    //Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create the blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container to use for the sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    Console.ReadLine();
}
```

<span data-ttu-id="c2fb9-174">Amikor futtatja a GenerateSharedAccessSignatures konzolalkalmazást, látni fogja, a következőhöz hasonló kimenetet.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-174">When you run the GenerateSharedAccessSignatures console application, you'll see output similar to the following.</span></span> <span data-ttu-id="c2fb9-175">Ezek azok a megosztott hozzáférési aláírásokkal használhatja az oktatóanyag az 2.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-175">These are the shared access signatures you use in Part 2 of the tutorial.</span></span>

```
Container SAS URI: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=pFlEZD%2F6sJTNLxD%2FQ26Hh85j%2FzYPxZav6mP1KJwnvJE%3D&se=2017-05-16T16%3A16%3A47Z&sp=wl

Blob SAS URI: https://storagesample.blob.core.windows.net/sascontainer/sasblob.txt?sv=2016-05-31&sr=b&sig=%2FiBWAZbXESzCMvRcm7JwJBK0gT0BtPSWEq4pRwmlBRI%3D&st=2017-05-15T16%3A11%3A48Z&se=2017-05-16T16%3A16%3A48Z&sp=rw

Container SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&si=tutorialpolicy&sig=aMb6rKDvvpfiGVsZI2rCmyUra6ZPpq%2BZ%2FLyTgAeec%2Bk%3D

Blob SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer/sasblobpolicy.txt?sv=2016-05-31&sr=b&si=tutorialpolicy&sig=%2FkTWkT23SS45%2FoF4bK2mqXkN%2BPKs%2FyHuzkfQ4GFoZVU%3D
```

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a><span data-ttu-id="c2fb9-176">2. lépés: Hozzon létre egy konzolalkalmazást a közös hozzáférésű jogosultságkód tesztelése</span><span class="sxs-lookup"><span data-stu-id="c2fb9-176">Part 2: Create a console application to test the shared access signatures</span></span>
<span data-ttu-id="c2fb9-177">A közös hozzáférésű jogosultságkód létrehozása a fenti példákban teszteléséhez jelenleg hozzon létre egy második konzolalkalmazást, amely a aláírásait használja a műveletek végrehajtásához, a tároló és a blob.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-177">To test the shared access signatures created in the previous examples, we create a second console application that uses the signatures to perform operations on the container and on a blob.</span></span>

> [!NOTE]
> <span data-ttu-id="c2fb9-178">Ha több mint 24 órája eltelt óta elvégezte az oktatóanyag első részét, az Ön által létrehozott aláírásokat már nem érvényesek.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-178">If more than 24 hours have passed since you completed the first part of the tutorial, the signatures you generated will no longer be valid.</span></span> <span data-ttu-id="c2fb9-179">Ebben az esetben az első Konzolalkalmazás friss közös hozzáférésű jogosultságkód használatra létrehozni az oktatóanyag második része a kódot kell futtatni.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-179">In this case, you should run the code in the first console application to generate fresh shared access signatures for use in the second part of the tutorial.</span></span>
>

<span data-ttu-id="c2fb9-180">A Visual Studio, hozzon létre egy új Windows-konzolalkalmazást, és adjon neki nevet **ConsumeSharedAccessSignatures**.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-180">In Visual Studio, create a new Windows console application and name it **ConsumeSharedAccessSignatures**.</span></span> <span data-ttu-id="c2fb9-181">Hivatkozásokat adni [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) és [windowsazure.Storage kifejezésre](https://www.nuget.org/packages/WindowsAzure.Storage/), mint korábban.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-181">Add references to [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) and [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), as you did previously.</span></span>

<span data-ttu-id="c2fb9-182">A Program.cs fájl felső részén adja hozzá a következő **használatával** irányelveket:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-182">At the top of the Program.cs file, add the following **using** directives:</span></span>

```csharp
using System.IO;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="c2fb9-183">Törzsében a **Main()** módszer, adja hozzá a következő karakterlánckonstansokat, az értékek módosítása a közös hozzáférésű jogosultságkód az oktatóanyag 1 részében létrehozott.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-183">In the body of the **Main()** method, add the following string constants, changing their values to the shared access signatures you generated in part 1 of the tutorial.</span></span>

```csharp
static void Main(string[] args)
{
    const string containerSAS = "<your container SAS>";
    const string blobSAS = "<your blob SAS>";
    const string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    const string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
}
```

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a><span data-ttu-id="c2fb9-184">Próbálja meg a tároló műveletek használatával a közös hozzáférésű jogosultságkód egy olyan metódus hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c2fb9-184">Add a method to try container operations using a shared access signature</span></span>
<span data-ttu-id="c2fb9-185">Ezután azt adja hozzá a módszere, amely megvizsgálja az egyes tároló műveletek használatával a közös hozzáférésű jogosultságkód ahhoz a tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-185">Next, we add a method that tests some container operations using a shared access signature for the container.</span></span> <span data-ttu-id="c2fb9-186">A közös hozzáférésű jogosultságkódot ad vissza egy hivatkozást a tárolóhoz, a tároló önmagában aláírás alapján való hozzáférés hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-186">The shared access signature is used to return a reference to the container, authenticating access to the container based on the signature alone.</span></span>

<span data-ttu-id="c2fb9-187">Adja hozzá a következő metódust a program.cs fájlt:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-187">Add the following method to Program.cs:</span></span>

```csharp
static void UseContainerSAS(string sas)
{
    //Try performing container operations with the SAS provided.

    //Return a reference to the container using the SAS URI.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

    //Create a list to store blob URIs returned by a listing operation on the container.
    List<ICloudBlob> blobList = new List<ICloudBlob>();

    //Write operation: write a new blob to the container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
        string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
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

    //List operation: List the blobs in the container.
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

    //Read operation: Get a reference to one of the blobs in the container and read it.
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

    //Delete operation: Delete a blob in the container.
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

<span data-ttu-id="c2fb9-188">Frissítés a **Main()** metódus hívására **UseContainerSAS()** mindkét a közös hozzáférésű jogosultságkód létrehozta a tárolón:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-188">Update the **Main()** method to call **UseContainerSAS()** with both of the shared access signatures you created on the container:</span></span>

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call the test methods with the shared access signatures created on the container, with and without the access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    Console.ReadLine();
}
```

### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a><span data-ttu-id="c2fb9-189">Egy blob műveletek használatával a közös hozzáférésű jogosultságkód kipróbálásához metódus hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c2fb9-189">Add a method to try blob operations using a shared access signature</span></span>
<span data-ttu-id="c2fb9-190">Végül azt adja hozzá a módszere, amely a blob néhány blob-műveletekbe egy közös hozzáférésű jogosultságkódot teszteli.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-190">Finally, we add a method that tests some blob operations using a shared access signature on the blob.</span></span> <span data-ttu-id="c2fb9-191">Ebben az esetben a konstruktor használjuk **CloudBlockBlob(String)**, tompított a közös hozzáférésű jogosultságkódot, a blobra mutató hivatkozást ad eredményül.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-191">In this case, we use the constructor **CloudBlockBlob(String)**, passing in the shared access signature, to return a reference to the blob.</span></span> <span data-ttu-id="c2fb9-192">Nincs más hitelesítés szükség; az aláírás önmagában alapul.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-192">No other authentication is required; it's based on the signature alone.</span></span>

<span data-ttu-id="c2fb9-193">Adja hozzá a következő metódust a program.cs fájlt:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-193">Add the following method to Program.cs:</span></span>

```csharp
static void UseBlobSAS(string sas)
{
    //Try performing blob operations using the SAS provided.

    //Return a reference to the blob using the SAS URI.
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

    //Write operation: Write a new blob to the container.
    try
    {
        string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
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

    //Read operation: Read the contents of the blob.
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

    //Delete operation: Delete the blob.
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

<span data-ttu-id="c2fb9-194">Frissítés a **Main()** metódus hívására **UseBlobSAS()** mindkét a közös hozzáférésű jogosultságkód blobot létrehozott:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-194">Update the **Main()** method to call **UseBlobSAS()** with both of the shared access signatures that you created on the blob:</span></span>

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call the test methods with the shared access signatures created on the container, with and without the access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
    UseBlobSAS(blobSAS);
    UseBlobSAS(blobSASWithAccessPolicy);

    Console.ReadLine();
}
```

<span data-ttu-id="c2fb9-195">Futtassa a konzolalkalmazást, és megfigyelheti, hogy mely műveleteket úgy is, mely aláírások kimeneti.</span><span class="sxs-lookup"><span data-stu-id="c2fb9-195">Run the console application and observe the output to see which operations are permitted for which signatures.</span></span> <span data-ttu-id="c2fb9-196">A konzolablakban a kimenet az alábbihoz hasonló fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="c2fb9-196">The output in the console window will look similar to the following:</span></span>

```
Write operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

List operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

Read operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: The remote server returned an error: (403) Forbidden.

Delete operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: The remote server returned an error: (403) Forbidden.

...
```

## <a name="next-steps"></a><span data-ttu-id="c2fb9-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2fb9-197">Next Steps</span></span>

* [<span data-ttu-id="c2fb9-198">Megosztott hozzáférési aláírásokkal, 1. lépés: Az SAS-modell ismertetése</span><span class="sxs-lookup"><span data-stu-id="c2fb9-198">Shared Access Signatures, Part 1: Understanding the SAS Model</span></span>](storage-dotnet-shared-access-signature-part-1.md)
* [<span data-ttu-id="c2fb9-199">Tárolók és blobok névtelen olvasási hozzáférés kezelése</span><span class="sxs-lookup"><span data-stu-id="c2fb9-199">Manage anonymous read access to containers and blobs</span></span>](storage-manage-access-to-resources.md)
* [<span data-ttu-id="c2fb9-200">A közös hozzáférésű jogosultságkód (REST API-t) hozzáférés delegálása</span><span class="sxs-lookup"><span data-stu-id="c2fb9-200">Delegating access with a shared access signature (REST API)</span></span>](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [<span data-ttu-id="c2fb9-201">Tábla- és várólista SAS bemutatása</span><span class="sxs-lookup"><span data-stu-id="c2fb9-201">Introducing Table and Queue SAS</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
