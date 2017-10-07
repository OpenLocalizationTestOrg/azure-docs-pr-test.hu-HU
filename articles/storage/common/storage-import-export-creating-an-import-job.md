---
title: "az importálási feladat az Azure Import/Export aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate a Microsoft Azure Import/Export szolgáltatás hello importálásakor."
author: muralikk
manager: syadav
editor: syadav
services: storage
documentationcenter: 
ms.assetid: 8b886e83-6148-4149-9d0f-5d48ec822475
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: da974c33a3688bb5e2412c8bfcbeca704096c2fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-import-job-for-hello-azure-importexport-service"></a><span data-ttu-id="24436-103">Az importálási feladat hello Azure Import/Export szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="24436-103">Creating an import job for hello Azure Import/Export service</span></span>

<span data-ttu-id="24436-104">Az importálási feladat hello Microsoft Azure Import/Export szolgáltatás REST API hello segítségével létrehozása magában foglalja a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="24436-104">Creating an import job for hello Microsoft Azure Import/Export service using hello REST API involves hello following steps:</span></span>

-   <span data-ttu-id="24436-105">Az Azure Import/Export eszköz hello meghajtók előkészítése.</span><span class="sxs-lookup"><span data-stu-id="24436-105">Preparing drives with hello Azure Import/Export Tool.</span></span>

-   <span data-ttu-id="24436-106">Nem sikerült beolvasni hello hely toowhich tooship hello meghajtó.</span><span class="sxs-lookup"><span data-stu-id="24436-106">Obtaining hello location toowhich tooship hello drive.</span></span>

-   <span data-ttu-id="24436-107">Hello importálási feladat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="24436-107">Creating hello import job.</span></span>

-   <span data-ttu-id="24436-108">TooMicrosoft hello szállítási meghajtók támogatott szolgáltatónként szolgáltatáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="24436-108">Shipping hello drives tooMicrosoft via a supported carrier service.</span></span>

