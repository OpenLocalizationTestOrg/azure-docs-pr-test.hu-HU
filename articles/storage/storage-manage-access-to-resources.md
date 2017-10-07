---
title: "aaaEnable nyilvános olvasási hozzáférés a tárolók és blobok az Azure Blob storage |} Microsoft Docs"
description: "Megtudhatja, hogyan toomake tárolók és blobok névtelen hozzáférés, elérhető, és hogyan tooaccess őket programozott módon."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: a2cffee6-3224-4f2a-8183-66ca23b2d2d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: marsma
ms.openlocfilehash: 0675b5dc4d32a3a0a34376ae4c049542b07ba03a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-anonymous-read-access-toocontainers-and-blobs"></a>Kezelheti a névtelen olvasási hozzáférés toocontainers és blobok
Névtelen, nyilvános olvasási hozzáférés tooa tároló és a benne található blobokat az Azure Blob storage használatával engedélyezheti. Ezzel a módszerrel csak olvasási hozzáféréssel toothese erőforrások megosztása a fiókkulcs nélkül, és anélkül, hogy a közös hozzáférésű jogosultságkód (SAS) adhat meg.

Nyilvános olvasási hozzáférés esetén ajánlott használni kívánt egyes blobok tooalways érhetők el a névtelen olvasási hozzáférés forgatókönyvek esetén. Részletesebb vezérlést létrehozhat egy közös hozzáférésű jogosultságkódot. Közös hozzáférésű jogosultságkód lehetővé teszik a korlátozott tooprovide hozzáférés eltérő engedélyekkel, egy adott időszakra vonatkozóan. Megosztott létrehozásával kapcsolatos további információért aláírások hozzáférni, lásd: [használata közös hozzáférésű jogosultságkód (SAS) az Azure Storage](storage-dotnet-shared-access-signature-part-1.md).

## <a name="grant-anonymous-users-permissions-toocontainers-and-blobs"></a>Adja meg a névtelen felhasználók engedélyek toocontainers és blobok
Alapértelmezés szerint a tároló és szereplő bármely BLOB érhetik csak el hello tárfiók hello tulajdonosa. toogive névtelen felhasználók olvasási engedéllyel a tooa és a benne található blobokat, hello tároló engedélyek tooallow nyilvános hozzáférés állíthatja be. Névtelen felhasználók olvashatják a nyilvánosan elérhető tárolóban található blobok hello kérelem hitelesítése nélkül.

Az alábbi engedélyek használata hello egy tárolót is konfigurálhatja:

* **Nincs nyilvános olvasási hozzáférés:** hello tároló és a benne található blobokat hozzáférhet csak hello tárfiók tulajdonosa. Ez a hello alapértelmezett az összes új tárolókat.
* **Nyilvános olvasási hozzáférés a blobok csak:** hello tárolóban található Blobok névtelen kérelem által is olvasható, de adatai nem érhető el. Névtelen ügyfeleket nem tudja felsorolni hello blobok hello tárolóban.
* **Teljes nyilvános olvasási hozzáférés:** minden tároló és a blob adatainak névtelen kérelem által olvasható. Ügyfelek hello tárolóban található blobok névtelen kérelem enumerálása, de nem tudja felsorolni a tárolók hello tárfiókon belül.

Használhatja az alábbi tooset tároló engedélyek hello:

