---
title: "aaaSet és lekérése objektum tulajdonságait és a metaadatok az Azure Storage |} Microsoft Docs"
description: "Egyéni metaadat tárolásához az Azure Storage-objektumokhoz, és állítsa be, és Rendszertulajdonságok lekéréséhez."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 036f9006-273e-400b-844b-3329045e9e1f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: marsma
ms.openlocfilehash: 44f9243183014845964f337b476a6b0069dc0902
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-and-retrieve-properties-and-metadata"></a><span data-ttu-id="084e6-103">Tulajdonságok és metaadatok beállítása és lekérése</span><span class="sxs-lookup"><span data-stu-id="084e6-103">Set and retrieve properties and metadata</span></span>

<span data-ttu-id="084e6-104">Objektumok Azure Storage támogatási Rendszertulajdonságok és a felhasználó által definiált metaadatok, továbbá toohello adatokat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="084e6-104">Objects in Azure Storage support system properties and user-defined metadata, in addition toohello data they contain.</span></span> <span data-ttu-id="084e6-105">Ez a cikk ismerteti, amelyek kezelése rendszer tulajdonságai és a felhasználó által definiált metaadatok hello [Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span><span class="sxs-lookup"><span data-stu-id="084e6-105">This article discusses managing system properties and user-defined metadata with hello [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span></span>

* <span data-ttu-id="084e6-106">**A Rendszertulajdonságok**: Rendszertulajdonságok egyes tárolási erőforrások léteznek.</span><span class="sxs-lookup"><span data-stu-id="084e6-106">**System properties**: System properties exist on each storage resource.</span></span> <span data-ttu-id="084e6-107">Némelyikük olvasása vagy beállítása, míg mások csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="084e6-107">Some of them can be read or set, while others are read-only.</span></span> <span data-ttu-id="084e6-108">Hello színfalak néhány Rendszertulajdonságok toocertain szabványos HTTP-fejlécek felelnek meg.</span><span class="sxs-lookup"><span data-stu-id="084e6-108">Under hello covers, some system properties correspond toocertain standard HTTP headers.</span></span> <span data-ttu-id="084e6-109">Ezek az Ön hello Azure storage ügyféloldali kódtár tart fenn.</span><span class="sxs-lookup"><span data-stu-id="084e6-109">hello Azure storage client library maintains these for you.</span></span>

* <span data-ttu-id="084e6-110">**Felhasználó által definiált metaadatok**: felhasználói metaadatai metaadatokat, amelyek egy adott erőforráshoz hello képernyőn név-érték pár ad meg.</span><span class="sxs-lookup"><span data-stu-id="084e6-110">**User-defined metadata**: User-defined metadata is metadata that you specify on a given resource in hello form of a name-value pair.</span></span> <span data-ttu-id="084e6-111">A tároló egyik erőforrásához a metaadatok toostore további értékeket is használhat.</span><span class="sxs-lookup"><span data-stu-id="084e6-111">You can use metadata toostore additional values with a storage resource.</span></span> <span data-ttu-id="084e6-112">Ezek további metaadatokat az értékek csak a saját célokra, és nincsenek hatással a hello erőforrás működését.</span><span class="sxs-lookup"><span data-stu-id="084e6-112">These additional metadata values are for your own purposes only, and do not affect how hello resource behaves.</span></span>

<span data-ttu-id="084e6-113">A tároló egyik erőforrásához tulajdonság és metaadatok értékeinek beolvasásakor két lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="084e6-113">Retrieving property and metadata values for a storage resource is a two-step process.</span></span> <span data-ttu-id="084e6-114">Olvasása előtt hasznos lehet ezeket az értékeket, akkor kell explicit módon fetch őket hívó hello által **FetchAttributes** metódust.</span><span class="sxs-lookup"><span data-stu-id="084e6-114">Before you can read these values, you must explicitly fetch them by calling hello **FetchAttributes** method.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="084e6-115">A tároló egyik erőforrásához tulajdonság és a metaadat értéke nincs feltöltve, kivéve, ha meghívja a hello egyik **FetchAttributes** módszerek.</span><span class="sxs-lookup"><span data-stu-id="084e6-115">Property and metadata values for a storage resource are not populated unless you call one of hello **FetchAttributes** methods.</span></span>
>
> <span data-ttu-id="084e6-116">Kapni fog egy `400 Bad Request` , ha a név/érték párok nem ASCII-karaktereket tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="084e6-116">You will receive a `400 Bad Request` if any name/value pairs contain non-ASCII characters.</span></span> <span data-ttu-id="084e6-117">Metaadatok név/érték párok érvényes HTTP-fejlécek, ezért a HTTP-fejlécek szabályozó tooall korlátozások meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="084e6-117">Metadata name/value pairs are valid HTTP headers, and so must adhere tooall restrictions governing HTTP headers.</span></span> <span data-ttu-id="084e6-118">Ezért ajánlott URL-kódolást vagy a Base64 kódolás neveit és értékeit tartalmazó, nem ASCII-karaktereket használja.</span><span class="sxs-lookup"><span data-stu-id="084e6-118">It is therefore recommended that you use URL encoding or Base64 encoding for names and values containing non-ASCII characters.</span></span>
>

## <a name="setting-and-retrieving-properties"></a><span data-ttu-id="084e6-119">És tulajdonságainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="084e6-119">Setting and retrieving properties</span></span>
<span data-ttu-id="084e6-120">tooretrieve tulajdonságértékek, hívás hello **FetchAttributes** metódust a blob vagy tároló toopopulate hello tulajdonságokat, majd olvassa el a hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="084e6-120">tooretrieve property values, call hello **FetchAttributes** method on your blob or container toopopulate hello properties, then read hello values.</span></span>

<span data-ttu-id="084e6-121">egy objektum tooset tulajdonságainak hello tulajdonság értékét adja meg, majd hívjon hello **SetProperties metódus** metódust.</span><span class="sxs-lookup"><span data-stu-id="084e6-121">tooset properties on an object, specify hello property value, then call hello **SetProperties** method.</span></span>

<span data-ttu-id="084e6-122">hello alábbi példakód létrehoz egy tárolót, majd a tulajdonság értékek tooa konzolablak részénél írja.</span><span class="sxs-lookup"><span data-stu-id="084e6-122">hello following code example creates a container, then writes some of its property values tooa console window.</span></span>

```csharp
//Parse hello connection string for hello storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

//Create hello service client object for credentialed access toohello Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create hello container if it does not already exist.
container.CreateIfNotExists();

// Fetch container properties and write out their values.
container.FetchAttributes();
Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
Console.WriteLine("ETag: {0}", container.Properties.ETag);
Console.WriteLine();
```

## <a name="setting-and-retrieving-metadata"></a><span data-ttu-id="084e6-123">És metaadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="084e6-123">Setting and retrieving metadata</span></span>
<span data-ttu-id="084e6-124">Egy vagy több név-érték párként blob vagy tároló erőforrásra vonatkozó metaadatok is megadhat.</span><span class="sxs-lookup"><span data-stu-id="084e6-124">You can specify metadata as one or more name-value pairs on a blob or container resource.</span></span> <span data-ttu-id="084e6-125">tooset metaadatok, vegye fel a név-érték párok toohello **metaadatok** gyűjtemény hello erőforráson, majd hívja a hello **SetMetadata** metódus toosave hello értékek toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="084e6-125">tooset metadata, add name-value pairs toohello **Metadata** collection on hello resource, then call hello **SetMetadata** method toosave hello values toohello service.</span></span>

> [!NOTE]
> <span data-ttu-id="084e6-126">a metaadatok hello nevét meg kell felelnie a C# azonosítók toohello elnevezési szabályai.</span><span class="sxs-lookup"><span data-stu-id="084e6-126">hello name of your metadata must conform toohello naming conventions for C# identifiers.</span></span>
>
>

<span data-ttu-id="084e6-127">hello alábbi példakód beállítja a metaadatok tárolóba.</span><span class="sxs-lookup"><span data-stu-id="084e6-127">hello following code example sets metadata on a container.</span></span> <span data-ttu-id="084e6-128">Egy változó értéke hello gyűjtemény használata **Hozzáadás** metódust.</span><span class="sxs-lookup"><span data-stu-id="084e6-128">One value is set using hello collection's **Add** method.</span></span> <span data-ttu-id="084e6-129">hello más értéke implicit kulcs/érték-szintaxis használatával.</span><span class="sxs-lookup"><span data-stu-id="084e6-129">hello other value is set using implicit key/value syntax.</span></span> <span data-ttu-id="084e6-130">Egyaránt érvényesek.</span><span class="sxs-lookup"><span data-stu-id="084e6-130">Both are valid.</span></span>

```csharp
public static void AddContainerMetadata(CloudBlobContainer container)
{
    //Add some metadata toohello container.
    container.Metadata.Add("docType", "textDocuments");
    container.Metadata["category"] = "guidance";

    //Set hello container's metadata.
    container.SetMetadata();
}
```

<span data-ttu-id="084e6-131">tooretrieve metaadatok, hívás hello **FetchAttributes** a blob vagy tároló toopopulate hello metódusa **metaadatok** gyűjtemény, olvassa el hello értékeket, az alábbi hello példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="084e6-131">tooretrieve metadata, call hello **FetchAttributes** method on your blob or container toopopulate hello **Metadata** collection, then read hello values, as shown in hello example below.</span></span>

```csharp
public static void ListContainerMetadata(CloudBlobContainer container)
{
    //Fetch container attributes in order toopopulate hello container's properties and metadata.
    container.FetchAttributes();

    //Enumerate hello container's metadata.
    Console.WriteLine("Container metadata:");
    foreach (var metadataItem in container.Metadata)
    {
        Console.WriteLine("\tKey: {0}", metadataItem.Key);
        Console.WriteLine("\tValue: {0}", metadataItem.Value);
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="084e6-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="084e6-132">Next steps</span></span>
* [<span data-ttu-id="084e6-133">Az Azure Storage ügyféloldali kódtára a .NET-referencia</span><span class="sxs-lookup"><span data-stu-id="084e6-133">Azure Storage Client Library for .NET reference</span></span>](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [<span data-ttu-id="084e6-134">Az Azure Storage ügyféloldali kódtára a .NET NuGet-csomag</span><span class="sxs-lookup"><span data-stu-id="084e6-134">Azure Storage Client Library for .NET NuGet package</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
