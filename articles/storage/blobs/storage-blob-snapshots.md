---
title: "Csak olvasható pillanatkép létrehozása a blob az Azure Storage |} Microsoft Docs"
description: "Útmutató: a pillanatkép létrehozása a blob adatainak biztonsági mentése blob idő az adott pillanatban. Ismerje meg, hogyan számlázása a pillanatképek és a használatukat kapacitás költségek minimalizálása érdekében."
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
ms.openlocfilehash: b1d87cd66457b08bba594bfc7de1e9e4e2dff1e6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-blob-snapshot"></a><span data-ttu-id="3d499-104">Blob-pillanatkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d499-104">Create a blob snapshot</span></span>

<span data-ttu-id="3d499-105">Pillanatkép egy blobot, amely egy adott időben csak olvasható verziója telepítve.</span><span class="sxs-lookup"><span data-stu-id="3d499-105">A snapshot is a read-only version of a blob that's taken at a point in time.</span></span> <span data-ttu-id="3d499-106">A pillanatképek biztonsági mentési bináris objektumok hasznosak.</span><span class="sxs-lookup"><span data-stu-id="3d499-106">Snapshots are useful for backing up blobs.</span></span> <span data-ttu-id="3d499-107">Pillanatkép létrehozása után olvassa el, másolja, vagy törölje azt, de nem módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="3d499-107">After you create a snapshot, you can read, copy, or delete it, but you cannot modify it.</span></span>

<span data-ttu-id="3d499-108">Egy blob pillanatképe megegyezik az alap blob, azzal a különbséggel, hogy a blob URI egy **DateTime** érték hozzáfűzi a blob URI-t, amellyel során létrehozott pillanatképen időpontját jelzi.</span><span class="sxs-lookup"><span data-stu-id="3d499-108">A snapshot of a blob is identical to its base blob, except that the blob URI has a **DateTime** value appended to the blob URI to indicate the time at which the snapshot was taken.</span></span> <span data-ttu-id="3d499-109">Például ha egy lap blob URI van `http://storagesample.core.blob.windows.net/mydrives/myvhd`, a pillanatkép URI hasonlít `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.</span><span class="sxs-lookup"><span data-stu-id="3d499-109">For example, if a page blob URI is `http://storagesample.core.blob.windows.net/mydrives/myvhd`, the snapshot URI is similar to `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.</span></span>

> [!NOTE]
> <span data-ttu-id="3d499-110">Minden pillanatkép megosztása a base blob URI.</span><span class="sxs-lookup"><span data-stu-id="3d499-110">All snapshots share the base blob's URI.</span></span> <span data-ttu-id="3d499-111">A kiinduló blob és a pillanatkép csak megkülönböztetése a hozzáfűzött **DateTime** érték.</span><span class="sxs-lookup"><span data-stu-id="3d499-111">The only distinction between the base blob and the snapshot is the appended **DateTime** value.</span></span>
>

<span data-ttu-id="3d499-112">Blob állhat tetszőleges számú pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="3d499-112">A blob can have any number of snapshots.</span></span> <span data-ttu-id="3d499-113">A pillanatképek továbbra is fennáll, addig, amíg explicit módon törlődnek.</span><span class="sxs-lookup"><span data-stu-id="3d499-113">Snapshots persist until they are explicitly deleted.</span></span> <span data-ttu-id="3d499-114">A pillanatkép nem outlive az alap blob.</span><span class="sxs-lookup"><span data-stu-id="3d499-114">A snapshot cannot outlive its base blob.</span></span> <span data-ttu-id="3d499-115">A pillanatképek nyomon követéséhez a jelenlegi pillanatképek alap blob társított enumerálása.</span><span class="sxs-lookup"><span data-stu-id="3d499-115">You can enumerate the snapshots associated with the base blob to track your current snapshots.</span></span>

<span data-ttu-id="3d499-116">Amikor létrehoz egy blob pillanatképet, a blob tulajdonságai ugyanazokat az értékeket a pillanatkép lesz másolva.</span><span class="sxs-lookup"><span data-stu-id="3d499-116">When you create a snapshot of a blob, the blob's system properties are copied to the snapshot with the same values.</span></span> <span data-ttu-id="3d499-117">A kiinduló blob metaadatait is másolja a pillanatképet, ha nem ad meg, a pillanatkép külön metaadatok létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="3d499-117">The base blob's metadata is also copied to the snapshot, unless you specify separate metadata for the snapshot when you create it.</span></span>