* [Azure Portal](https://portal.azure.com)
* [Azure PowerShell](storage-powershell-guide-full.md#how-to-manage-azure-blobs)
* [Azure CLI 2.0](storage-azure-cli.md#create-and-manage-blobs)
* Programozott módon egyikével hello storage ügyfélkódtáraival vagy hello REST API-n

### <a name="set-container-permissions-in-hello-azure-portal"></a>Hello Azure-portálon a tároló engedélyeinek beállítása
hello tooset tároló engedélyek [Azure-portálon](https://portal.azure.com), kövesse az alábbi lépéseket:

1. Nyissa meg a **tárfiók** hello portál panel. A tárfiók található kiválasztásával **tárfiókok** hello portál főmenü panelen.
1. A **BLOB szolgáltatás** hello menü paneljén válassza **tárolók**.
1. Kattintson a jobb gombbal a hello tároló sor- vagy select hello három pont tooopen hello tároló **helyi menü**.
1. Válassza ki **házirendhez** hello helyi menü.
1. Válasszon egy **hozzáférési típus** hello a legördülő menü.

    ![Szerkessze a Csomagtároló metaadatai párbeszédpanel](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a>A .NET tároló engedélyeinek beállítása
C# és a Storage ügyféloldali kódtára hello használata a .NET-hez, a tároló engedélyeinek tooset hello tároló meglévő engedélyek először lekérdezése által hívó hello **GetPermissions** metódust. Majd a készlet hello **PublicAccess** hello tulajdonsága **BlobContainerPermissions** hello által visszaadott objektum **GetPermissions** metódust. Végezetül hívás hello **SetPermissions** hello metódus engedélyek frissítése.

a következő példa hello toofull nyilvános olvasási hozzáférés hello tároló engedélyeket állít be. tooset engedélyek toopublic olvasási hozzáféréssel a blobok csak, állítsa be a hello **PublicAccess** tulajdonság túl**BlobContainerPublicAccessType.Blob**. tooremove névtelen felhasználók minden engedélyeinek beállítása tulajdonság túl hello**BlobContainerPublicAccessType.Off**.

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a>Tárolók és blobok névtelen eléréséhez
Ügyfél tárolók és blobok névtelen hozzáféréshez használhatja a konstruktorok, amelyekhez nem szükséges hitelesítő adatokat. a következő példák hello többféleképpen tooreference Blob szolgáltatás erőforrások megjelenítése névtelenül.

### <a name="create-an-anonymous-client-object"></a>Névtelen ügyfél objektum létrehozása
Létrehozhat egy új ügyfél szolgáltatásobjektum névtelen hozzáférés hello Blob-szolgáltatásvégpont hello fiók megadásával. Azonban is ismernie kell a tároló, amelyet a névtelen hozzáférés fiókhoz tartozó hello nevének.

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create hello client object using hello Blob service endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference tooa container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read hello container's properties. Note this is only possible when hello container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a>Egy tároló névtelenül hivatkozik
Ha hello URL-cím tooa tároló, amely névtelenül érhető el, használhatja tooreference hello tároló közvetlenül.

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference tooa container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in hello container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a>Egy blob névtelenül hivatkozik
Ha hello URL-cím tooa blob, amelyet a névtelen hozzáférés, melyeket referenciaként használhat hello blob közvetlenül az adott URL-cím segítségével:

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-tooanonymous-users"></a>Szolgáltatások rendelkezésre tooanonymous felhasználók
a következő táblázat hello jeleníti meg, milyen műveletek előfordulhat, hogy hívható névtelen felhasználók számára, ha a tároló hozzáférés-vezérlési lista tooallow nyilvános hozzáférés van beállítva.

| REST-művelet | Teljes körű nyilvános olvasási joggal rendelkező engedély | A blobok csak nyilvános olvasási joggal rendelkező engedély |
| --- | --- | --- |
| Tárolók listája |Csak a tulajdonos |Csak a tulajdonos |
| Tároló létrehozása |Csak a tulajdonos |Csak a tulajdonos |
| A tároló tulajdonságainak beolvasása |Összes |Csak a tulajdonos |
| Tároló metaadatot beszerezni |Összes |Csak a tulajdonos |
| Állítsa be a Csomagtároló metaadatai |Csak a tulajdonos |Csak a tulajdonos |
| Tároló hozzáférés-vezérlési lista beolvasása |Csak a tulajdonos |Csak a tulajdonos |
| Tároló hozzáférés-vezérlési lista beállítása |Csak a tulajdonos |Csak a tulajdonos |
| Törli a tárolót |Csak a tulajdonos |Csak a tulajdonos |
| Lista Blobok |Összes |Csak a tulajdonos |
| Helyezze a Blob |Csak a tulajdonos |Csak a tulajdonos |
| A Blob beolvasása |Összes |Összes |
| A Blob tulajdonságainak beolvasása |Összes |Összes |
| A Blob tulajdonságainak beállítása |Csak a tulajdonos |Csak a tulajdonos |
| A Blob metaadatot beszerezni |Összes |Összes |
| Állítsa be a Blob metaadatai |Csak a tulajdonos |Csak a tulajdonos |
| PUT letiltása |Csak a tulajdonos |Csak a tulajdonos |
| (Csak a véglegesített blokkolja) tiltólista beolvasása |Összes |Összes |
| (Csak a nem véglegesített blokkok vagy blokkok) tiltólista beolvasása |Csak a tulajdonos |Csak a tulajdonos |
| PUT tiltólista |Csak a tulajdonos |Csak a tulajdonos |
| A Blob törlése |Csak a tulajdonos |Csak a tulajdonos |
| A Blob másolása |Csak a tulajdonos |Csak a tulajdonos |
| Pillanatkép Blob |Csak a tulajdonos |Csak a tulajdonos |
| Címbérlet Blob |Csak a tulajdonos |Csak a tulajdonos |
| Helyezze a lap |Csak a tulajdonos |Csak a tulajdonos |
| Get tartományokat |Összes |Összes |
| Hozzáfűző Blob |Csak a tulajdonos |Csak a tulajdonos |

## <a name="next-steps"></a>Következő lépések

* [Hello Azure Storage szolgáltatásainak hitelesítése](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [Közös hozzáférésű Jogosultságkód (SAS) használatával](storage-dotnet-shared-access-signature-part-1.md)
* [Hozzáférés delegálása közös hozzáférésű jogosultságkód használatával](https://msdn.microsoft.com/library/azure/ee395415.aspx)