-   <span data-ttu-id="24436-109">Hello importálási feladat részletei szállítási hello frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="24436-109">Updating hello import job with hello shipping details.</span></span>

 <span data-ttu-id="24436-110">Lásd: [hello Microsoft Azure Import/Export szolgáltatás tooTransfer adatok tooBlob Storage használatával](storage-import-export-service.md) hello Import/Export szolgáltatás és mutatja be a áttekintését hogyan toouse hello [Azure-portálon](https://portal.azure.com/) toocreate és kezelése az importálás és exportálni a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="24436-110">See [Using hello Microsoft Azure Import/Export service tooTransfer Data tooBlob Storage](storage-import-export-service.md) for an overview of hello Import/Export service and a tutorial that demonstrates how toouse hello [Azure  portal](https://portal.azure.com/) toocreate and manage import and export jobs.</span></span>

## <a name="preparing-drives-with-hello-azure-importexport-tool"></a><span data-ttu-id="24436-111">Az Azure Import/Export eszköz hello meghajtók előkészítése</span><span class="sxs-lookup"><span data-stu-id="24436-111">Preparing drives with hello Azure Import/Export Tool</span></span>

<span data-ttu-id="24436-112">hello lépéseket tooprepare meghajtók, hogy az importálás vannak hello azonos e hoz létre jobvia hello portal hello vagy keresztül hello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="24436-112">hello steps tooprepare drives for an import job are hello same whether you create hello jobvia hello portal or via hello REST API.</span></span>

<span data-ttu-id="24436-113">Az alábbiakban van meghajtó előkészítése rövid áttekintést.</span><span class="sxs-lookup"><span data-stu-id="24436-113">Below is a brief overview of drive preparation.</span></span> <span data-ttu-id="24436-114">Tekintse meg a toohello [Azure Import-ExportTool hivatkozás](storage-import-export-tool-how-to-v1.md) részletes utasításokért.</span><span class="sxs-lookup"><span data-stu-id="24436-114">Refer toohello [Azure Import-ExportTool Reference](storage-import-export-tool-how-to-v1.md) for complete instructions.</span></span> <span data-ttu-id="24436-115">Hello Azure Import/Export eszközt letöltheti [Itt](http://go.microsoft.com/fwlink/?LinkID=301900).</span><span class="sxs-lookup"><span data-stu-id="24436-115">You can download hello Azure Import/Export Tool [here](http://go.microsoft.com/fwlink/?LinkID=301900).</span></span>

<span data-ttu-id="24436-116">A meghajtó előkészítése foglal magában:</span><span class="sxs-lookup"><span data-stu-id="24436-116">Preparing your drive involves:</span></span>

-   <span data-ttu-id="24436-117">Azonosító hello adatok toobe importálva.</span><span class="sxs-lookup"><span data-stu-id="24436-117">Identifying hello data toobe imported.</span></span>

-   <span data-ttu-id="24436-118">Hello cél blobot, amely a Windows Azure Storage azonosítása.</span><span class="sxs-lookup"><span data-stu-id="24436-118">Identifying hello destination blobs in Windows Azure Storage.</span></span>

-   <span data-ttu-id="24436-119">Hello Azure Import/Export eszköz toocopy használatával, az adatok tooone vagy további merevlemez-meghajtókat.</span><span class="sxs-lookup"><span data-stu-id="24436-119">Using hello Azure Import/Export Tool toocopy your data tooone or more hard drives.</span></span>

 <span data-ttu-id="24436-120">hello Azure Import/Export eszköz is létrehoz egy jegyzékfájl hello meghajtókhoz előkészítettként van.</span><span class="sxs-lookup"><span data-stu-id="24436-120">hello Azure Import/Export Tool will also generate a manifest file for each of hello drives as it is prepared.</span></span> <span data-ttu-id="24436-121">A jegyzékfájl tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="24436-121">A manifest file contains:</span></span>

-   <span data-ttu-id="24436-122">Egy feltöltési és a fájlok tooblobs hello hozzárendelések szánt fájlok hello enumerálása.</span><span class="sxs-lookup"><span data-stu-id="24436-122">An enumeration of all hello files intended for upload and hello mappings from these files tooblobs.</span></span>

-   <span data-ttu-id="24436-123">Az egyes fájlok hello szegmensek ellenőrzőösszegeket.</span><span class="sxs-lookup"><span data-stu-id="24436-123">Checksums of hello segments of each file.</span></span>

-   <span data-ttu-id="24436-124">Minden egyes blob a metaadatok és a Tulajdonságok tooassociate hello kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="24436-124">Information about hello metadata and properties tooassociate with each blob.</span></span>

-   <span data-ttu-id="24436-125">Egy listája hello művelet tootake hello éppen feltöltött blob-e azonos nevet, amint egy meglévő blob hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="24436-125">A listing of hello action tootake if a blob that is being uploaded has hello same name as an existing blob in hello container.</span></span> <span data-ttu-id="24436-126">A lehetséges értékek: a) hello blob felülírása hello fájl, a b) megtartani hello meglévő blob és a skip hello fájl feltöltése, a c) fűzzön hozzá utótag toohello nevet úgy, hogy más fájlok nem ütköznek.</span><span class="sxs-lookup"><span data-stu-id="24436-126">Possible options are: a) overwrite hello blob with hello file, b) keep hello existing blob and skip uploading hello file, c) append a suffix toohello name so that it does not conflict with other files.</span></span>

## <a name="obtaining-your-shipping-location"></a><span data-ttu-id="24436-127">A szállítási hely beszerzése</span><span class="sxs-lookup"><span data-stu-id="24436-127">Obtaining your shipping location</span></span>

