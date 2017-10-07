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
# <a name="create-a-blob-snapshot"></a><span data-ttu-id="37aff-104">Blob-pillanatkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="37aff-104">Create a blob snapshot</span></span>

<span data-ttu-id="37aff-105">Pillanatkép egy blobot, amely egy adott időben csak olvasható verziója telepítve.</span><span class="sxs-lookup"><span data-stu-id="37aff-105">A snapshot is a read-only version of a blob that's taken at a point in time.</span></span> <span data-ttu-id="37aff-106">A pillanatképek biztonsági mentési bináris objektumok hasznosak.</span><span class="sxs-lookup"><span data-stu-id="37aff-106">Snapshots are useful for backing up blobs.</span></span> <span data-ttu-id="37aff-107">Pillanatkép létrehozása után olvassa el, másolja, vagy törölje azt, de nem módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="37aff-107">After you create a snapshot, you can read, copy, or delete it, but you cannot modify it.</span></span>

<span data-ttu-id="37aff-108">Blob pillanatképe azonos tooits alap blob, kivéve, hogy a hello blob URI-Azonosítójához tartozik egy **DateTime** érték toohello blob URI tooindicate hello idő, mely hello során létrehozott pillanatképen lesz hozzáfűzve.</span><span class="sxs-lookup"><span data-stu-id="37aff-108">A snapshot of a blob is identical tooits base blob, except that hello blob URI has a **DateTime** value appended toohello blob URI tooindicate hello time at which hello snapshot was taken.</span></span> <span data-ttu-id="37aff-109">Például ha egy lap blob URI van `http://storagesample.core.blob.windows.net/mydrives/myvhd`, hello pillanatkép URI túl hasonló`http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.</span><span class="sxs-lookup"><span data-stu-id="37aff-109">For example, if a page blob URI is `http://storagesample.core.blob.windows.net/mydrives/myvhd`, hello snapshot URI is similar too`http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.</span></span>

> [!NOTE]
> <span data-ttu-id="37aff-110">Minden pillanatkép megosztása hello base blob URI.</span><span class="sxs-lookup"><span data-stu-id="37aff-110">All snapshots share hello base blob's URI.</span></span> <span data-ttu-id="37aff-111">csak alapszintű hello blob és hello pillanatkép megkülönböztetése fűzött hello hello **DateTime** érték.</span><span class="sxs-lookup"><span data-stu-id="37aff-111">hello only distinction between hello base blob and hello snapshot is hello appended **DateTime** value.</span></span>
>

<span data-ttu-id="37aff-112">Blob állhat tetszőleges számú pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="37aff-112">A blob can have any number of snapshots.</span></span> <span data-ttu-id="37aff-113">A pillanatképek továbbra is fennáll, addig, amíg explicit módon törlődnek.</span><span class="sxs-lookup"><span data-stu-id="37aff-113">Snapshots persist until they are explicitly deleted.</span></span> <span data-ttu-id="37aff-114">A pillanatkép nem outlive az alap blob.</span><span class="sxs-lookup"><span data-stu-id="37aff-114">A snapshot cannot outlive its base blob.</span></span> <span data-ttu-id="37aff-115">Enumerálása hello alap blob tootrack tartozó hello pillanatképet az aktuális pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="37aff-115">You can enumerate hello snapshots associated with hello base blob tootrack your current snapshots.</span></span>

<span data-ttu-id="37aff-116">Pillanatkép készítése a blob létrehozásakor hello blob tulajdonságai vannak hello másolt toohello pillanatfelvételt ugyanazokat az értékeket.</span><span class="sxs-lookup"><span data-stu-id="37aff-116">When you create a snapshot of a blob, hello blob's system properties are copied toohello snapshot with hello same values.</span></span> <span data-ttu-id="37aff-117">hello alap blob metaadatai is másolt toohello pillanatképet, ha nem ad meg, hogy a pillanatkép-létrehozás során hello külön metaadatok.</span><span class="sxs-lookup"><span data-stu-id="37aff-117">hello base blob's metadata is also copied toohello snapshot, unless you specify separate metadata for hello snapshot when you create it.</span></span>

