---
title: "aaaImport és exportálhatja az adatokat az Azure Redis Cache |} Microsoft Docs"
description: "Megtudhatja, hogyan blobtárolóból a premium Azure Redis Cache osztályt adatok tooand tooimport és exportálása"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 4a68ac38-87af-4075-adab-569d37d7cc9e
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: sdanie
ms.openlocfilehash: f17464b207f1c652952f4da63ca147473fee2759
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="import-and-export-data-in-azure-redis-cache"></a><span data-ttu-id="d8ac5-103">Az Azure Redis Cache adatok importálása és exportálása</span><span class="sxs-lookup"><span data-stu-id="d8ac5-103">Import and Export data in Azure Redis Cache</span></span>
<span data-ttu-id="d8ac5-104">Importálási/exportálási egy Azure Redis Cache adatok felügyeleti művelet, amely lehetővé teszi tooimport adatok Azure Redis Cache vagy Azure Redis Cache exportálási adatokat importálni, és a prémium szintű gyorsítótár tooa blobból az Azure Redis Cache-adatbázis (Rekordadatbázis) pillanatkép exportálása Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-104">Import/Export is an Azure Redis Cache data management operation, which allows you tooimport data into Azure Redis Cache or export data from Azure Redis Cache by importing and exporting a Redis Cache Database (RDB) snapshot from a premium cache tooa blob in an Azure Storage Account.</span></span> 

- <span data-ttu-id="d8ac5-105">**Exportálás** -exportálhatja az Azure Redis Cache Rekordadatbázis pillanatképek tooa oldalakra vonatkozó Blob.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-105">**Export** - you can export your Azure Redis Cache RDB snapshots tooa Page Blob.</span></span>
- <span data-ttu-id="d8ac5-106">**Importálás** -oldalakra vonatkozó Blob vagy egy Blokkblob importálhatja a Redis gyorsítótár Rekordadatbázis pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-106">**Import** - you can import your Redis Cache RDB snapshots from either a Page Blob or a Block Blob.</span></span>

<span data-ttu-id="d8ac5-107">Importálási/exportálási lehetővé teszi a különböző Azure Redis Cache példányai között toomigrate vagy hello gyorsítótár adatokkal használat előtt.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-107">Import/Export enables you toomigrate between different Azure Redis Cache instances or populate hello cache with data before use.</span></span>

<span data-ttu-id="d8ac5-108">Ez a cikk egy útmutatót biztosít az Azure Redis Cache-adatok importálása és exportálása és hello választ ad toocommonly kérdések.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-108">This article provides a guide for importing and exporting data with Azure Redis Cache and provides hello answers toocommonly asked questions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d8ac5-109">Importálási/exportálási jelenleg előzetes verzióban érhető, és csak [prémium csomagban](cache-premium-tier-intro.md) gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-109">Import/Export is in preview and is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span>
>
>

## <a name="import"></a><span data-ttu-id="d8ac5-110">Importálás</span><span class="sxs-lookup"><span data-stu-id="d8ac5-110">Import</span></span>
<span data-ttu-id="d8ac5-111">Importálás használt toobring Redis kompatibilis Rekordadatbázis fájlokat minden futtató bármely felhő és a környezet, beleértve a futó Linux, Windows vagy bármely felhőalapú szolgáltató, például az Amazon Web Services, míg mások Redis Redis-kiszolgáló lehet.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-111">Import can be used toobring Redis compatible RDB files from any Redis server running in any cloud or environment, including Redis running on Linux, Windows, or any cloud provider such as Amazon Web Services and others.</span></span> <span data-ttu-id="d8ac5-112">Adatok importálása egy egyszerűen toocreate egy előre megadott adatokkal gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-112">Importing data is an easy way toocreate a cache with pre-populated data.</span></span> <span data-ttu-id="d8ac5-113">Hello importálási folyamat során az Azure Redis Cache hello Rekordadatbázis fájlok az Azure storage betölti a memóriába, és, majd a kívánt hello kulcsok hello gyorsítótárába.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-113">During hello import process, Azure Redis Cache loads hello RDB files from Azure storage into memory and then inserts hello keys into hello cache.</span></span>