<span data-ttu-id="24436-128">Mielőtt létrehozna egy importálási feladat, szükséges tooobtain szállítási hely nevét és címét hívó hello [lista helyek](/rest/api/storageimportexport/listlocations) műveletet.</span><span class="sxs-lookup"><span data-stu-id="24436-128">Before creating an import job, you need tooobtain a shipping location name and address by calling hello [List Locations](/rest/api/storageimportexport/listlocations) operation.</span></span> <span data-ttu-id="24436-129">`List Locations`helyek és a levelezési címek listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="24436-129">`List Locations` will return a list of locations and their mailing addresses.</span></span> <span data-ttu-id="24436-130">Jelöljön ki egy helyet a hello-listát adott vissza, és küldje el a merevlemez-meghajtók toothat címét.</span><span class="sxs-lookup"><span data-stu-id="24436-130">You can select a location from hello returned list and ship your hard drives toothat address.</span></span> <span data-ttu-id="24436-131">Is használhatja a hello `Get Location` művelet tooobtain hello közvetlenül szállítási címeket az adott helyen.</span><span class="sxs-lookup"><span data-stu-id="24436-131">You can also use hello `Get Location` operation tooobtain hello shipping address for a specific location directly.</span></span>

 <span data-ttu-id="24436-132">Kövesse az alábbi tooobtain hello szállítási hely hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="24436-132">Follow hello steps below tooobtain hello shipping location:</span></span>