<span data-ttu-id="37aff-118">Bármely hello alap blob társított bérleteket nem befolyásolják a hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="37aff-118">Any leases associated with hello base blob do not affect hello snapshot.</span></span> <span data-ttu-id="37aff-119">Nem olvasható be a pillanatkép bérleti idejét.</span><span class="sxs-lookup"><span data-stu-id="37aff-119">You cannot acquire a lease on a snapshot.</span></span>

<span data-ttu-id="37aff-120">A VHD-fájl használt toostore hello aktuális adatait és állapotát egy Virtuálisgép-lemez.</span><span class="sxs-lookup"><span data-stu-id="37aff-120">A VHD file is used toostore hello current information and status for a VM disk.</span></span> <span data-ttu-id="37aff-121">A lemezt leválasztani a virtuális gép hello belül vagy hello VM leállítása, és majd pillanatkép készítése a VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="37aff-121">You can detach a disk from within hello VM or shut down hello VM, and then take a snapshot of its VHD file.</span></span> <span data-ttu-id="37aff-122">Is használhatja, hogy a pillanatkép-fájl újabb tooretrieve hello VHD ezen a ponton idő fájlt, és hozza létre újra a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="37aff-122">You can use that snapshot file later tooretrieve hello VHD file at that point in time and recreate hello VM.</span></span>

<span data-ttu-id="37aff-123">Ha engedélyezve van a Storage szolgáltatás titkosítási (SSE) hello tárfiók mely hello a blob helyezkedik el, majd venni, hogy a blob meglévő pillanatképeket lesz titkosítva, aktívan.</span><span class="sxs-lookup"><span data-stu-id="37aff-123">If Storage Service Encryption (SSE) is enabled for hello storage account in which hello blob resides, then any snapshots taken of that blob will be encrypted at rest.</span></span>

