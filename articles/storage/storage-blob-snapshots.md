---
title: "az Azure Storage blob írásvédett pillanatfelvételt aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy blob tooback idő az adott pillanatban blob adatok pillanatképe. Megérteni, hogyan pillanatképek vannak számlázva, és hogyan toouse toominimize kapacitás díjak őket."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 3710705d-e127-4b01-8d0f-29853fb06d0d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: marsma
ms.openlocfilehash: 9e7af611e885d83df69d3329217f261b3f3f333e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-blob-snapshot"></a>Blob-pillanatkép létrehozása

Pillanatkép egy blobot, amely egy adott időben csak olvasható verziója telepítve. A pillanatképek biztonsági mentési bináris objektumok hasznosak. Pillanatkép létrehozása után olvassa el, másolja, vagy törölje azt, de nem módosíthatja.

Blob pillanatképe azonos tooits alap blob, kivéve, hogy a hello blob URI-Azonosítójához tartozik egy **DateTime** érték toohello blob URI tooindicate hello idő, mely hello során létrehozott pillanatképen lesz hozzáfűzve. Például ha egy lap blob URI van `http://storagesample.core.blob.windows.net/mydrives/myvhd`, hello pillanatkép URI túl hasonló`http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.

> [!NOTE]
> Minden pillanatkép megosztása hello base blob URI. csak alapszintű hello blob és hello pillanatkép megkülönböztetése fűzött hello hello **DateTime** érték.
>

Blob állhat tetszőleges számú pillanatkép. A pillanatképek továbbra is fennáll, addig, amíg explicit módon törlődnek. A pillanatkép nem outlive az alap blob. Enumerálása hello alap blob tootrack tartozó hello pillanatképet az aktuális pillanatképeket.

Pillanatkép készítése a blob létrehozásakor hello blob tulajdonságai vannak hello másolt toohello pillanatfelvételt ugyanazokat az értékeket. hello alap blob metaadatai is másolt toohello pillanatképet, ha nem ad meg, hogy a pillanatkép-létrehozás során hello külön metaadatok.

Bármely hello alap blob társított bérleteket nem befolyásolják a hello pillanatkép. Nem olvasható be a pillanatkép bérleti idejét.

A VHD-fájl használt toostore hello aktuális adatait és állapotát egy Virtuálisgép-lemez. A lemezt leválasztani a virtuális gép hello belül vagy hello VM leállítása, és majd pillanatkép készítése a VHD-fájlt. Is használhatja, hogy a pillanatkép-fájl újabb tooretrieve hello VHD ezen a ponton idő fájlt, és hozza létre újra a virtuális gép hello.

Ha engedélyezve van a Storage szolgáltatás titkosítási (SSE) hello tárfiók mely hello a blob helyezkedik el, majd venni, hogy a blob meglévő pillanatképeket lesz titkosítva, aktívan.

## <a name="create-a-snapshot"></a>Pillanatkép létrehozása
hello következő kódrészlet példa bemutatja, hogyan toocreate használatával pillanatkép hello [Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). Ebben a példában létrehozásakor adja meg a további metaadatokat hello pillanatkép.

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in hello container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload hello blob toocreate it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of hello base blob.
        // Specify metadata at hello time that hello snapshot is created toospecify unique metadata for hello snapshot.
        // If no metadata is specified when hello snapshot is created, hello base blob's metadata is copied toohello snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="copy-snapshots"></a>A pillanatképek másolása
Blobok és a pillanatképek másolási műveletek kövesse ezeket a szabályokat:

* Az alap blob keresztül pillanatkép másolhatja. Hello alap blob pillanatkép toohello pozíció támogatásával, visszaállíthatja egy korábbi blob. hello pillanatkép marad, de hello alap blob hello pillanatkép írható másolatot felülírja.
* Más néven pillanatkép tooa cél blob másolhatja. hello eredményül kapott cél blob írható blob és a pillanatkép nem.
* Forrás blob másolását követően bármely hello forrás BLOB pillanatképei nem másolt toohello cél. Amikor másolatot felülírja a cél blob, meglévő pillanatképeket hello eredeti cél blob társított változatlanok maradnak.
* Pillanatkép készítése a blokkblob létrehozásakor hello blob véglegesített tiltólista egyben másolt toohello pillanatkép. A nem véglegesített téglalapok nem kerülnek.

## <a name="specify-an-access-condition"></a>Adjon meg egy hozzáférési állapot
A hívás esetén [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], megadhatja a hozzáférés egy feltétele, hogy hello pillanatkép jön létre, csak ha egy feltétel teljesül. hozzáférés egy feltétele toospecify hello használata [AccessCondition] [ dotnet_AccessCondition] paraméter. Ha hello megadott feltétel nem teljesül, a hello pillanatkép nem jön létre, és a Blob szolgáltatás hello adja vissza. állapotkód: [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.

## <a name="delete-snapshots"></a>Törölje a pillanatképeket
Egy blobot a pillanatképek nem törölhető, kivéve, ha hello pillanatképek is törlődnek. Pillanatkép törlése egyenként, vagy adja meg, hogy minden pillanatképeket törölni hello forrás blob törlésekor. Ha továbbra is pillanatképekkel blob toodelete kísérli meg, hibát okoz.

a következő kód példa azt mutatja meg hogyan hello toodelete blob és a pillanatképeket a .NET, ahol `blockBlob` típusú objektum [CloudBlockBlob][dotnet_CloudBlockBlob]:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a>Prémium szintű Azure Storage pillanatképei
Ha készített pillanatfelvételek segítségével történő prémium szintű Storage, hello a következő szabályok lépnek érvénybe:

* a prémium szintű tárfiók oldalakra vonatkozó blob / pillanatképek hello maximális száma érték 100. Ha túllépi ezt a határértéket, a hello pillanatkép Blob művelet hibakód 409 adja vissza (`SnapshotCountExceeded`).
* Akkor is készítsen pillanatképet oldalakra vonatkozó blob egy prémium szintű tárfiók a 10 percenként egyszer. Adott aránya túllépi a határértéket, hello pillanatkép Blob művelet vissza hibakód 409 (`SnapshotOperationRateExceeded`).
* pillanatkép tooread, hello Blob másolási művelet toocopy pillanatkép tooanother oldalakra vonatkozó blob használhatja hello fiók. hello cél blob hello másolási művelet nem lehet a meglévő pillanatképeket. Ha hello cél blob pillanatképekkel rendelkezik, akkor hello Blob másolási művelet hibakódot 409 adja vissza (`SnapshotsPresent`).

## <a name="return-hello-absolute-uri-tooa-snapshot"></a>Visszatérési hello abszolút URI tooa pillanatkép
A C# Kódpélda pillanatképet hoz létre, és írja a hello hello elsődleges hely abszolút URI.

```csharp
//Create hello blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference tooa blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of hello blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a>Hogyan pillanatképek keletkeznek költségek ismertetése
Egy pillanatképet, amely egy blob csak olvasható másolatát, létrehozása azt eredményezheti, hogy további adatok tárolási költségek tooyour fiók. Az alkalmazás tervezésekor fontos toobe tudomást hogyan keletkeznek a díjak előfordulhat, hogy, hogy minimalizálja a költségek is.