-   <span data-ttu-id="24436-133">A tárfiók helye hello hello nevének azonosítása.</span><span class="sxs-lookup"><span data-stu-id="24436-133">Identify hello name of hello location of your storage account.</span></span> <span data-ttu-id="24436-134">Ez az érték hello alatt található **hely** hello tárolási fiók található **irányítópult** az Azure portál vagy hello service management API-művelet használatával lekérdezett hello [tárolási beolvasása Tulajdonságok fiók](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span><span class="sxs-lookup"><span data-stu-id="24436-134">This value can be found under hello **Location** field on hello storage account's **Dashboard** in hello Azure portal or queried for by using hello service management API operation [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span></span>

-   <span data-ttu-id="24436-135">Ezt a tárfiókot hello helyre, amely elérhető tooprocess beolvasása hívó hello által `Get Location` műveletet.</span><span class="sxs-lookup"><span data-stu-id="24436-135">Retrieve hello location that is available tooprocess this storage account by calling hello `Get Location` operation.</span></span>

-   <span data-ttu-id="24436-136">Ha hello `AlternateLocations` hello hely tulajdonsága tartalmazza magát hello helye, akkor célszerű rendben toouse ezen a helyen.</span><span class="sxs-lookup"><span data-stu-id="24436-136">If hello `AlternateLocations` property of hello location contains hello location itself, then it is okay toouse this location.</span></span> <span data-ttu-id="24436-137">Ellenkező esetben hívható hello `Get Location` hello másodlagos helyek egyikén újra a műveletet.</span><span class="sxs-lookup"><span data-stu-id="24436-137">Otherwise, call hello `Get Location` operation again with one of hello alternate locations.</span></span> <span data-ttu-id="24436-138">hello eredeti helyre ideiglenesen le lehet, hogy a következő karbantartási.</span><span class="sxs-lookup"><span data-stu-id="24436-138">hello original location might be temporarily closed for maintenance.</span></span>

## <a name="creating-hello-import-job"></a><span data-ttu-id="24436-139">Hello importálási feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="24436-139">Creating hello import job</span></span>
<span data-ttu-id="24436-140">toocreate hello importálási feladat, hívás hello [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet.</span><span class="sxs-lookup"><span data-stu-id="24436-140">toocreate hello import job, call hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="24436-141">Szüksége lesz a következő információ tooprovide hello:</span><span class="sxs-lookup"><span data-stu-id="24436-141">You will need tooprovide hello following information:</span></span>

-   <span data-ttu-id="24436-142">Hello feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="24436-142">A name for hello job.</span></span>

-   <span data-ttu-id="24436-143">hello tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="24436-143">hello storage account name.</span></span>

-   <span data-ttu-id="24436-144">a hely neve, hello előző lépésben beszerzett szállítási hello.</span><span class="sxs-lookup"><span data-stu-id="24436-144">hello shipping location name, obtained from hello previous step.</span></span>

-   <span data-ttu-id="24436-145">A feladat típusa (importálás).</span><span class="sxs-lookup"><span data-stu-id="24436-145">A job type (Import).</span></span>

-   <span data-ttu-id="24436-146">hello Válaszcím ahol hello meghajtók küldjön hello importálási feladat befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="24436-146">hello return address where hello drives should be sent after hello import job has completed.</span></span>

-   <span data-ttu-id="24436-147">a meghajtók hello feladat hello listáját.</span><span class="sxs-lookup"><span data-stu-id="24436-147">hello list of drives in hello job.</span></span> <span data-ttu-id="24436-148">Minden olyan meghajtó meg kell adni a következő információ hello meghajtó előkészítési lépés során kapott hello:</span><span class="sxs-lookup"><span data-stu-id="24436-148">For each drive, you must include hello following information that was obtained during hello drive preparation step:</span></span>

    -   <span data-ttu-id="24436-149">hello meghajtó azonosítója</span><span class="sxs-lookup"><span data-stu-id="24436-149">hello drive Id</span></span>

    -   <span data-ttu-id="24436-150">hello BitLocker kulcs</span><span class="sxs-lookup"><span data-stu-id="24436-150">hello BitLocker key</span></span>

    -   <span data-ttu-id="24436-151">hello jegyzékfájl relatív elérési út hello merevlemezről.</span><span class="sxs-lookup"><span data-stu-id="24436-151">hello manifest file relative path on hello hard drive</span></span>

    -   <span data-ttu-id="24436-152">hello Base16 kódolású jegyzékfájl MD5 kivonatoló</span><span class="sxs-lookup"><span data-stu-id="24436-152">hello Base16 encoded manifest file MD5 hash</span></span>

## <a name="shipping-your-drives"></a><span data-ttu-id="24436-153">A meghajtók szállítási</span><span class="sxs-lookup"><span data-stu-id="24436-153">Shipping your drives</span></span>
<span data-ttu-id="24436-154">Kell küldje el a meghajtók toohello cím hello előző lépésben beszerzett, és meg kell adnia a hello csomag számú követési hello az Import/Export szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="24436-154">You must ship your drives toohello address that you obtained from hello previous step, and you must provide hello Import/Export service with hello tracking number of hello package.</span></span>

> [!NOTE]
>  <span data-ttu-id="24436-155">A meghajtók, amely előállít egy azonosítószám a csomag támogatott szolgáltatónként szolgáltatáson keresztül kell küldje el.</span><span class="sxs-lookup"><span data-stu-id="24436-155">You must ship your drives via a supported carrier service, which will provide a tracking number for your package.</span></span>

## <a name="updating-hello-import-job-with-your-shipping-information"></a><span data-ttu-id="24436-156">A szállítási adatokat hello importálási feladat frissítése</span><span class="sxs-lookup"><span data-stu-id="24436-156">Updating hello import job with your shipping information</span></span>
<span data-ttu-id="24436-157">Miután a nyilvántartási szám, hívja hello [frissítés Feladattulajdonság](/api/storageimportexport/jobs#Jobs_Update) művelet tooupdate hello szállítási szolgáltatónként nevét, hello azonosítószám hello feladat és hello szolgáltatónként számát a visszatérési szállításra.</span><span class="sxs-lookup"><span data-stu-id="24436-157">After you have your tracking number, call hello [Update Job Properties](/api/storageimportexport/jobs#Jobs_Update) operation tooupdate hello shipping carrier name, hello tracking number for hello job, and hello carrier account number for return shipping.</span></span> <span data-ttu-id="24436-158">Opcionálisan megadhat meghajtók és a szállítási dátumot is hello hello száma.</span><span class="sxs-lookup"><span data-stu-id="24436-158">You can optionally specify hello number of drives and hello shipping date as well.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24436-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24436-159">Next steps</span></span>

* [<span data-ttu-id="24436-160">Hello Import/Export szolgáltatás REST API használatával</span><span class="sxs-lookup"><span data-stu-id="24436-160">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