<span data-ttu-id="3d499-118">Az alap blob társított bármely címbérleteket nem befolyásolják a pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="3d499-118">Any leases associated with the base blob do not affect the snapshot.</span></span> <span data-ttu-id="3d499-119">Nem olvasható be a pillanatkép bérleti idejét.</span><span class="sxs-lookup"><span data-stu-id="3d499-119">You cannot acquire a lease on a snapshot.</span></span>

<span data-ttu-id="3d499-120">A VHD-fájl az aktuális adatait és a Virtuálisgép-lemez állapota tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="3d499-120">A VHD file is used to store the current information and status for a VM disk.</span></span> <span data-ttu-id="3d499-121">Válassza le a lemezt a virtuális Gépen belül, vagy állítsa le a virtuális gép, és majd pillanatkép készítése a VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="3d499-121">You can detach a disk from within the VM or shut down the VM, and then take a snapshot of its VHD file.</span></span> <span data-ttu-id="3d499-122">A pillanatkép-fájlt később használhatja kérhető le a VHD-fájl ezen a ponton az időben, és hozza létre újra a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="3d499-122">You can use that snapshot file later to retrieve the VHD file at that point in time and recreate the VM.</span></span>

<span data-ttu-id="3d499-123">Storage Service Encryption (SSE) engedélyezve van a tárfiók a blob helyezkedik el, ha majd venni, hogy a blob meglévő pillanatképeket lesz titkosítva, aktívan.</span><span class="sxs-lookup"><span data-stu-id="3d499-123">If Storage Service Encryption (SSE) is enabled for the storage account in which the blob resides, then any snapshots taken of that blob will be encrypted at rest.</span></span>