### <a name="important-billing-considerations"></a>Fontos számlázási szempontok
a következő lista hello tartalmazza a legfontosabb tooconsider pillanatkép létrehozásakor.

* A tárfiók terhel egyedi blokkok vagy lapok, hogy vannak-e hello blob vagy hello pillanatkép. A fiók nem többletköltségei társított blob, amíg nem frissíti az azok alapjául hello blob pillanatképek. Alapszintű hello blob frissítése után a pillanatképek eltér. Ez akkor fordul elő, ha van szó, a hello egyedi blokkok vagy minden egyes blob vagy a pillanatkép oldalán.
* A blokkolás a blokkblob belül cseréjekor, amelyek blokkolják ezt követően és egyedi blokk számítjuk fel. Ez igaz, akkor is, ha hello blokk azonos azonosító letiltása, és azonos hello hello adatok, mert rendelkezik-e hello pillanatkép. Hello blokk véglegesítése után újra, azt a megfelelő bármely pillanatfelvétel eltér, és a saját adataikhoz fizetnie kell. Ugyanez érvényes a lap az oldalakra vonatkozó blob azonos adatokkal rendelkező frissített hello.
* A mag cseréje a blokkblob hívó hello által [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream] [ dotnet_UploadFromStream], vagy [UploadFromByteArray] [ dotnet_UploadFromByteArray] metódus hello BLOB blokkok váltja fel. Ha, hogy a blob társított pillanatképet, hello alap blob és a pillanatkép blokkok most tér el, és mindkét blobok hello blokkok sávszélességért fizetnie kell. Ez igaz, akkor is, ha hello alap blob és hello pillanatkép hello adatai továbbra is azonos.
* hello Azure Blob szolgáltatás nem rendelkezik a azt jelenti, hogy toodetermine e két blokk azonos adatokat tartalmaz. Minden feltöltött és véglegesített blokk egyedi kezeli, akkor is, ha az ugyanazon adatokhoz és hello rendelkezik hello azonos blokkolása azonosítóját. Mivel a díjak egyedi blokk keletkeznek, fontos tooconsider, amely további egyedi blokkok és további díjakat eredményez, amely egy pillanatkép tartozik blob frissítése is.