## <a name="create-a-snapshot"></a><span data-ttu-id="37aff-124">Pillanatkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="37aff-124">Create a snapshot</span></span>
<span data-ttu-id="37aff-125">hello következő kódrészlet példa bemutatja, hogyan toocreate használatával pillanatkép hello [Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span><span class="sxs-lookup"><span data-stu-id="37aff-125">hello following code example shows how toocreate a snapshot by using hello [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span></span> <span data-ttu-id="37aff-126">Ebben a példában létrehozásakor adja meg a további metaadatokat hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="37aff-126">This example specifies additional metadata for hello snapshot when it is created.</span></span>

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

## <a name="copy-snapshots"></a><span data-ttu-id="37aff-127">A pillanatképek másolása</span><span class="sxs-lookup"><span data-stu-id="37aff-127">Copy snapshots</span></span>
<span data-ttu-id="37aff-128">Blobok és a pillanatképek másolási műveletek kövesse ezeket a szabályokat:</span><span class="sxs-lookup"><span data-stu-id="37aff-128">Copy operations involving blobs and snapshots follow these rules:</span></span>

* <span data-ttu-id="37aff-129">Az alap blob keresztül pillanatkép másolhatja.</span><span class="sxs-lookup"><span data-stu-id="37aff-129">You can copy a snapshot over its base blob.</span></span> <span data-ttu-id="37aff-130">Hello alap blob pillanatkép toohello pozíció támogatásával, visszaállíthatja egy korábbi blob.</span><span class="sxs-lookup"><span data-stu-id="37aff-130">By promoting a snapshot toohello position of hello base blob, you can restore an earlier version of a blob.</span></span> <span data-ttu-id="37aff-131">hello pillanatkép marad, de hello alap blob hello pillanatkép írható másolatot felülírja.</span><span class="sxs-lookup"><span data-stu-id="37aff-131">hello snapshot remains, but hello base blob is overwritten with a writable copy of hello snapshot.</span></span>
* <span data-ttu-id="37aff-132">Más néven pillanatkép tooa cél blob másolhatja.</span><span class="sxs-lookup"><span data-stu-id="37aff-132">You can copy a snapshot tooa destination blob with a different name.</span></span> <span data-ttu-id="37aff-133">hello eredményül kapott cél blob írható blob és a pillanatkép nem.</span><span class="sxs-lookup"><span data-stu-id="37aff-133">hello resulting destination blob is a writable blob and not a snapshot.</span></span>
* <span data-ttu-id="37aff-134">Forrás blob másolását követően bármely hello forrás BLOB pillanatképei nem másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="37aff-134">When a source blob is copied, any snapshots of hello source blob are not copied toohello destination.</span></span> <span data-ttu-id="37aff-135">Amikor másolatot felülírja a cél blob, meglévő pillanatképeket hello eredeti cél blob társított változatlanok maradnak.</span><span class="sxs-lookup"><span data-stu-id="37aff-135">When a destination blob is overwritten with a copy, any snapshots associated with hello original destination blob remain intact.</span></span>
* <span data-ttu-id="37aff-136">Pillanatkép készítése a blokkblob létrehozásakor hello blob véglegesített tiltólista egyben másolt toohello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="37aff-136">When you create a snapshot of a block blob, hello blob's committed block list is also copied toohello snapshot.</span></span> <span data-ttu-id="37aff-137">A nem véglegesített téglalapok nem kerülnek.</span><span class="sxs-lookup"><span data-stu-id="37aff-137">Any uncommitted blocks are not copied.</span></span>

## <a name="specify-an-access-condition"></a><span data-ttu-id="37aff-138">Adjon meg egy hozzáférési állapot</span><span class="sxs-lookup"><span data-stu-id="37aff-138">Specify an access condition</span></span>
<span data-ttu-id="37aff-139">A hívás esetén [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], megadhatja a hozzáférés egy feltétele, hogy hello pillanatkép jön létre, csak ha egy feltétel teljesül.</span><span class="sxs-lookup"><span data-stu-id="37aff-139">When you call [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], you can specify an access condition so that hello snapshot is created only if a condition is met.</span></span> <span data-ttu-id="37aff-140">hozzáférés egy feltétele toospecify hello használata [AccessCondition] [ dotnet_AccessCondition] paraméter.</span><span class="sxs-lookup"><span data-stu-id="37aff-140">toospecify an access condition, use hello [AccessCondition][dotnet_AccessCondition] parameter.</span></span> <span data-ttu-id="37aff-141">Ha hello megadott feltétel nem teljesül, a hello pillanatkép nem jön létre, és a Blob szolgáltatás hello adja vissza. állapotkód: [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.</span><span class="sxs-lookup"><span data-stu-id="37aff-141">If hello specified condition is not met, hello snapshot is not created, and hello Blob service returns status code [HTTPStatusCode][dotnet_HTTPStatusCode].PreconditionFailed.</span></span>

## <a name="delete-snapshots"></a><span data-ttu-id="37aff-142">Törölje a pillanatképeket</span><span class="sxs-lookup"><span data-stu-id="37aff-142">Delete snapshots</span></span>
<span data-ttu-id="37aff-143">Egy blobot a pillanatképek nem törölhető, kivéve, ha hello pillanatképek is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="37aff-143">You can't delete a blob with snapshots unless hello snapshots are also deleted.</span></span> <span data-ttu-id="37aff-144">Pillanatkép törlése egyenként, vagy adja meg, hogy minden pillanatképeket törölni hello forrás blob törlésekor.</span><span class="sxs-lookup"><span data-stu-id="37aff-144">You can delete a snapshot individually, or specify that all snapshots be deleted when hello source blob is deleted.</span></span> <span data-ttu-id="37aff-145">Ha továbbra is pillanatképekkel blob toodelete kísérli meg, hibát okoz.</span><span class="sxs-lookup"><span data-stu-id="37aff-145">If you attempt toodelete a blob that still has snapshots, an error results.</span></span>

<span data-ttu-id="37aff-146">a következő kód példa azt mutatja meg hogyan hello toodelete blob és a pillanatképeket a .NET, ahol `blockBlob` típusú objektum [CloudBlockBlob][dotnet_CloudBlockBlob]:</span><span class="sxs-lookup"><span data-stu-id="37aff-146">hello following code example shows how toodelete a blob and its snapshots in .NET, where `blockBlob` is an object of type [CloudBlockBlob][dotnet_CloudBlockBlob]:</span></span>

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a><span data-ttu-id="37aff-147">Prémium szintű Azure Storage pillanatképei</span><span class="sxs-lookup"><span data-stu-id="37aff-147">Snapshots with Azure Premium Storage</span></span>
<span data-ttu-id="37aff-148">Ha készített pillanatfelvételek segítségével történő prémium szintű Storage, hello a következő szabályok lépnek érvénybe:</span><span class="sxs-lookup"><span data-stu-id="37aff-148">When using snapshots with Premium Storage, hello following rules apply:</span></span>

* <span data-ttu-id="37aff-149">a prémium szintű tárfiók oldalakra vonatkozó blob / pillanatképek hello maximális száma érték 100.</span><span class="sxs-lookup"><span data-stu-id="37aff-149">hello maximum number of snapshots per page blob in a premium storage account is 100.</span></span> <span data-ttu-id="37aff-150">Ha túllépi ezt a határértéket, a hello pillanatkép Blob művelet hibakód 409 adja vissza (`SnapshotCountExceeded`).</span><span class="sxs-lookup"><span data-stu-id="37aff-150">If that limit is exceeded, hello Snapshot Blob operation returns error code 409 (`SnapshotCountExceeded`).</span></span>
* <span data-ttu-id="37aff-151">Akkor is készítsen pillanatképet oldalakra vonatkozó blob egy prémium szintű tárfiók a 10 percenként egyszer.</span><span class="sxs-lookup"><span data-stu-id="37aff-151">You can take a snapshot of a page blob in a premium storage account once every 10 minutes.</span></span> <span data-ttu-id="37aff-152">Adott aránya túllépi a határértéket, hello pillanatkép Blob művelet vissza hibakód 409 (`SnapshotOperationRateExceeded`).</span><span class="sxs-lookup"><span data-stu-id="37aff-152">If that rate is exceeded, hello Snapshot Blob operation returns error code 409 (`SnapshotOperationRateExceeded`).</span></span>
* <span data-ttu-id="37aff-153">pillanatkép tooread, hello Blob másolási művelet toocopy pillanatkép tooanother oldalakra vonatkozó blob használhatja hello fiók.</span><span class="sxs-lookup"><span data-stu-id="37aff-153">tooread a snapshot, you can use hello Copy Blob operation toocopy a snapshot tooanother page blob in hello account.</span></span> <span data-ttu-id="37aff-154">hello cél blob hello másolási művelet nem lehet a meglévő pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="37aff-154">hello destination blob for hello copy operation must not have any existing snapshots.</span></span> <span data-ttu-id="37aff-155">Ha hello cél blob pillanatképekkel rendelkezik, akkor hello Blob másolási művelet hibakódot 409 adja vissza (`SnapshotsPresent`).</span><span class="sxs-lookup"><span data-stu-id="37aff-155">If hello destination blob does have snapshots, then hello Copy Blob operation returns error code 409 (`SnapshotsPresent`).</span></span>

## <a name="return-hello-absolute-uri-tooa-snapshot"></a><span data-ttu-id="37aff-156">Visszatérési hello abszolút URI tooa pillanatkép</span><span class="sxs-lookup"><span data-stu-id="37aff-156">Return hello absolute URI tooa snapshot</span></span>
<span data-ttu-id="37aff-157">A C# Kódpélda pillanatképet hoz létre, és írja a hello hello elsődleges hely abszolút URI.</span><span class="sxs-lookup"><span data-stu-id="37aff-157">This C# code example creates a snapshot and writes out hello absolute URI for hello primary location.</span></span>

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

## <a name="understand-how-snapshots-accrue-charges"></a><span data-ttu-id="37aff-158">Hogyan pillanatképek keletkeznek költségek ismertetése</span><span class="sxs-lookup"><span data-stu-id="37aff-158">Understand how snapshots accrue charges</span></span>
<span data-ttu-id="37aff-159">Egy pillanatképet, amely egy blob csak olvasható másolatát, létrehozása azt eredményezheti, hogy további adatok tárolási költségek tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="37aff-159">Creating a snapshot, which is a read-only copy of a blob, can result in additional data storage charges tooyour account.</span></span> <span data-ttu-id="37aff-160">Az alkalmazás tervezésekor fontos toobe tudomást hogyan keletkeznek a díjak előfordulhat, hogy, hogy minimalizálja a költségek is.</span><span class="sxs-lookup"><span data-stu-id="37aff-160">When designing your application, it is important toobe aware of how these charges might accrue so that you can minimize costs.</span></span>

### <a name="important-billing-considerations"></a><span data-ttu-id="37aff-161">Fontos számlázási szempontok</span><span class="sxs-lookup"><span data-stu-id="37aff-161">Important billing considerations</span></span>
<span data-ttu-id="37aff-162">a következő lista hello tartalmazza a legfontosabb tooconsider pillanatkép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="37aff-162">hello following list includes key points tooconsider when creating a snapshot.</span></span>

* <span data-ttu-id="37aff-163">A tárfiók terhel egyedi blokkok vagy lapok, hogy vannak-e hello blob vagy hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="37aff-163">Your storage account incurs charges for unique blocks or pages, whether they are in hello blob or in hello snapshot.</span></span> <span data-ttu-id="37aff-164">A fiók nem többletköltségei társított blob, amíg nem frissíti az azok alapjául hello blob pillanatképek.</span><span class="sxs-lookup"><span data-stu-id="37aff-164">Your account does not incur additional charges for snapshots associated with a blob until you update hello blob on which they are based.</span></span> <span data-ttu-id="37aff-165">Alapszintű hello blob frissítése után a pillanatképek eltér.</span><span class="sxs-lookup"><span data-stu-id="37aff-165">After you update hello base blob, it diverges from its snapshots.</span></span> <span data-ttu-id="37aff-166">Ez akkor fordul elő, ha van szó, a hello egyedi blokkok vagy minden egyes blob vagy a pillanatkép oldalán.</span><span class="sxs-lookup"><span data-stu-id="37aff-166">When this happens, you are charged for hello unique blocks or pages in each blob or snapshot.</span></span>
* <span data-ttu-id="37aff-167">A blokkolás a blokkblob belül cseréjekor, amelyek blokkolják ezt követően és egyedi blokk számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="37aff-167">When you replace a block within a block blob, that block is subsequently charged as a unique block.</span></span> <span data-ttu-id="37aff-168">Ez igaz, akkor is, ha hello blokk azonos azonosító letiltása, és azonos hello hello adatok, mert rendelkezik-e hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="37aff-168">This is true even if hello block has hello same block ID and hello same data as it has in hello snapshot.</span></span> <span data-ttu-id="37aff-169">Hello blokk véglegesítése után újra, azt a megfelelő bármely pillanatfelvétel eltér, és a saját adataikhoz fizetnie kell.</span><span class="sxs-lookup"><span data-stu-id="37aff-169">After hello block is committed again, it diverges from its counterpart in any snapshot, and you will be charged for its data.</span></span> <span data-ttu-id="37aff-170">Ugyanez érvényes a lap az oldalakra vonatkozó blob azonos adatokkal rendelkező frissített hello.</span><span class="sxs-lookup"><span data-stu-id="37aff-170">hello same holds true for a page in a page blob that's updated with identical data.</span></span>
* <span data-ttu-id="37aff-171">A mag cseréje a blokkblob hívó hello által [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream] [ dotnet_UploadFromStream], vagy [UploadFromByteArray] [ dotnet_UploadFromByteArray] metódus hello BLOB blokkok váltja fel.</span><span class="sxs-lookup"><span data-stu-id="37aff-171">Replacing a block blob by calling hello [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray] method replaces all blocks in hello blob.</span></span> <span data-ttu-id="37aff-172">Ha, hogy a blob társított pillanatképet, hello alap blob és a pillanatkép blokkok most tér el, és mindkét blobok hello blokkok sávszélességért fizetnie kell.</span><span class="sxs-lookup"><span data-stu-id="37aff-172">If you have a snapshot associated with that blob, all blocks in hello base blob and snapshot now diverge, and you will be charged for all hello blocks in both blobs.</span></span> <span data-ttu-id="37aff-173">Ez igaz, akkor is, ha hello alap blob és hello pillanatkép hello adatai továbbra is azonos.</span><span class="sxs-lookup"><span data-stu-id="37aff-173">This is true even if hello data in hello base blob and hello snapshot remain identical.</span></span>
* <span data-ttu-id="37aff-174">hello Azure Blob szolgáltatás nem rendelkezik a azt jelenti, hogy toodetermine e két blokk azonos adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="37aff-174">hello Azure Blob service does not have a means toodetermine whether two blocks contain identical data.</span></span> <span data-ttu-id="37aff-175">Minden feltöltött és véglegesített blokk egyedi kezeli, akkor is, ha az ugyanazon adatokhoz és hello rendelkezik hello azonos blokkolása azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="37aff-175">Each block that is uploaded and committed is treated as unique, even if it has hello same data and hello same block ID.</span></span> <span data-ttu-id="37aff-176">Mivel a díjak egyedi blokk keletkeznek, fontos tooconsider, amely további egyedi blokkok és további díjakat eredményez, amely egy pillanatkép tartozik blob frissítése is.</span><span class="sxs-lookup"><span data-stu-id="37aff-176">Because charges accrue for unique blocks, it's important tooconsider that updating a blob that has a snapshot results in additional unique blocks and additional charges.</span></span>

### <a name="minimize-cost-with-snapshot-management"></a><span data-ttu-id="37aff-177">Pillanatkép-kezeléssel költségek minimalizálása érdekében</span><span class="sxs-lookup"><span data-stu-id="37aff-177">Minimize cost with snapshot management</span></span>

<span data-ttu-id="37aff-178">Javasoljuk, hogy a pillanatképek gondosan kezelése tooavoid további díjakat.</span><span class="sxs-lookup"><span data-stu-id="37aff-178">We recommend managing your snapshots carefully tooavoid extra charges.</span></span> <span data-ttu-id="37aff-179">Kövesse az alábbi gyakorlati tanácsok toohelp minimalizálása érdekében a pillanatképek tárolását hello hello költségeit:</span><span class="sxs-lookup"><span data-stu-id="37aff-179">You can follow these best practices toohelp minimize hello costs incurred by hello storage of your snapshots:</span></span>

* <span data-ttu-id="37aff-180">Törölje, és hozza létre a pillanatképek társított blob frissítésénél hello blob, még akkor is, ha frissíti azonos adatokkal, kivéve, ha az alkalmazás tervhez szükséges, hogy a pillanatképek karbantartása.</span><span class="sxs-lookup"><span data-stu-id="37aff-180">Delete and re-create snapshots associated with a blob whenever you update hello blob, even if you are updating with identical data, unless your application design requires that you maintain snapshots.</span></span> <span data-ttu-id="37aff-181">Törlésével és ismételt létrehozása a hello blob pillanatképeket, biztosíthatja, hogy hello blob és a pillanatfelvételeket tér el.</span><span class="sxs-lookup"><span data-stu-id="37aff-181">By deleting and re-creating hello blob's snapshots, you can ensure that hello blob and snapshots do not diverge.</span></span>
* <span data-ttu-id="37aff-182">Ha a blob-pillanatképek fenntartásának ne hívása [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], vagy [UploadFromByteArray] [ dotnet_UploadFromByteArray] tooupdate hello blob.</span><span class="sxs-lookup"><span data-stu-id="37aff-182">If you are maintaining snapshots for a blob, avoid calling [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray] tooupdate hello blob.</span></span> <span data-ttu-id="37aff-183">Ezek a módszerek hello blob, a kiinduló blob és a pillanatképek toodiverge jelentősen okozó hello adatblokkok cseréjét.</span><span class="sxs-lookup"><span data-stu-id="37aff-183">These methods replace all of hello blocks in hello blob, causing your base blob and its snapshots toodiverge significantly.</span></span> <span data-ttu-id="37aff-184">Ehelyett frissítés hello hello a lehető legkevesebb blokkok [PutBlock] [ dotnet_PutBlock] és [PutBlockList] [ dotnet_PutBlockList] módszerek.</span><span class="sxs-lookup"><span data-stu-id="37aff-184">Instead, update hello fewest possible number of blocks by using hello [PutBlock][dotnet_PutBlock] and [PutBlockList][dotnet_PutBlockList] methods.</span></span>

### <a name="snapshot-billing-scenarios"></a><span data-ttu-id="37aff-185">Pillanatkép számlázási forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="37aff-185">Snapshot billing scenarios</span></span>
<span data-ttu-id="37aff-186">a következő forgatókönyvek hello bemutatják, hogyan költségek letiltása blob és a pillanatképek keletkeznek.</span><span class="sxs-lookup"><span data-stu-id="37aff-186">hello following scenarios demonstrate how charges accrue for a block blob and its snapshots.</span></span>

<span data-ttu-id="37aff-187">**1. forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="37aff-187">**Scenario 1**</span></span>

<span data-ttu-id="37aff-188">Az 1. forgatókönyv hello alap blob nem lett frissítve után hello pillanatkép készült, ezért díjak sem merülnek fel, csak az egyedi blokkok 1, 2 és 3.</span><span class="sxs-lookup"><span data-stu-id="37aff-188">In scenario 1, hello base blob has not been updated after hello snapshot was taken, so charges are incurred only for unique blocks 1, 2, and 3.</span></span>

![Azure Storage-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

<span data-ttu-id="37aff-190">**2. forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="37aff-190">**Scenario 2**</span></span>

<span data-ttu-id="37aff-191">A 2. forgatókönyv hello alap blob frissítve lett, de hello pillanatkép nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="37aff-191">In scenario 2, hello base blob has been updated, but hello snapshot has not.</span></span> <span data-ttu-id="37aff-192">Blokk 3 frissítve lett, és annak ellenére, hogy a benne található hello ugyanazokat az adatokat, és ugyanazzal az Azonosítóval hello, hogy nem hello ugyanaz, mint 3 hello pillanatfelvétel blokkolása.</span><span class="sxs-lookup"><span data-stu-id="37aff-192">Block 3 was updated, and even though it contains hello same data and hello same ID, it is not hello same as block 3 in hello snapshot.</span></span> <span data-ttu-id="37aff-193">Ennek eredményeképpen hello fiók négy blokk számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="37aff-193">As a result, hello account is charged for four blocks.</span></span>

![Azure Storage-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

<span data-ttu-id="37aff-195">**3. forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="37aff-195">**Scenario 3**</span></span>

<span data-ttu-id="37aff-196">3. forgatókönyv hello alap blob frissítve lett, de nem rendelkezik hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="37aff-196">In scenario 3, hello base blob has been updated, but hello snapshot has not.</span></span> <span data-ttu-id="37aff-197">Blokk 3 váltotta fel a hello alap blob 4 blokkja, de hello pillanatkép továbbra is tükrözi blokk 3.</span><span class="sxs-lookup"><span data-stu-id="37aff-197">Block 3 was replaced with block 4 in hello base blob, but hello snapshot still reflects block 3.</span></span> <span data-ttu-id="37aff-198">Ennek eredményeképpen hello fiók négy blokk számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="37aff-198">As a result, hello account is charged for four blocks.</span></span>

![Azure Storage-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

<span data-ttu-id="37aff-200">**4. forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="37aff-200">**Scenario 4**</span></span>

<span data-ttu-id="37aff-201">A 4. forgatókönyv hello alap blob teljesen frissült, ezért az eredeti blokkok egyike sem tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="37aff-201">In scenario 4, hello base blob has been completely updated and contains none of its original blocks.</span></span> <span data-ttu-id="37aff-202">Ennek eredményeképpen hello fiók összes nyolc egyedi blokk számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="37aff-202">As a result, hello account is charged for all eight unique blocks.</span></span> <span data-ttu-id="37aff-203">Ez akkor fordulhat elő, ha egy frissítési módszerét használja, mint [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], vagy [UploadFromByteArray][dotnet_UploadFromByteArray], mert ezek a módszerek cseréjét hello a blob tartalmát.</span><span class="sxs-lookup"><span data-stu-id="37aff-203">This scenario can occur if you are using an update method such as [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray], because these methods replace all of hello contents of a blob.</span></span>

![Azure Storage-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a><span data-ttu-id="37aff-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="37aff-205">Next steps</span></span>

* <span data-ttu-id="37aff-206">További információ a virtuális gép (VM) lemez pillanatképek használata található [készítsen biztonsági másolatot az Azure nem felügyelt méretű lemezek növekményes pillanatképek](storage-incremental-snapshots.md)</span><span class="sxs-lookup"><span data-stu-id="37aff-206">You can find more information about working with virtual machine (VM) disk snapshots in [Back up Azure unmanaged VM disks with incremental snapshots](storage-incremental-snapshots.md)</span></span>

* <span data-ttu-id="37aff-207">Kód Blob storage használatával, tekintse meg a [Azure mintakódok](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob).</span><span class="sxs-lookup"><span data-stu-id="37aff-207">For additional code examples using Blob storage, see [Azure Code Samples](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob).</span></span> <span data-ttu-id="37aff-208">Töltse le a mintaalkalmazást és futtatáshoz, vagy keresse meg a hello kódja a Githubon.</span><span class="sxs-lookup"><span data-stu-id="37aff-208">You can download a sample application and run it, or browse hello code on GitHub.</span></span>

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