> [!NOTE]
> <span data-ttu-id="d8ac5-114">Hello importálási művelet megkezdése előtt győződjön meg arról, hogy a Redis adatbázis (Rekordadatbázis) fájl vagy -fájlok feltöltése oldalhoz vagy blokk-blobok az Azure-tárfiókba, a hello azonos régióban és az előfizetés az Azure Redis Cache példányt.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-114">Before beginning hello import operation, ensure that your Redis Database (RDB) file or files are uploaded into page or block blobs in Azure storage, in hello same region and subscription as your Azure Redis Cache instance.</span></span> <span data-ttu-id="d8ac5-115">További információkért lásd: [Ismerkedés az Azure Blob storage szolgáltatással](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="d8ac5-115">For more information, see [Get started with Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="d8ac5-116">Ha a Rekordadatbázis fájl hello exportált [Azure Redis Cache exportálása](#export) funkció, a Rekordadatbázis fájl oldalakra vonatkozó blob már tárolja, és készen áll a importálása.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-116">If you exported your RDB file using hello [Azure Redis Cache Export](#export) feature, your RDB file is already stored in a page blob and is ready for importing.</span></span>
>
>

1. <span data-ttu-id="d8ac5-117">tooimport egy vagy több gyorsítótár blobok exportált [tooyour gyorsítótár Tallózás](cache-configure.md#configure-redis-cache-settings) a hello Azure-portálon, és kattintson a **adatimportálás** a hello **erőforrás menü**.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-117">tooimport one or more exported cache blobs, [browse tooyour cache](cache-configure.md#configure-redis-cache-settings) in hello Azure portal and click **Import data** from hello **Resource menu**.</span></span>

    ![Adatok importálása][cache-import-data]
2. <span data-ttu-id="d8ac5-119">Kattintson a **válasszon Blob(s)** , és válassza ki, amely tartalmazza a hello adatok tooimport hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-119">Click **Choose Blob(s)** and select hello storage account that contains hello data tooimport.</span></span>

    ![Válassza ki a tárfiók][cache-import-choose-storage-account]
3. <span data-ttu-id="d8ac5-121">Kattintson a hello adatok tooimport tartalmazó hello tároló.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-121">Click hello container that contains hello data tooimport.</span></span>

    ![Válassza ki a tárolót][cache-import-choose-container]
4. <span data-ttu-id="d8ac5-123">Egy vagy több blobot tooimport hello terület toohello hello blob nevétől balra kattintva válassza ki, és kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-123">Select one or more blobs tooimport by clicking hello area toohello left of hello blob name, and then click **Select**.</span></span>

    ![Válassza ki a blobok][cache-import-choose-blobs]
5. <span data-ttu-id="d8ac5-125">Kattintson a **importálása** toobegin hello az importálási folyamat.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-125">Click **Import** toobegin hello import process.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="d8ac5-126">hello gyorsítótár nem gyorsítótár-ügyfelek által elérhető hello importálási folyamat során, és bármely hello gyorsítótárában adat törlődik.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-126">hello cache is not accessible by cache clients during hello import process, and any existing data in hello cache is deleted.</span></span>
   >
   >

    ![Importálás][cache-import-blobs]

    <span data-ttu-id="d8ac5-128">Kísérheti hello hello importálási művelet a következő hello értesítést kapnak hello Azure-portálon, vagy hello események megtekintése az hello [napló](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="d8ac5-128">You can monitor hello progress of hello import operation by following hello notifications from hello Azure portal, or by viewing hello events in hello [audit log](../azure-resource-manager/resource-group-audit.md).</span></span>

    ![Importálás folyamatban][cache-import-data-import-complete]

## <a name="export"></a><span data-ttu-id="d8ac5-130">Exportálás</span><span class="sxs-lookup"><span data-stu-id="d8ac5-130">Export</span></span>
<span data-ttu-id="d8ac5-131">Exportálás lehetővé teszi tooexport hello adataihoz az Azure Redis Cache tooRedis kompatibilis Rekordadatbázis (oka) t.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-131">Export allows you tooexport hello data stored in Azure Redis Cache tooRedis compatible RDB file(s).</span></span> <span data-ttu-id="d8ac5-132">Ez a szolgáltatás toomove adatokat egy Azure Redis Cache példány tooanother vagy tooanother Redis-kiszolgáló is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-132">You can use this feature toomove data from one Azure Redis Cache instance tooanother or tooanother Redis server.</span></span> <span data-ttu-id="d8ac5-133">VM állomások hello Azure Redis Cache server-példány, és hello fájl kijelölve tárfiók feltöltött toohello hello hello exportálás során ideiglenes fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-133">During hello export process, a temporary file is created on hello VM that hosts hello Azure Redis Cache server instance, and hello file is uploaded toohello designated storage account.</span></span> <span data-ttu-id="d8ac5-134">Hello az exportálási művelet befejezése után a vagy állapota sikeres végrehajtásával vagy hibajelzéssel hello ideiglenes fájl törlődik.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-134">When hello export operation completes with either a status of success or failure, hello temporary file is deleted.</span></span>

1. <span data-ttu-id="d8ac5-135">tooexport hello tartalmát hello gyorsítótár toostorage, [tooyour gyorsítótár Tallózás](cache-configure.md#configure-redis-cache-settings) a hello Azure-portálon, és kattintson a **adatok exportálása** a hello **erőforrás menü**.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-135">tooexport hello current contents of hello cache toostorage, [browse tooyour cache](cache-configure.md#configure-redis-cache-settings) in hello Azure portal and click **Export data** from hello **Resource menu**.</span></span>

    ![A tároló kiválasztása][cache-export-data-choose-storage-container]
2. <span data-ttu-id="d8ac5-137">Kattintson a **válassza ki a tároló** válassza ki a kívánt hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-137">Click **Choose Storage Container** and select hello desired storage account.</span></span> <span data-ttu-id="d8ac5-138">hello tárfiók hello kell lennie ugyanazon előfizetésével és régiójával, a gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-138">hello storage account must be in hello same subscription and region as your cache.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="d8ac5-139">Exportálja a lapblobokat, amelyek klasszikus és Resource Manager tárfiókok által támogatott, de nem támogatja a együttműködik [Blob storage-fiókok](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts) most.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-139">Export works with page blobs, which are supported by both classic and Resource Manager storage accounts, but are not supported by [Blob storage accounts](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts) at this time.</span></span>
   >
   >

    ![Tárfiók][cache-export-data-choose-account]
3. <span data-ttu-id="d8ac5-141">Válassza ki a hello szükséges blobtároló és kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-141">Choose hello desired blob container and click **Select**.</span></span> <span data-ttu-id="d8ac5-142">toouse új tárolót, kattintson a **tároló hozzáadása** tooadd az első, és jelölje ki azt hello listából.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-142">toouse new a container, click **Add Container** tooadd it first and then select it from hello list.</span></span>

    ![A tároló kiválasztása][cache-export-data-container]
4. <span data-ttu-id="d8ac5-144">Adjon meg egy **Blob előtagja** kattintson **exportálása** toostart hello exportálás során.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-144">Type a **Blob name prefix** and click **Export** toostart hello export process.</span></span> <span data-ttu-id="d8ac5-145">hello blob előtagja az exportálási művelet által létrehozott fájlok használt tooprefix hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-145">hello blob name prefix is used tooprefix hello names of files generated by this export operation.</span></span>

    ![Exportálás][cache-export-data]

    <span data-ttu-id="d8ac5-147">Kísérheti hello hello az exportálási művelet a következő hello értesítést kapnak hello Azure-portálon, vagy hello események megtekintése az hello [napló](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="d8ac5-147">You can monitor hello progress of hello export operation by following hello notifications from hello Azure portal, or by viewing hello events in hello [audit log](../azure-resource-manager/resource-group-audit.md).</span></span>

    ![Az adatok exportálása befejeződött][cache-export-data-export-complete]

    <span data-ttu-id="d8ac5-149">Gyorsítótárak hello exportálási folyamat során elérhetők maradnak.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-149">Caches remain available for use during hello export process.</span></span>

## <a name="importexport-faq"></a><span data-ttu-id="d8ac5-150">Importálási/exportálási – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="d8ac5-150">Import/Export FAQ</span></span>
<span data-ttu-id="d8ac5-151">Ez a szakasz hello Import/Export szolgáltatás kapcsolatos gyakori kérdésekre.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-151">This section contains frequently asked questions about hello Import/Export feature.</span></span>

* [<span data-ttu-id="d8ac5-152">Milyen árképzési tiers használhatja az Import/Export?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-152">What pricing tiers can use Import/Export?</span></span>](#what-pricing-tiers-can-use-importexport)
* [<span data-ttu-id="d8ac5-153">Szeretnék adatokat importálhat a Redis-kiszolgáló?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-153">Can I import data from any Redis server?</span></span>](#can-i-import-data-from-any-redis-server)
* [<span data-ttu-id="d8ac5-154">Milyen Rekordadatbázis verziók lehet importálni?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-154">What RDB versions can I import?</span></span>](#what-rdb-versions-can-i-import)
* [<span data-ttu-id="d8ac5-155">A gyorsítótár érhető el az importálási/exportálási művelet közben?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-155">Is my cache available during an Import/Export operation?</span></span>](#is-my-cache-available-during-an-importexport-operation)
* [<span data-ttu-id="d8ac5-156">Használhatok Import/Export Redis-fürt?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-156">Can I use Import/Export with Redis cluster?</span></span>](#can-i-use-importexport-with-redis-cluster)
* [<span data-ttu-id="d8ac5-157">Hogyan működik a Import/Export beállítása egy egyéni adatbázisok?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-157">How does Import/Export work with a custom databases setting?</span></span>](#how-does-importexport-work-with-a-custom-databases-setting)
* [<span data-ttu-id="d8ac5-158">Miben különbözik Import/Export Redis-adatmegőrzés?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-158">How is Import/Export different from Redis persistence?</span></span>](#how-is-importexport-different-from-redis-persistence)
* [<span data-ttu-id="d8ac5-159">Automatizálható a PowerShell, a parancssori felületen vagy a más felügyeleti ügyfelek Import/Export?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-159">Can I automate Import/Export using PowerShell, CLI, or other management clients?</span></span>](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [<span data-ttu-id="d8ac5-160">Időtúllépési hiba érkezett a importálási/exportálási művelet közben. Mit jelent?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-160">I received a timeout error during my Import/Export operation. What does it mean?</span></span>](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [<span data-ttu-id="d8ac5-161">Hiba történt az adatok tooAzure Blob Storage exportálásakor jelent. mi történt?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-161">I got an error when exporting my data tooAzure Blob Storage. What happened?</span></span>](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a><span data-ttu-id="d8ac5-162">Milyen árképzési tiers használhatja az Import/Export?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-162">What pricing tiers can use Import/Export?</span></span>
<span data-ttu-id="d8ac5-163">Importálási/exportálási csak hello prémium tarifacsomag érhető el.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-163">Import/Export is available only in hello premium pricing tier.</span></span>

### <a name="can-i-import-data-from-any-redis-server"></a><span data-ttu-id="d8ac5-164">Szeretnék adatokat importálhat a Redis-kiszolgáló?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-164">Can I import data from any Redis server?</span></span>
<span data-ttu-id="d8ac5-165">Igen, továbbá tooimporting Azure Redis Cache példány exportált adatok, importálhat Rekordadatbázis fájlok bármely Redis-kiszolgáló futtatja a felhő vagy a környezetben, például a Linux, Windows vagy például az Amazon Web Services szolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-165">Yes, in addition tooimporting data exported from Azure Redis Cache instances, you can import RDB files from any Redis server running in any cloud or environment, such as Linux, Windows, or cloud providers such as Amazon Web Services.</span></span> <span data-ttu-id="d8ac5-166">toodo, ez a feltöltési hello Rekordadatbázis fájlként szükséges hello Redis-kiszolgáló be egy Azure Storage-fiókot, majd importálja a lap vagy blokk blob azt be a prémium szintű Azure Redis Cache példány.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-166">toodo this, upload hello RDB file from hello desired Redis server into a page or block blob in an Azure Storage Account, and then import it into your premium Azure Redis Cache instance.</span></span> <span data-ttu-id="d8ac5-167">Például előfordulhat, hogy szeretne tooexport hello adatokat a termelési gyorsítótárból, és importálja a fájlt a gyorsítótár tesztelési, illetve áttelepítési egy átmeneti környezet részeként használatos.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-167">For example, you may want tooexport hello data from your production cache and import it into a cache used as part of a staging environment for testing or migration.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d8ac5-168">toosuccessfully exportált eltérő Azure Redis Cache Redis-kiszolgálókról, amikor egy oldalakra vonatkozó blob, és a méretet a hello blob egyeztetni kell olyan 512 bájtos határon belül található adatok importálása.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-168">toosuccessfully import data exported from Redis servers other than Azure Redis Cache when using a page blob, hello page blob size must be aligned on a 512 byte boundary.</span></span> <span data-ttu-id="d8ac5-169">A minta kód tooperform el a szükséges bájt kitöltési című [minta lap blog feltöltéséhez](https://github.com/JimRoberts-MS/SamplePageBlobUpload).</span><span class="sxs-lookup"><span data-stu-id="d8ac5-169">For sample code tooperform any required byte padding, see [Sample page blog upload](https://github.com/JimRoberts-MS/SamplePageBlobUpload).</span></span>
> 
> 

### <a name="what-rdb-versions-can-i-import"></a><span data-ttu-id="d8ac5-170">Milyen Rekordadatbázis verziók lehet importálni?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-170">What RDB versions can I import?</span></span>

<span data-ttu-id="d8ac5-171">Azure Redis Cache Rekordadatbázis importálási legfeljebb 7-es verzió Rekordadatbázis keresztül támogat.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-171">Azure Redis Cache supports RDB import up through RDB version 7.</span></span>

### <a name="is-my-cache-available-during-an-importexport-operation"></a><span data-ttu-id="d8ac5-172">A gyorsítótár érhető el az importálási/exportálási művelet közben?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-172">Is my cache available during an Import/Export operation?</span></span>
* <span data-ttu-id="d8ac5-173">**Exportálás** - gyorsítótárak elérhetők maradnak, és folytathatja a toouse a gyorsítótár az exportálási művelet során.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-173">**Export** - Caches remain available and you can continue toouse your cache during an export operation.</span></span>
* <span data-ttu-id="d8ac5-174">**Importálás** - gyorsítótárak elérhetetlenné válik, az importálási művelet indításakor, és hello importálási művelet befejeződése után lesz használható.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-174">**Import** - Caches become unavailable when an import operation starts, and become available for use when hello import operation completes.</span></span>

### <a name="can-i-use-importexport-with-redis-cluster"></a><span data-ttu-id="d8ac5-175">Használhatok Import/Export Redis-fürt?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-175">Can I use Import/Export with Redis cluster?</span></span>
<span data-ttu-id="d8ac5-176">Igen, és akkor is importálási/exportálási egy fürtözött gyorsítótár és egy nem fürtözött gyorsítótár között.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-176">Yes, and you can import/export between a clustered cache and a non-clustered cache.</span></span> <span data-ttu-id="d8ac5-177">A Redis-fürt óta [csak által támogatott adatbázis-0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), egyetlen megadott adattal sem adatbázisok 0 kivételével nincs importálva.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-177">Since Redis cluster [only supports database 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), any data in databases other than 0 isn't imported.</span></span> <span data-ttu-id="d8ac5-178">Fürtözött gyorsítótár-adatok importálása során a rendszer újra hello kulcsok hello szilánkok hello fürt között.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-178">When clustered cache data is imported, hello keys are redistributed among hello shards of hello cluster.</span></span>

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a><span data-ttu-id="d8ac5-179">Hogyan működik a Import/Export beállítása egy egyéni adatbázisok?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-179">How does Import/Export work with a custom databases setting?</span></span>
<span data-ttu-id="d8ac5-180">Különböző tartalmaznak az egyes tarifacsomagok [korlátok adatbázisok](cache-configure.md#databases), úgy, hogy nincs szempontokat importálásakor, ha egy egyéni értéket hello konfigurált `databases` beállítása a gyorsítótár létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-180">Some pricing tiers have different [databases limits](cache-configure.md#databases), so there are some considerations when importing if you configured a custom value for hello `databases` setting during cache creation.</span></span>

* <span data-ttu-id="d8ac5-181">Az alsó tarifacsomag tooa importálásakor `databases` , amelyből exportált hello réteg határa:</span><span class="sxs-lookup"><span data-stu-id="d8ac5-181">When importing tooa pricing tier with a lower `databases` limit than hello tier from which you exported:</span></span>
  * <span data-ttu-id="d8ac5-182">Hello alapértelmezett számának használata `databases`, amely az összes tarifacsomagok 16, adatok nem vesztek el.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-182">If you are using hello default number of `databases`, which is 16 for all pricing tiers, no data is lost.</span></span>
  * <span data-ttu-id="d8ac5-183">Ha egyéni számos használ `databases` , hogy a hello hello határokon belül esik réteget toowhich importált adatok nem vesztek el.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-183">If you are using a custom number of `databases` that falls within hello limits for hello tier toowhich you are importing, no data is lost.</span></span>
  * <span data-ttu-id="d8ac5-184">Az exportált adatok egy adatbázisban, amelynek hossza meghaladja a hello korlátok hello új réteg adatokat tartalmazott, ha a rendszer nem importálja ezeket magasabb adatbázisok hello adatait.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-184">If your exported data contained data in a database that exceeds hello limits of hello new tier, hello data from those higher databases is not imported.</span></span>

### <a name="how-is-importexport-different-from-redis-persistence"></a><span data-ttu-id="d8ac5-185">Miben különbözik Import/Export Redis-adatmegőrzés?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-185">How is Import/Export different from Redis persistence?</span></span>
<span data-ttu-id="d8ac5-186">Azure Redis Cache adatmegőrzési lehetővé teszi a Redis tooAzure tárolási toopersist adataihoz.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-186">Azure Redis Cache persistence allows you toopersist data stored in Redis tooAzure Storage.</span></span> <span data-ttu-id="d8ac5-187">Ha adatmegőrzési van konfigurálva, az Azure Redis Cache hello Redis gyorsítótár egy Redis bináris formátum toodisk egy konfigurálható biztonsági mentési gyakoriság alapján a pillanatkép továbbra is fennáll.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-187">When persistence is configured, Azure Redis Cache persists a snapshot of hello Redis cache in a Redis binary format toodisk based on a configurable backup frequency.</span></span> <span data-ttu-id="d8ac5-188">Ha egy katasztrofális esemény történik, amely letiltja a hello elsődleges és replika gyorsítótár, hello gyorsítótárazott adatokat állítja automatikusan hello legutóbbi pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-188">If a catastrophic event occurs that disables both hello primary and replica cache, hello cache data is restored automatically using hello most recent snapshot.</span></span> <span data-ttu-id="d8ac5-189">További információkért lásd: [hogyan tooconfigure adatok megőrzését egy prémium szintű Azure Redis Cache](cache-how-to-premium-persistence.md).</span><span class="sxs-lookup"><span data-stu-id="d8ac5-189">For more information, see [How tooconfigure data persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md).</span></span>

<span data-ttu-id="d8ac5-190">Importálás / exportálás lehetővé teszi a toobring adatok vagy az Azure Redis Cache exportálása.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-190">Import/ Export allows you toobring data into or export from Azure Redis Cache.</span></span> <span data-ttu-id="d8ac5-191">Biztonsági mentés konfigurálása, és nem visszaállítása a Redis-adatmegőrzés használatával.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-191">It does not configure backup and restore using Redis persistence.</span></span>

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a><span data-ttu-id="d8ac5-192">Automatizálható a PowerShell, a parancssori felületen vagy a más felügyeleti ügyfelek Import/Export?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-192">Can I automate Import/Export using PowerShell, CLI, or other management clients?</span></span>
<span data-ttu-id="d8ac5-193">Igen, a PowerShell útmutatásért lásd: [tooimport a Redis gyorsítótár](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) és [tooexport a Redis gyorsítótár](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).</span><span class="sxs-lookup"><span data-stu-id="d8ac5-193">Yes, for PowerShell instructions see [tooimport a Redis cache](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) and [tooexport a Redis cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).</span></span>

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a><span data-ttu-id="d8ac5-194">Időtúllépési hiba érkezett a importálási/exportálási művelet közben.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-194">I received a timeout error during my Import/Export operation.</span></span> <span data-ttu-id="d8ac5-195">Mit jelent?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-195">What does it mean?</span></span>
<span data-ttu-id="d8ac5-196">Ha a számítógép a hello **adatimportálás** vagy **adatok exportálása** panel több mint 15 percig hello művelet kezdeményezése előtt, hibaüzenetet kap egy hiba üzenet hasonló toohello a következő példa a:</span><span class="sxs-lookup"><span data-stu-id="d8ac5-196">If you remain on hello **Import data** or **Export data** blade for longer than 15 minutes before initiating hello operation, you receive an error with an error message similar toohello following example:</span></span>

    hello request tooimport data into cache 'contoso55' failed with status 'error' and error 'One of hello SAS URIs provided could not be used for hello following reason: hello SAS token end time (se) must be at least 1 hour from now and hello start time (st), if given, must be at least 15 minutes in hello past.

<span data-ttu-id="d8ac5-197">tooresolve, kezdeményezése hello importálási vagy exportálási művelet előtt 15 perc telt el.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-197">tooresolve this, initiate hello import or export operation before 15 minutes has elapsed.</span></span>

### <a name="i-got-an-error-when-exporting-my-data-tooazure-blob-storage-what-happened"></a><span data-ttu-id="d8ac5-198">Hiba történt az adatok tooAzure Blob Storage exportálásakor jelent.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-198">I got an error when exporting my data tooAzure Blob Storage.</span></span> <span data-ttu-id="d8ac5-199">mi történt?</span><span class="sxs-lookup"><span data-stu-id="d8ac5-199">What happened?</span></span>
<span data-ttu-id="d8ac5-200">Exportálás csak Rekordadatbázis fájlokat tárolja a lapblobokat működik.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-200">Export works only with RDB files stored as page blobs.</span></span> <span data-ttu-id="d8ac5-201">Egyéb blob típusú jelenleg nem támogatottak, beleértve a blob storage-fiókok gyakran és ritkán használt rétegekkel.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-201">Other blob types are not currently supported, including blob storage accounts with hot and cool tiers.</span></span> <span data-ttu-id="d8ac5-202">További információkért lásd: [Blob storage-fiókok](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts).</span><span class="sxs-lookup"><span data-stu-id="d8ac5-202">For more information, see [Blob storage accounts](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8ac5-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d8ac5-203">Next steps</span></span>
<span data-ttu-id="d8ac5-204">Ismerje meg, hogyan több premium toouse gyorsítótár szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="d8ac5-204">Learn how toouse more premium cache features.</span></span>

* [<span data-ttu-id="d8ac5-205">Bevezetés toohello Azure Redis Cache prémium szintjének</span><span class="sxs-lookup"><span data-stu-id="d8ac5-205">Introduction toohello Azure Redis Cache Premium tier</span></span>](cache-premium-tier-intro.md)    

<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png
