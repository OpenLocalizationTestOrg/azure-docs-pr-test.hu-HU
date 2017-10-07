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
# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Közös hozzáférésű Jogosultságkód, 2. rész: Létrehozása és SAS-kód használata a Blob-tároló

[1. rész](storage-dotnet-shared-access-signature-part-1.md) felfedezte az oktatóanyag a közös hozzáférésű jogosultságkód (SAS), és azok használatának ajánlott eljárásai alapján. 2. rész bemutatja, hogyan toogenerate, majd használja megosztott hozzáférési aláírásokkal rendelkező Blob-tároló. hello példák C# nyelven íródtak, és használja a hello Azure Storage ügyféloldali kódtára a .NET-hez. az oktatóanyagban szereplő példák hello:

* A közös hozzáférésű jogosultságkód egy tároló létrehozása
* A blob megosztott hozzáférési aláírást létrehozni
* Létrehoz egy tárolt hozzáférési házirend toomanage aláírások egy tároló-erőforrások
* Egy ügyfélalkalmazás hello megosztott hozzáférési aláírásokkal tesztelése

## <a name="about-this-tutorial"></a>Az oktatóanyag ismertetése
Ebben az oktatóanyagban létrehozhatunk két konzol alkalmazások, amelyek bemutatják, létrehozása és a tárolók és blobok közös hozzáférésű jogosultságkód használata:

**1 alkalmazás**: hello felügyeleti alkalmazás. A tároló és a blob egy közös hozzáférésű jogosultságkódot állít elő. Forráskód hello tárfiók_elérési_kulcsa tartalmazza.

**Alkalmazás 2**: hello ügyfélalkalmazást. Hozzáférések tároló és a blob erőforrások hello megosztott hozzáférési aláírásokkal hello első alkalmazással létrehozott használatával. Felhasználási csak hello közös hozzáférésű aláírások tooaccess tároló és a blob-erőforrások – az hajtja végre *nem* hello tárfiók_elérési_kulcsa tartalmazza.