## <a name="create-a-snapshot"></a><span data-ttu-id="3d499-124">Pillanatkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d499-124">Create a snapshot</span></span>
<span data-ttu-id="3d499-125">Az alábbi példakód bemutatja, hogyan hozzon létre egy pillanatképet a [Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span><span class="sxs-lookup"><span data-stu-id="3d499-125">The following code example shows how to create a snapshot by using the [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span></span> <span data-ttu-id="3d499-126">Ebben a példában adja meg a pillanatkép metaadatokat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="3d499-126">This example specifies additional metadata for the snapshot when it is created.</span></span>

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
        // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
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

## <a name="copy-snapshots"></a><span data-ttu-id="3d499-127">A pillanatképek másolása</span><span class="sxs-lookup"><span data-stu-id="3d499-127">Copy snapshots</span></span>
<span data-ttu-id="3d499-128">Blobok és a pillanatképek másolási műveletek kövesse ezeket a szabályokat:</span><span class="sxs-lookup"><span data-stu-id="3d499-128">Copy operations involving blobs and snapshots follow these rules:</span></span>

* <span data-ttu-id="3d499-129">Az alap blob keresztül pillanatkép másolhatja.</span><span class="sxs-lookup"><span data-stu-id="3d499-129">You can copy a snapshot over its base blob.</span></span> <span data-ttu-id="3d499-130">A kiinduló blob pozícióját az aktuális támogatásával, egy blobot egy korábbi verzióját is helyreállíthatja.</span><span class="sxs-lookup"><span data-stu-id="3d499-130">By promoting a snapshot to the position of the base blob, you can restore an earlier version of a blob.</span></span> <span data-ttu-id="3d499-131">A pillanatkép marad, de a következő blob felülírja a pillanatkép írható másolatát.</span><span class="sxs-lookup"><span data-stu-id="3d499-131">The snapshot remains, but the base blob is overwritten with a writable copy of the snapshot.</span></span>
* <span data-ttu-id="3d499-132">Egy forrásblobot más néven pillanatkép másolhatja.</span><span class="sxs-lookup"><span data-stu-id="3d499-132">You can copy a snapshot to a destination blob with a different name.</span></span> <span data-ttu-id="3d499-133">Az eredményül kapott cél blob írható blob és a pillanatkép nem.</span><span class="sxs-lookup"><span data-stu-id="3d499-133">The resulting destination blob is a writable blob and not a snapshot.</span></span>
* <span data-ttu-id="3d499-134">Forrás blob másolását követően a forrás BLOB meglévő pillanatképeket a rendszer nem másolja a cél.</span><span class="sxs-lookup"><span data-stu-id="3d499-134">When a source blob is copied, any snapshots of the source blob are not copied to the destination.</span></span> <span data-ttu-id="3d499-135">Cél blob másolatot felülírja, ha a meglévő pillanatképeket, az eredeti cél blob társított változatlanok maradnak.</span><span class="sxs-lookup"><span data-stu-id="3d499-135">When a destination blob is overwritten with a copy, any snapshots associated with the original destination blob remain intact.</span></span>
* <span data-ttu-id="3d499-136">Amikor egy blokkblob pillanatképet hoz létre, a blob véglegesített tiltólista is másolja a pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="3d499-136">When you create a snapshot of a block blob, the blob's committed block list is also copied to the snapshot.</span></span> <span data-ttu-id="3d499-137">A nem véglegesített téglalapok nem kerülnek.</span><span class="sxs-lookup"><span data-stu-id="3d499-137">Any uncommitted blocks are not copied.</span></span>

## <a name="specify-an-access-condition"></a><span data-ttu-id="3d499-138">Adjon meg egy hozzáférési állapot</span><span class="sxs-lookup"><span data-stu-id="3d499-138">Specify an access condition</span></span>
<span data-ttu-id="3d499-139">A hívás esetén [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], megadhatja a hozzáférés egy feltétele, hogy a pillanatkép létrehozása csak akkor, ha egy feltétel teljesül.</span><span class="sxs-lookup"><span data-stu-id="3d499-139">When you call [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], you can specify an access condition so that the snapshot is created only if a condition is met.</span></span> <span data-ttu-id="3d499-140">A hozzáférési állapot megadásához használja a [AccessCondition] [ dotnet_AccessCondition] paraméter.</span><span class="sxs-lookup"><span data-stu-id="3d499-140">To specify an access condition, use the [AccessCondition][dotnet_AccessCondition] parameter.</span></span> <span data-ttu-id="3d499-141">Ha a megadott feltétel nem teljesül, a pillanatkép nem jön létre, és a Blob szolgáltatás adja vissza. állapotkód: [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.</span><span class="sxs-lookup"><span data-stu-id="3d499-141">If the specified condition is not met, the snapshot is not created, and the Blob service returns status code [HTTPStatusCode][dotnet_HTTPStatusCode].PreconditionFailed.</span></span>

## <a name="delete-snapshots"></a><span data-ttu-id="3d499-142">Törölje a pillanatképeket</span><span class="sxs-lookup"><span data-stu-id="3d499-142">Delete snapshots</span></span>
<span data-ttu-id="3d499-143">Egy blobot a pillanatképek nem törölhető, kivéve, ha a pillanatképek is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="3d499-143">You can't delete a blob with snapshots unless the snapshots are also deleted.</span></span> <span data-ttu-id="3d499-144">Pillanatkép törlése egyenként, vagy adja meg, hogy minden pillanatképet törlődik a forrás blob törlődik.</span><span class="sxs-lookup"><span data-stu-id="3d499-144">You can delete a snapshot individually, or specify that all snapshots be deleted when the source blob is deleted.</span></span> <span data-ttu-id="3d499-145">Ha a blob, amely még a pillanatképeket törölni próbál, hibát okoz.</span><span class="sxs-lookup"><span data-stu-id="3d499-145">If you attempt to delete a blob that still has snapshots, an error results.</span></span>

<span data-ttu-id="3d499-146">Az alábbi példakód bemutatja, hogyan törli a blob és a pillanatképeket a .NET, ahol `blockBlob` típusú objektum [CloudBlockBlob][dotnet_CloudBlockBlob]:</span><span class="sxs-lookup"><span data-stu-id="3d499-146">The following code example shows how to delete a blob and its snapshots in .NET, where `blockBlob` is an object of type [CloudBlockBlob][dotnet_CloudBlockBlob]:</span></span>

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a><span data-ttu-id="3d499-147">Prémium szintű Azure Storage pillanatképei</span><span class="sxs-lookup"><span data-stu-id="3d499-147">Snapshots with Azure Premium Storage</span></span>
<span data-ttu-id="3d499-148">Prémium szintű Storage, pillanatképek használatakor a következő szabályok érvényesek:</span><span class="sxs-lookup"><span data-stu-id="3d499-148">When using snapshots with Premium Storage, the following rules apply:</span></span>

* <span data-ttu-id="3d499-149">A maximális számú pillanatképpel oldalakra vonatkozó blob egy prémium szintű tárfiók / 100.</span><span class="sxs-lookup"><span data-stu-id="3d499-149">The maximum number of snapshots per page blob in a premium storage account is 100.</span></span> <span data-ttu-id="3d499-150">Ha túllépi ezt a határértéket, a pillanatkép-Blob visszaadja hibakód 409 (`SnapshotCountExceeded`).</span><span class="sxs-lookup"><span data-stu-id="3d499-150">If that limit is exceeded, the Snapshot Blob operation returns error code 409 (`SnapshotCountExceeded`).</span></span>
* <span data-ttu-id="3d499-151">Akkor is készítsen pillanatképet oldalakra vonatkozó blob egy prémium szintű tárfiók a 10 percenként egyszer.</span><span class="sxs-lookup"><span data-stu-id="3d499-151">You can take a snapshot of a page blob in a premium storage account once every 10 minutes.</span></span> <span data-ttu-id="3d499-152">Ha adott aránya túllépi a határértéket, a pillanatkép-Blob művelet adja vissza hibakód 409 (`SnapshotOperationRateExceeded`).</span><span class="sxs-lookup"><span data-stu-id="3d499-152">If that rate is exceeded, the Snapshot Blob operation returns error code 409 (`SnapshotOperationRateExceeded`).</span></span>
* <span data-ttu-id="3d499-153">Olvassa el a pillanatképet, használhatja a Blob másolási művelet pillanatkép átmásolása a fiók egy másik oldalakra vonatkozó blob.</span><span class="sxs-lookup"><span data-stu-id="3d499-153">To read a snapshot, you can use the Copy Blob operation to copy a snapshot to another page blob in the account.</span></span> <span data-ttu-id="3d499-154">A másolási művelet a rendeltetési blob nem lehet a meglévő pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="3d499-154">The destination blob for the copy operation must not have any existing snapshots.</span></span> <span data-ttu-id="3d499-155">Ha a cél blob pillanatképekkel rendelkezik, akkor a Blob másolási művelet hibakódot 409 adja vissza (`SnapshotsPresent`).</span><span class="sxs-lookup"><span data-stu-id="3d499-155">If the destination blob does have snapshots, then the Copy Blob operation returns error code 409 (`SnapshotsPresent`).</span></span>

## <a name="return-the-absolute-uri-to-a-snapshot"></a><span data-ttu-id="3d499-156">Térjen vissza a abszolút URI pillanatkép</span><span class="sxs-lookup"><span data-stu-id="3d499-156">Return the absolute URI to a snapshot</span></span>
<span data-ttu-id="3d499-157">A C# Kódpélda pillanatképet hoz létre, és írja meg az elsődleges hely abszolút URI.</span><span class="sxs-lookup"><span data-stu-id="3d499-157">This C# code example creates a snapshot and writes out the absolute URI for the primary location.</span></span>

```csharp
//Create the blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference to a blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of the blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a><span data-ttu-id="3d499-158">Hogyan pillanatképek keletkeznek költségek ismertetése</span><span class="sxs-lookup"><span data-stu-id="3d499-158">Understand how snapshots accrue charges</span></span>
<span data-ttu-id="3d499-159">Egy pillanatképet, amely egy blob csak olvasható másolatát, létrehozása eredményezhet további adatok tárolási költségek a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="3d499-159">Creating a snapshot, which is a read-only copy of a blob, can result in additional data storage charges to your account.</span></span> <span data-ttu-id="3d499-160">Az alkalmazás megtervezéséhez fontos tudni, hogyan lehet, hogy keletkeznek ezeket a díjakat, így minimalizálhatja a költségeket.</span><span class="sxs-lookup"><span data-stu-id="3d499-160">When designing your application, it is important to be aware of how these charges might accrue so that you can minimize costs.</span></span>

### <a name="important-billing-considerations"></a><span data-ttu-id="3d499-161">Fontos számlázási szempontok</span><span class="sxs-lookup"><span data-stu-id="3d499-161">Important billing considerations</span></span>
<span data-ttu-id="3d499-162">Az alábbi lista tartalmazza a legfontosabb megfontolandó a pillanatkép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3d499-162">The following list includes key points to consider when creating a snapshot.</span></span>

* <span data-ttu-id="3d499-163">A tárfiók terhel egyedi blokkok vagy lapok, hogy vannak-e a blob vagy a pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="3d499-163">Your storage account incurs charges for unique blocks or pages, whether they are in the blob or in the snapshot.</span></span> <span data-ttu-id="3d499-164">A fiók nem többletköltségei a pillanatképek társított blob, amíg nem frissíti a blob, amelyeken alapulnak.</span><span class="sxs-lookup"><span data-stu-id="3d499-164">Your account does not incur additional charges for snapshots associated with a blob until you update the blob on which they are based.</span></span> <span data-ttu-id="3d499-165">Miután frissítette az alap blob, a pillanatképek eltér.</span><span class="sxs-lookup"><span data-stu-id="3d499-165">After you update the base blob, it diverges from its snapshots.</span></span> <span data-ttu-id="3d499-166">Ez akkor fordul elő, ha van szó, az egyedi blokkok vagy lapok minden egyes blob vagy a pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="3d499-166">When this happens, you are charged for the unique blocks or pages in each blob or snapshot.</span></span>
* <span data-ttu-id="3d499-167">A blokkolás a blokkblob belül cseréjekor, amelyek blokkolják ezt követően és egyedi blokk számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="3d499-167">When you replace a block within a block blob, that block is subsequently charged as a unique block.</span></span> <span data-ttu-id="3d499-168">Ez igaz, akkor is, ha a blokk blokk azonos Azonosítóval és ugyanazokat az adatokat, mert a pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="3d499-168">This is true even if the block has the same block ID and the same data as it has in the snapshot.</span></span> <span data-ttu-id="3d499-169">A blokk véglegesítése után újra, ez eltér a párjukhoz bármely pillanatfelvétel, és a saját adataikhoz fizetnie kell.</span><span class="sxs-lookup"><span data-stu-id="3d499-169">After the block is committed again, it diverges from its counterpart in any snapshot, and you will be charged for its data.</span></span> <span data-ttu-id="3d499-170">Ugyanez érvényes a lap az oldalakra vonatkozó blob, amely azonos adatokkal rendelkező frissül.</span><span class="sxs-lookup"><span data-stu-id="3d499-170">The same holds true for a page in a page blob that's updated with identical data.</span></span>
* <span data-ttu-id="3d499-171">A mag cseréje a blokkblob meghívásával a [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream] [ dotnet_UploadFromStream], vagy [UploadFromByteArray] [ dotnet_UploadFromByteArray] metódus található blokkok váltja fel.</span><span class="sxs-lookup"><span data-stu-id="3d499-171">Replacing a block blob by calling the [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray] method replaces all blocks in the blob.</span></span> <span data-ttu-id="3d499-172">Ha, hogy a blob társított pillanatkép, az alap blob és a pillanatkép blokkok most tér el, és mindkét blobok blokkokról sávszélességért fizetnie kell.</span><span class="sxs-lookup"><span data-stu-id="3d499-172">If you have a snapshot associated with that blob, all blocks in the base blob and snapshot now diverge, and you will be charged for all the blocks in both blobs.</span></span> <span data-ttu-id="3d499-173">Ez igaz, akkor is, ha az alap blob és a pillanatkép adatainak azonos maradnak.</span><span class="sxs-lookup"><span data-stu-id="3d499-173">This is true even if the data in the base blob and the snapshot remain identical.</span></span>
* <span data-ttu-id="3d499-174">Az Azure Blob szolgáltatás nem rendelkezik segítségével határozza meg, hogy a két blokk azonos adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3d499-174">The Azure Blob service does not have a means to determine whether two blocks contain identical data.</span></span> <span data-ttu-id="3d499-175">Minden feltöltött és véglegesített blokk rendszer egyedi, még akkor is, ha ugyanazokat az adatokat, és a blokk megegyezik.</span><span class="sxs-lookup"><span data-stu-id="3d499-175">Each block that is uploaded and committed is treated as unique, even if it has the same data and the same block ID.</span></span> <span data-ttu-id="3d499-176">Díjak egyedi blokk keletkeznek, mert fontos figyelembe venni, hogy egy blobot, amely rendelkezik egy pillanatkép eredmények további egyedi blokkok és további díjakat frissítése.</span><span class="sxs-lookup"><span data-stu-id="3d499-176">Because charges accrue for unique blocks, it's important to consider that updating a blob that has a snapshot results in additional unique blocks and additional charges.</span></span>

### <a name="minimize-cost-with-snapshot-management"></a><span data-ttu-id="3d499-177">Pillanatkép-kezeléssel költségek minimalizálása érdekében</span><span class="sxs-lookup"><span data-stu-id="3d499-177">Minimize cost with snapshot management</span></span>

<span data-ttu-id="3d499-178">Azt javasoljuk, alaposan további költségek elkerülése érdekében a pillanatképek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="3d499-178">We recommend managing your snapshots carefully to avoid extra charges.</span></span> <span data-ttu-id="3d499-179">Az alábbi gyakorlati tanácsok a felmerült a pillanatképek tárolási költségek csökkentése érdekében kövesse:</span><span class="sxs-lookup"><span data-stu-id="3d499-179">You can follow these best practices to help minimize the costs incurred by the storage of your snapshots:</span></span>

* <span data-ttu-id="3d499-180">Törölje, és hozza létre a pillanatképek társított blob amikor frissíti a blob, még akkor is, ha frissíti azonos adatokkal, kivéve, ha az alkalmazás tervhez szükséges, hogy a pillanatképek karbantartása.</span><span class="sxs-lookup"><span data-stu-id="3d499-180">Delete and re-create snapshots associated with a blob whenever you update the blob, even if you are updating with identical data, unless your application design requires that you maintain snapshots.</span></span> <span data-ttu-id="3d499-181">Törlésével és ismételt létrehozása a blob-pillanatképekkel, akkor biztosíthatja, hogy a blob és a pillanatfelvételeket tér el.</span><span class="sxs-lookup"><span data-stu-id="3d499-181">By deleting and re-creating the blob's snapshots, you can ensure that the blob and snapshots do not diverge.</span></span>
* <span data-ttu-id="3d499-182">Ha a blob-pillanatképek fenntartásának ne hívása [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], vagy [UploadFromByteArray] [ dotnet_UploadFromByteArray] a blob frissítése.</span><span class="sxs-lookup"><span data-stu-id="3d499-182">If you are maintaining snapshots for a blob, avoid calling [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray] to update the blob.</span></span> <span data-ttu-id="3d499-183">Ezek a módszerek a BLOB, amely az alapszintű blob és a pillanatképek tér el jelentősen blokkok cseréjét.</span><span class="sxs-lookup"><span data-stu-id="3d499-183">These methods replace all of the blocks in the blob, causing your base blob and its snapshots to diverge significantly.</span></span> <span data-ttu-id="3d499-184">Ehelyett frissítése a lehető legkevesebb blokkok használatával a [PutBlock] [ dotnet_PutBlock] és [PutBlockList] [ dotnet_PutBlockList] módszerek.</span><span class="sxs-lookup"><span data-stu-id="3d499-184">Instead, update the fewest possible number of blocks by using the [PutBlock][dotnet_PutBlock] and [PutBlockList][dotnet_PutBlockList] methods.</span></span>

### <a name="snapshot-billing-scenarios"></a><span data-ttu-id="3d499-185">Pillanatkép számlázási forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="3d499-185">Snapshot billing scenarios</span></span>
<span data-ttu-id="3d499-186">A következő esetekben bemutatják, hogyan költségek letiltása blob és a pillanatképek keletkeznek.</span><span class="sxs-lookup"><span data-stu-id="3d499-186">The following scenarios demonstrate how charges accrue for a block blob and its snapshots.</span></span>

<span data-ttu-id="3d499-187">**1. forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="3d499-187">**Scenario 1**</span></span>

<span data-ttu-id="3d499-188">Az 1. forgatókönyv az alap blob nem lett frissítve után során létrehozott pillanatképen, így díjak sem merülnek fel, csak az egyedi blokkok 1, 2 és 3.</span><span class="sxs-lookup"><span data-stu-id="3d499-188">In scenario 1, the base blob has not been updated after the snapshot was taken, so charges are incurred only for unique blocks 1, 2, and 3.</span></span>

![Azure Storage-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

<span data-ttu-id="3d499-190">**2. forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="3d499-190">**Scenario 2**</span></span>

<span data-ttu-id="3d499-191">A 2. forgatókönyv az alap blob frissítve lett, de nem rendelkezik a pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="3d499-191">In scenario 2, the base blob has been updated, but the snapshot has not.</span></span> <span data-ttu-id="3d499-192">Blokk 3 frissítették, és annak ellenére, hogy ugyanazokat az adatokat, és ugyanazzal az Azonosítóval tartalmaz, nincs megegyezik a pillanatkép 3 letiltása.</span><span class="sxs-lookup"><span data-stu-id="3d499-192">Block 3 was updated, and even though it contains the same data and the same ID, it is not the same as block 3 in the snapshot.</span></span> <span data-ttu-id="3d499-193">Ennek eredményeképpen a fiók négy blokk számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="3d499-193">As a result, the account is charged for four blocks.</span></span>

![Azure Storage-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

<span data-ttu-id="3d499-195">**3. forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="3d499-195">**Scenario 3**</span></span>

<span data-ttu-id="3d499-196">3. forgatókönyv az alap blob frissítve lett, de nem rendelkezik a pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="3d499-196">In scenario 3, the base blob has been updated, but the snapshot has not.</span></span> <span data-ttu-id="3d499-197">Blokk 3 váltotta fel a kiinduló blob 4 blokkja, de a pillanatkép továbbra is tükrözi blokk 3.</span><span class="sxs-lookup"><span data-stu-id="3d499-197">Block 3 was replaced with block 4 in the base blob, but the snapshot still reflects block 3.</span></span> <span data-ttu-id="3d499-198">Ennek eredményeképpen a fiók négy blokk számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="3d499-198">As a result, the account is charged for four blocks.</span></span>

![Azure Storage-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

<span data-ttu-id="3d499-200">**4. forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="3d499-200">**Scenario 4**</span></span>

<span data-ttu-id="3d499-201">A 4. forgatókönyv az alap blob teljesen frissült, ezért az eredeti blokkok egyike sem tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3d499-201">In scenario 4, the base blob has been completely updated and contains none of its original blocks.</span></span> <span data-ttu-id="3d499-202">Ennek eredményeképpen a fiók összes nyolc egyedi blokk számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="3d499-202">As a result, the account is charged for all eight unique blocks.</span></span> <span data-ttu-id="3d499-203">Ez akkor fordulhat elő, ha egy frissítési módszerét használja, mint [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], vagy [UploadFromByteArray][dotnet_UploadFromByteArray], mert ezek a módszerek cseréjét a blob tartalmát.</span><span class="sxs-lookup"><span data-stu-id="3d499-203">This scenario can occur if you are using an update method such as [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], or [UploadFromByteArray][dotnet_UploadFromByteArray], because these methods replace all of the contents of a blob.</span></span>

![Azure Storage-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a><span data-ttu-id="3d499-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3d499-205">Next steps</span></span>

* <span data-ttu-id="3d499-206">További információ a virtuális gép (VM) lemez pillanatképek használata található [készítsen biztonsági másolatot az Azure nem felügyelt méretű lemezek növekményes pillanatképek](../../virtual-machines/windows/incremental-snapshots.md)</span><span class="sxs-lookup"><span data-stu-id="3d499-206">You can find more information about working with virtual machine (VM) disk snapshots in [Back up Azure unmanaged VM disks with incremental snapshots](../../virtual-machines/windows/incremental-snapshots.md)</span></span>

* <span data-ttu-id="3d499-207">Kód Blob storage használatával, tekintse meg a [Azure mintakódok](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob).</span><span class="sxs-lookup"><span data-stu-id="3d499-207">For additional code examples using Blob storage, see [Azure Code Samples](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob).</span></span> <span data-ttu-id="3d499-208">Töltse le a mintaalkalmazást, majd futtassa, vagy keresse meg a kódot a Githubon.</span><span class="sxs-lookup"><span data-stu-id="3d499-208">You can download a sample application and run it, or browse the code on GitHub.</span></span>

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