### <a name="minimize-cost-with-snapshot-management"></a>Pillanatkép-kezeléssel költségek minimalizálása érdekében

Javasoljuk, hogy a pillanatképek gondosan kezelése tooavoid további díjakat. Kövesse az alábbi gyakorlati tanácsok toohelp minimalizálása érdekében a pillanatképek tárolását hello hello költségeit:

* Törölje, és hozza létre a pillanatképek társított blob frissítésénél hello blob, még akkor is, ha frissíti azonos adatokkal, kivéve, ha az alkalmazás tervhez szükséges, hogy a pillanatképek karbantartása. Törlésével és ismételt létrehozása a hello blob pillanatképeket, biztosíthatja, hogy hello blob és a pillanatfelvételeket tér el.
* Ha a blob-pillanatképek fenntartásának ne hívása [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], vagy [UploadFromByteArray] [ dotnet_UploadFromByteArray] tooupdate hello blob. Ezek a módszerek hello blob, a kiinduló blob és a pillanatképek toodiverge jelentősen okozó hello adatblokkok cseréjét. Ehelyett frissítés hello hello a lehető legkevesebb blokkok [PutBlock] [ dotnet_PutBlock] és [PutBlockList] [ dotnet_PutBlockList] módszerek.

### <a name="snapshot-billing-scenarios"></a>Pillanatkép számlázási forgatókönyvek
a következő forgatókönyvek hello bemutatják, hogyan költségek letiltása blob és a pillanatképek keletkeznek.

**1. forgatókönyv**

Az 1. forgatókönyv hello alap blob nem lett frissítve után hello pillanatkép készült, ezért díjak sem merülnek fel, csak az egyedi blokkok 1, 2 és 3.

![Azure Storage-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

**2. forgatókönyv**

A 2. forgatókönyv hello alap blob frissítve lett, de hello pillanatkép nem rendelkezik. Blokk 3 frissítve lett, és annak ellenére, hogy a benne található hello ugyanazokat az adatokat, és ugyanazzal az Azonosítóval hello, hogy nem hello ugyanaz, mint 3 hello pillanatfelvétel blokkolása. Ennek eredményeképpen hello fiók négy blokk számítjuk fel.

![Azure Storage-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

**3. forgatókönyv**

3. forgatókönyv hello alap blob frissítve lett, de nem rendelkezik hello pillanatkép. Blokk 3 váltotta fel a hello alap blob 4 blokkja, de hello pillanatkép továbbra is tükrözi blokk 3. Ennek eredményeképpen hello fiók négy blokk számítjuk fel.

![Azure Storage-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

**4. forgatókönyv**

A 4. forgatókönyv hello alap blob teljesen frissült, ezért az eredeti blokkok egyike sem tartalmazza. Ennek eredményeképpen hello fiók összes nyolc egyedi blokk számítjuk fel. Ez akkor fordulhat elő, ha egy frissítési módszerét használja, mint [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], vagy [UploadFromByteArray][dotnet_UploadFromByteArray], mert ezek a módszerek cseréjét hello a blob tartalmát.

![Azure Storage-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Következő lépések

* További információ a virtuális gép (VM) lemez pillanatképek használata található [készítsen biztonsági másolatot az Azure nem felügyelt méretű lemezek növekményes pillanatképek](storage-incremental-snapshots.md)

* Kód Blob storage használatával, tekintse meg a [Azure mintakódok](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Töltse le a mintaalkalmazást és futtatáshoz, vagy keresse meg a hello kódja a Githubon.

[dotnet_AccessCondition]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.accesscondition.aspx
[dotnet_CloudBlockBlob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx
[dotnet_CreateSnapshotAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.createsnapshotasync.aspx
[dotnet_HTTPStatusCode]: https://msdn.microsoft.com/library/system.net.httpstatuscode(v=vs.110).aspx
[dotnet_PutBlockList]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.putblocklist.aspx
[dotnet_PutBlock]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.putblock.aspx
[dotnet_UploadFromByteArray]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfrombytearray.aspx
[dotnet_UploadFromFile]: https://msdn.microsoft.com/library/azure/mt705654.aspx
[dotnet_UploadFromStream]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstream.aspx
[dotnet_UploadText]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadtext.aspx