## <a name="part-1-create-a-console-application-toogenerate-shared-access-signatures"></a>1. rész: Létrehoz konzol alkalmazás toogenerate megosztott hozzáférési aláírásokkal
Először győződjön meg róla, hogy rendelkezik-e hello Azure Storage ügyféloldali kódtára a .NET telepítése. Hello telepítése [NuGet-csomag](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet-csomag") tartalmazó hello tartozó hello ügyféloldali kódtár legújabb szerelvényeket. Ez az ajánlott módszer, amellyel biztosítható, hogy rendelkezik-e hello legújabb javításokat hello. Hello ügyféloldali kódtár legújabb verziójának hello hello részeként is letölthető [Azure SDK for .NET](https://azure.microsoft.com/downloads/).

A Visual Studio, hozzon létre egy új Windows-konzolalkalmazást, és adjon neki nevet **GenerateSharedAccessSignatures**. Hivatkozás hozzáadása túl[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) és [windowsazure.Storage kifejezésre](https://www.nuget.org/packages/WindowsAzure.Storage/) hello a következő módszerek egyikével:

* Használjon hello [NuGet-Csomagkezelő](https://docs.nuget.org/consume/installing-nuget) a Visual Studióban. Válassza ki **projekt** > **NuGet-csomagok kezelése**, keresse rá az interneten csomagonként (Microsoft.WindowsAzure.ConfigurationManager és windowsazure.Storage kifejezésre), és a telepítést.
* Keresse meg a szerelvények hello Azure SDK telepítése, és hivatkozások toothem hozzáadása:
  * Microsoft.WindowsAzure.Configuration.dll
  * Microsoft.WindowsAzure.Storage.dll

A Program.cs fájl hello hello tetején adja hozzá a hello következő **használatával** irányelveket:

```csharp
using System.IO;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

Hello app.config fájl szerkesztése, hogy a konfigurációs beállításokat egy olyan kapcsolati karakterlánccal, amely tooyour tárfiók mutat tartalmaz. Az app.config fájlt egy hasonló toothis kell kinéznie:

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

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>A közös hozzáférésű jogosultságkód URI egy tároló létrehozása
a toobegin, jelenleg felvenni egy metódus toogenerate egy közös hozzáférésű jogosultságkódot új tárolóba. Ebben az esetben hello aláírás nincs társítva egy tárolt házirend, így hello URI hello információt, amely jelzi, az a lejárati dátum- és hello engedélyeket, az általa ad.

Először adja hozzá a kódot toohello **Main()** metódus tooauthenticate tooyour tárfiók eléréséhez, és hozzon létre egy új tároló:

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

Ezután adja hozzá a közös hozzáférésű jogosultságkódot hello hello tároló hoz létre és hello aláírás URI metódus:

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

Adja hozzá az alábbi hello hello alján hello **Main()** módszer, mielőtt hello hívása túl**Console.ReadLine()**, toocall **GetContainerSasUri()** és hello írása aláírás URI toohello konzolablak:

```csharp
//Generate a SAS URI for hello container, without a stored access policy.
Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
Console.WriteLine();
```

Fordítsa le, és futtassa a toooutput hello közös hozzáférésű jogosultságkódot URI hello új tároló. hello URI hasonló toohello következő lesz:

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D
```

Hello kód futtatása után hello közös hozzáférésű jogosultságkódot hello tároló létrehozott érvényes lesz hello 24 órán át. hello aláírás biztosít egy ügyfél engedélyt toolist blobokat hello tárolóhoz és annak toowrite új blobok toohello tároló.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>A közös hozzáférésű jogosultságkód URI egy BLOB készítése
Ezután azt írási hasonló kód toocreate egy új blob hello tárolóban, és egy közös hozzáférésű jogosultságkódot létrehozni hozzá. A közös hozzáférésű jogosultságkódot nincs társítva egy tárolt hozzáférési házirend, hello URI hello kezdési ideje, a lejárat időpontjának és a jogosultsági információ tartalmazza azt.

Adja hozzá egy új hoz létre egy új blob és néhány szöveg tooit ír majd állít elő, a közös hozzáférésű jogosultságkód és metódus hello aláírás URI:

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

Hello hello alján **Main()** módszer, adja hozzá a következő sorokat toocall hello **GetBlobSasUri()**, mielőtt hello hívása túl**Console.ReadLine()**, és megosztott hello írása hozzáférési aláírás URI toohello konzolablak:

```csharp
//Generate a SAS URI for a blob within hello container, without a stored access policy.
Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
Console.WriteLine();
```

Fordítsa le, és futtassa a toooutput hello közös hozzáférésű jogosultságkódot URI hello új blob. hello URI hasonló toohello következő lesz:

```
https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D
```

### <a name="create-a-stored-access-policy-on-hello-container"></a>Hello tárolóban tárolt hozzáférési házirend létrehozása
Most hozzon létre egy tárolt házirend hello tárolóra, amelyek meghatározzák hello megkötéseit minden megosztott hozzáférési aláírásokkal, amely hozzá van rendelve.

Az előző példákban hello azt megadott hello kezdési ideje, (implicit vagy explicit módon), hello lejárati idejének és hello hello engedélyeinek megosztott hozzáférési aláírás URI magát. A következő példák hello adtuk meg ezek tárolt hello hozzáférési házirend, nem a hello közös hozzáférésű jogosultságkóddal. Ez igen lehetővé teszi, hogy velünk ezek a megkötések nélkül hello hosszkorlátját megosztott toochange aláírás érhető el.

Egy vagy több hello korlátozza a hello közös hozzáférésű jogosultságkódot, és a hozzáférési házirendben tárolt hello hello fennmaradó lehetséges toohave. Azonban csak megadhat hello kezdési ideje, a lejárat időpontjának és az engedélyek egy hely vagy más hello. Például nem hello közös hozzáférésű jogosultságkódot engedélyeket ad meg, és is adja meg azokat hello tárolt házirend.

Amikor egy tárolt hozzáférés-tooa tároló, hello tároló meglévő engedélyeket szerezni, hello új házirend hozzáadása, és ezután a hello tároló engedélyek beállítása.

Adja hozzá egy új módszer, amely létrehoz egy új tárolt házirend tárolóba, és hello házirend hello nevét adja vissza:

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

Hello hello alján **Main()** módszer, mielőtt hello hívása túl**Console.ReadLine()**, adja hozzá a következő hello sorok toofirst törölje a meglévő hozzáférési házirendek és hívhatja hello  **CreateSharedAccessPolicy()** módszert:

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

Ha törli a hello hozzáférési házirendek tárolóba, kell először hello tároló meglévő engedélyeket, majd törölje a jelet hello engedélyek lekérése, majd újra hello engedélyek beállítása.

### <a name="generate-a-shared-access-signature-uri-on-hello-container-that-uses-an-access-policy"></a>A közös hozzáférésű jogosultságkód URI hello tárolóra által használt a hozzáférési házirend létrehozása
A következő létrehozhatunk egy másik közös hozzáférésű jogosultságkódot, amely a korábban létrehozott hello tároló, de ezúttal azt társítsa hello aláírás hello előző példában létrehozott hello tárolt házirend.

Adja hozzá egy új módszer toogenerate egy másik közös hozzáférésű jogosultságkódot hello tároló:

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

Hello hello alján **Main()** módszer, mielőtt hello hívása túl**Console.ReadLine()**, adja hozzá a következő sorokat toocall hello hello **GetContainerSasUriWithPolicy** módszer :

```csharp
//Generate a SAS URI for hello container, using a stored access policy tooset constraints on hello SAS.
Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

### <a name="generate-a-shared-access-signature-uri-on-hello-blob-that-uses-an-access-policy"></a>Megosztott hozzáférési aláírást URI Azonosítójának generálása hello Blob, hogy használja a hozzáférési házirend
Végül azt egy hasonló metódus toocreate hozzáadása egy másik blob, és tárolt hozzáférési házirenddel társított megosztott hozzáférési aláírást létrehozni.

Adja hozzá egy új módszer toocreate blob, és egy közös hozzáférésű jogosultságkódot létrehozni:

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

Hello hello alján **Main()** módszer, mielőtt hello hívása túl**Console.ReadLine()**, adja hozzá a következő sorokat toocall hello hello **GetBlobSasUriWithPolicy** módszert:

```csharp
//Generate a SAS URI for a blob within hello container, using a stored access policy tooset constraints on hello SAS.
Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

Hello **Main()** metódus most példához hasonló egészében. Toowrite hello közös hozzáférésű jogosultságkódot URI-azonosítók toohello konzolablakot, majd futtassa másolhat és illessze be egy szövegfájlt hello a második rész a jelen oktatóanyag használható.

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

Hello GenerateSharedAccessSignatures Konzolalkalmazás futtatásakor kimeneti hasonló toohello következő láthatja. Ezek a hello megosztott hozzáférési aláírásokkal használhatja az hello oktatóanyag 2.

```
Container SAS URI: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=pFlEZD%2F6sJTNLxD%2FQ26Hh85j%2FzYPxZav6mP1KJwnvJE%3D&se=2017-05-16T16%3A16%3A47Z&sp=wl

Blob SAS URI: https://storagesample.blob.core.windows.net/sascontainer/sasblob.txt?sv=2016-05-31&sr=b&sig=%2FiBWAZbXESzCMvRcm7JwJBK0gT0BtPSWEq4pRwmlBRI%3D&st=2017-05-15T16%3A11%3A48Z&se=2017-05-16T16%3A16%3A48Z&sp=rw

Container SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&si=tutorialpolicy&sig=aMb6rKDvvpfiGVsZI2rCmyUra6ZPpq%2BZ%2FLyTgAeec%2Bk%3D

Blob SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer/sasblobpolicy.txt?sv=2016-05-31&sr=b&si=tutorialpolicy&sig=%2FkTWkT23SS45%2FoF4bK2mqXkN%2BPKs%2FyHuzkfQ4GFoZVU%3D
```

## <a name="part-2-create-a-console-application-tootest-hello-shared-access-signatures"></a>2. lépés: Létrehoz konzol alkalmazás tootest hello megosztott hozzáférési aláírásokkal
tootest hello közös hozzáférésű jogosultságkód hello előző példákban létrehozott, a Microsoft hello tároló és a blob hello aláírások tooperform műveletek használó második Konzolalkalmazás létrehozása.

> [!NOTE]
> Ha több mint 24 órája eltelt óta hello oktatóanyag első részét hello elvégezte, hello aláírások akkor jönnek létre már nem érvényesek. Ebben az esetben kell futtatni hello kód hello első console application toogenerate friss közös hozzáférésű jogosultságkód hello hello oktatóanyag második része legyen.
>

A Visual Studio, hozzon létre egy új Windows-konzolalkalmazást, és adjon neki nevet **ConsumeSharedAccessSignatures**. Hivatkozás hozzáadása túl[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) és [windowsazure.Storage kifejezésre](https://www.nuget.org/packages/WindowsAzure.Storage/), mint korábban.

A Program.cs fájl hello hello tetején adja hozzá a hello következő **használatával** irányelveket:

```csharp
using System.IO;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

A hello hello törzsét **Main()** módszer, adja hozzá a következő karakterlánc állandók, az értékek toohello megosztott hozzáférési aláírásokkal hello oktatóanyag 1 részében létrehozott módosítása hello.

```csharp
static void Main(string[] args)
{
    const string containerSAS = "<your container SAS>";
    const string blobSAS = "<your blob SAS>";
    const string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    const string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
}
```

### <a name="add-a-method-tootry-container-operations-using-a-shared-access-signature"></a>A metódus tootry tároló műveletek használatával a közös hozzáférésű jogosultságkód hozzáadása
Ezután azt adja hozzá a módszere, amely az egyes tároló műveletek használatával a közös hozzáférésű jogosultságkód hello tároló teszteli. hello közös hozzáférésű jogosultságkódot hivatkozás toohello tárolója, hello aláírás önmagában alapuló hozzáférés toohello tároló hitelesítéséhez használt tooreturn.

Adja hozzá a következő metódus tooProgram.cs hello:

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

Frissítés hello **Main()** metódus toocall **UseContainerSAS()** mindkét hello megosztott hozzáférési aláírásokkal hello tárolójához létrehozott:

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

### <a name="add-a-method-tootry-blob-operations-using-a-shared-access-signature"></a>A metódus tootry blob műveletek használatával a közös hozzáférésű jogosultságkód hozzáadása
Végül azt adja hozzá egy módszert, amelyet hello blob néhány blob-műveletekbe egy közös hozzáférésű jogosultságkódot teszteli. Ebben az esetben használjuk hello konstruktor **CloudBlockBlob(String)**, a hello közös hozzáférésű jogosultságkódot, tooreturn hivatkozás toohello blob tompított. Nincs más hitelesítés szükség; hello aláírás önmagában alapul.

Adja hozzá a következő metódus tooProgram.cs hello:

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

Frissítés hello **Main()** metódus toocall **UseBlobSAS()** mindkét hello megosztott hozzáférési aláírásokkal, amelyet a hello blob hozott létre:

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

Futtassa a konzolalkalmazást hello, és tekintse meg az hello kimeneti toosee mely aláírások milyen műveletek engedélyezettek. hello kimeneti hello konzolablakban hasonló toohello következő fog kinézni:

```
Write operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

List operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

Read operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: hello remote server returned an error: (403) Forbidden.

Delete operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: hello remote server returned an error: (403) Forbidden.

...
```

## <a name="next-steps"></a>Következő lépések

* [Közös hozzáférésű Jogosultságkód, 1. rész: Hello SAS-modell ismertetése](storage-dotnet-shared-access-signature-part-1.md)
* [Kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](storage-manage-access-to-resources.md)
* [A közös hozzáférésű jogosultságkód (REST API-t) hozzáférés delegálása](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [Tábla- és várólista SAS bemutatása](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
