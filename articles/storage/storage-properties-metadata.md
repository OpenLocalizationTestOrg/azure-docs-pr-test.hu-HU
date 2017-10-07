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
# <a name="set-and-retrieve-properties-and-metadata"></a>Tulajdonságok és metaadatok beállítása és lekérése

Objektumok Azure Storage támogatási Rendszertulajdonságok és a felhasználó által definiált metaadatok, továbbá toohello adatokat tartalmaznak. Ez a cikk ismerteti, amelyek kezelése rendszer tulajdonságai és a felhasználó által definiált metaadatok hello [Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).

* **A Rendszertulajdonságok**: Rendszertulajdonságok egyes tárolási erőforrások léteznek. Némelyikük olvasása vagy beállítása, míg mások csak olvasható. Hello színfalak néhány Rendszertulajdonságok toocertain szabványos HTTP-fejlécek felelnek meg. Ezek az Ön hello Azure storage ügyféloldali kódtár tart fenn.

* **Felhasználó által definiált metaadatok**: felhasználói metaadatai metaadatokat, amelyek egy adott erőforráshoz hello képernyőn név-érték pár ad meg. A tároló egyik erőforrásához a metaadatok toostore további értékeket is használhat. Ezek további metaadatokat az értékek csak a saját célokra, és nincsenek hatással a hello erőforrás működését.

A tároló egyik erőforrásához tulajdonság és metaadatok értékeinek beolvasásakor két lépésből áll. Olvasása előtt hasznos lehet ezeket az értékeket, akkor kell explicit módon fetch őket hívó hello által **FetchAttributes** metódust.

> [!IMPORTANT]
> A tároló egyik erőforrásához tulajdonság és a metaadat értéke nincs feltöltve, kivéve, ha meghívja a hello egyik **FetchAttributes** módszerek.
>
> Kapni fog egy `400 Bad Request` , ha a név/érték párok nem ASCII-karaktereket tartalmaznak. Metaadatok név/érték párok érvényes HTTP-fejlécek, ezért a HTTP-fejlécek szabályozó tooall korlátozások meg kell felelnie. Ezért ajánlott URL-kódolást vagy a Base64 kódolás neveit és értékeit tartalmazó, nem ASCII-karaktereket használja.
>

## <a name="setting-and-retrieving-properties"></a>És tulajdonságainak beolvasása
tooretrieve tulajdonságértékek, hívás hello **FetchAttributes** metódust a blob vagy tároló toopopulate hello tulajdonságokat, majd olvassa el a hello értékeket.

egy objektum tooset tulajdonságainak hello tulajdonság értékét adja meg, majd hívjon hello **SetProperties metódus** metódust.

hello alábbi példakód létrehoz egy tárolót, majd a tulajdonság értékek tooa konzolablak részénél írja.

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

## <a name="setting-and-retrieving-metadata"></a>És metaadatok beolvasása
Egy vagy több név-érték párként blob vagy tároló erőforrásra vonatkozó metaadatok is megadhat. tooset metaadatok, vegye fel a név-érték párok toohello **metaadatok** gyűjtemény hello erőforráson, majd hívja a hello **SetMetadata** metódus toosave hello értékek toohello szolgáltatás.

> [!NOTE]
> a metaadatok hello nevét meg kell felelnie a C# azonosítók toohello elnevezési szabályai.
>
>

hello alábbi példakód beállítja a metaadatok tárolóba. Egy változó értéke hello gyűjtemény használata **Hozzáadás** metódust. hello más értéke implicit kulcs/érték-szintaxis használatával. Egyaránt érvényesek.

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

tooretrieve metaadatok, hívás hello **FetchAttributes** a blob vagy tároló toopopulate hello metódusa **metaadatok** gyűjtemény, olvassa el hello értékeket, az alábbi hello példában látható módon.

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

## <a name="next-steps"></a>Következő lépések
* [Az Azure Storage ügyféloldali kódtára a .NET-referencia](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [Az Azure Storage ügyféloldali kódtára a .NET NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.Storage/)
