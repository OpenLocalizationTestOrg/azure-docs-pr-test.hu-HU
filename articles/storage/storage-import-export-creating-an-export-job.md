---
title: "az exportálási feladat az Azure Import/Export aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate exportálása a Microsoft Azure Import/Export szolgáltatás hello feladat."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 613d480b-a8ef-4b28-8f54-54174d59b3f4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 4a10b42cc86dbf3bcea3a515bc065e2259228ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-export-job-for-hello-azure-importexport-service"></a><span data-ttu-id="102a4-103">Az Azure Import/Export szolgáltatás hello exportálási feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="102a4-103">Creating an export job for hello Azure Import/Export service</span></span>
<span data-ttu-id="102a4-104">A lépéseket követve hello hello Microsoft Azure Import/Export szolgáltatás REST API hello segítségével exportálási feladat létrehozása foglal magában:</span><span class="sxs-lookup"><span data-stu-id="102a4-104">Creating an export job for hello Microsoft Azure Import/Export service using hello REST API involves hello following steps:</span></span>

-   <span data-ttu-id="102a4-105">Hello kiválasztásával blobok tooexport.</span><span class="sxs-lookup"><span data-stu-id="102a4-105">Selecting hello blobs tooexport.</span></span>

-   <span data-ttu-id="102a4-106">Az beszerzése szállítási helyről.</span><span class="sxs-lookup"><span data-stu-id="102a4-106">Obtaining a shipping location.</span></span>

-   <span data-ttu-id="102a4-107">Hello exportálási feladat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="102a4-107">Creating hello export job.</span></span>

-   <span data-ttu-id="102a4-108">A szállítási a üres meghajtók tooMicrosoft támogatott szolgáltatónként szolgáltatáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="102a4-108">Shipping your empty drives tooMicrosoft via a supported carrier service.</span></span>

-   <span data-ttu-id="102a4-109">Hello exportálási feladatok frissítése hello csomag információval.</span><span class="sxs-lookup"><span data-stu-id="102a4-109">Updating hello export job with hello package information.</span></span>

-   <span data-ttu-id="102a4-110">A fogadó vissza a Microsoft hello meghajtókat.</span><span class="sxs-lookup"><span data-stu-id="102a4-110">Receiving hello drives back from Microsoft.</span></span>

 <span data-ttu-id="102a4-111">Lásd: [hello Windows Azure Import/Export szolgáltatás tooTransfer adatok tooBlob Storage használatával](storage-import-export-service.md) hello Import/Export szolgáltatás és mutatja be a áttekintését hogyan toouse hello [Azure-portálon](https://portal.azure.com/) toocreate és kezelése az importálás és exportálni a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="102a4-111">See [Using hello Windows Azure Import/Export service tooTransfer Data tooBlob Storage](storage-import-export-service.md) for an overview of hello Import/Export service and a tutorial that demonstrates how toouse hello [Azure portal](https://portal.azure.com/) toocreate and manage import and export jobs.</span></span>

## <a name="selecting-blobs-tooexport"></a><span data-ttu-id="102a4-112">Blobok tooexport kiválasztása</span><span class="sxs-lookup"><span data-stu-id="102a4-112">Selecting blobs tooexport</span></span>
 <span data-ttu-id="102a4-113">exportálási feladat toocreate, szüksége lesz a tooprovide megjeleníteni kívánt tooexport importáljon a tárfiókba blobok listáját.</span><span class="sxs-lookup"><span data-stu-id="102a4-113">toocreate an export job, you will need tooprovide a list of blobs that you want tooexport from your storage account.</span></span> <span data-ttu-id="102a4-114">Néhány módon tooselect blobok exportált toobe:</span><span class="sxs-lookup"><span data-stu-id="102a4-114">There are a few ways tooselect blobs toobe exported:</span></span>

-   <span data-ttu-id="102a4-115">Egy blob relatív elérési út tooselect egyetlen blob és az összes a pillanatképek használhatja.</span><span class="sxs-lookup"><span data-stu-id="102a4-115">You can use a relative blob path tooselect a single blob and all of its snapshots.</span></span>

-   <span data-ttu-id="102a4-116">Egy blob relatív elérési út tooselect használhatja a pillanatképek nélkül egyetlen blob.</span><span class="sxs-lookup"><span data-stu-id="102a4-116">You can use a relative blob path tooselect a single blob excluding its snapshots.</span></span>

-   <span data-ttu-id="102a4-117">Egy blob relatív elérési utat és egy pillanatkép idő tooselect egyetlen pillanatkép is használhatja.</span><span class="sxs-lookup"><span data-stu-id="102a4-117">You can use a relative blob path and a snapshot time tooselect a single snapshot.</span></span>

-   <span data-ttu-id="102a4-118">Használható a blob előtagja tooselect blobok és a pillanatfelvételeket hello megadott előtag.</span><span class="sxs-lookup"><span data-stu-id="102a4-118">You can use a blob prefix tooselect all blobs and snapshots with hello given prefix.</span></span>

-   <span data-ttu-id="102a4-119">Exportálhatja a blobok és a pillanatképek hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="102a4-119">You can export all blobs and snapshots in hello storage account.</span></span>

 <span data-ttu-id="102a4-120">További információ a blobok tooexport, lásd: hello [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet.</span><span class="sxs-lookup"><span data-stu-id="102a4-120">For more information about specifying blobs tooexport, see hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span>

## <a name="obtaining-your-shipping-location"></a><span data-ttu-id="102a4-121">A szállítási hely beszerzése</span><span class="sxs-lookup"><span data-stu-id="102a4-121">Obtaining your shipping location</span></span>
<span data-ttu-id="102a4-122">Exportálási feladat létrehozásához szükséges tooobtain szállítási hely nevét és címét hívó hello [első hely](https://portal.azure.com) vagy [lista helyek](/rest/api/storageimportexport/listlocations) műveletet.</span><span class="sxs-lookup"><span data-stu-id="102a4-122">Before creating an export job, you need tooobtain a shipping location name and address by calling hello [Get Location](https://portal.azure.com) or [List Locations](/rest/api/storageimportexport/listlocations) operation.</span></span> <span data-ttu-id="102a4-123">`List Locations`helyek és a levelezési címek listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="102a4-123">`List Locations` will return a list of locations and their mailing addresses.</span></span> <span data-ttu-id="102a4-124">Jelöljön ki egy helyet a hello-listát adott vissza, és küldje el a merevlemez-meghajtók toothat címét.</span><span class="sxs-lookup"><span data-stu-id="102a4-124">You can select a location from hello returned list and ship your hard drives toothat address.</span></span> <span data-ttu-id="102a4-125">Is használhatja a hello `Get Location` művelet tooobtain hello közvetlenül szállítási címeket az adott helyen.</span><span class="sxs-lookup"><span data-stu-id="102a4-125">You can also use hello `Get Location` operation tooobtain hello shipping address for a specific location directly.</span></span>

<span data-ttu-id="102a4-126">Kövesse az alábbi tooobtain hello szállítási hely hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="102a4-126">Follow hello steps below tooobtain hello shipping location:</span></span>

-   <span data-ttu-id="102a4-127">A tárfiók helye hello hello nevének azonosítása.</span><span class="sxs-lookup"><span data-stu-id="102a4-127">Identify hello name of hello location of your storage account.</span></span> <span data-ttu-id="102a4-128">Ez az érték található hello **hely** hello tárolási fiók található **irányítópult** hello klasszikus portál vagy hello service management API-művelet használatával lekérdezett a [beolvasása A tárfiók tulajdonságai](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span><span class="sxs-lookup"><span data-stu-id="102a4-128">This value can be found under hello **Location** field on hello storage account's **Dashboard** in hello classic portal or queried for by using hello service management API operation [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span></span>

-   <span data-ttu-id="102a4-129">Lekérni hello helyét, amelyek a rendelkezésre álló tooprocess ezt a tárfiókot hívó hello `Get Location` műveletet.</span><span class="sxs-lookup"><span data-stu-id="102a4-129">Retrieve hello location that are available tooprocess this storage account by calling hello `Get Location` operation.</span></span>

-   <span data-ttu-id="102a4-130">Ha hello `AlternateLocations` hello hely tulajdonsága tartalmazza magát hello helye, akkor célszerű rendben toouse ezen a helyen.</span><span class="sxs-lookup"><span data-stu-id="102a4-130">If hello `AlternateLocations` property of hello location contains hello location itself, then it is okay toouse this location.</span></span> <span data-ttu-id="102a4-131">Ellenkező esetben hívható hello `Get Location` hello másodlagos helyek egyikén újra a műveletet.</span><span class="sxs-lookup"><span data-stu-id="102a4-131">Otherwise, call hello `Get Location` operation again with one of hello alternate locations.</span></span> <span data-ttu-id="102a4-132">hello eredeti helyre ideiglenesen le lehet, hogy a következő karbantartási.</span><span class="sxs-lookup"><span data-stu-id="102a4-132">hello original location might be temporarily closed for maintenance.</span></span>

## <a name="creating-hello-export-job"></a><span data-ttu-id="102a4-133">Hello exportálási feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="102a4-133">Creating hello export job</span></span>
 <span data-ttu-id="102a4-134">toocreate hello exportálási feladat, hívás hello [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) műveletet.</span><span class="sxs-lookup"><span data-stu-id="102a4-134">toocreate hello export job, call hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="102a4-135">Szüksége lesz a következő információ tooprovide hello:</span><span class="sxs-lookup"><span data-stu-id="102a4-135">You will need tooprovide hello following information:</span></span>

-   <span data-ttu-id="102a4-136">Hello feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="102a4-136">A name for hello job.</span></span>

-   <span data-ttu-id="102a4-137">hello tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="102a4-137">hello storage account name.</span></span>

-   <span data-ttu-id="102a4-138">a hely neve, hello előző lépésben beolvasott szállítási hello.</span><span class="sxs-lookup"><span data-stu-id="102a4-138">hello shipping location name, obtained in hello previous step.</span></span>

-   <span data-ttu-id="102a4-139">A feladat típusa (Exportálás).</span><span class="sxs-lookup"><span data-stu-id="102a4-139">A job type (Export).</span></span>

-   <span data-ttu-id="102a4-140">hello Válaszcím ahol hello meghajtók küldjön hello exportálási feladat befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="102a4-140">hello return address where hello drives should be sent after hello export job has completed.</span></span>

-   <span data-ttu-id="102a4-141">hello azoknak a blobok (blob előtagok) toobe exportált.</span><span class="sxs-lookup"><span data-stu-id="102a4-141">hello list of blobs (or blob prefixes) toobe exported.</span></span>

## <a name="shipping-your-drives"></a><span data-ttu-id="102a4-142">A meghajtók szállítási</span><span class="sxs-lookup"><span data-stu-id="102a4-142">Shipping your drives</span></span>
 <span data-ttu-id="102a4-143">A következő hello Azure Import/Export eszköz toodetermine hello meghajtók száma toosend exportált toobe kijelölt hello blobok alapján kell használni, és hello meghajtó mérete.</span><span class="sxs-lookup"><span data-stu-id="102a4-143">Next, use hello Azure Import/Export Tool toodetermine hello number of drives you need toosend, based on hello blobs you have selected toobe exported and hello drive size.</span></span> <span data-ttu-id="102a4-144">Lásd: hello [Azure Import/Export eszköz hivatkozás](storage-import-export-tool-how-to-v1.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="102a4-144">See hello [Azure Import/Export Tool Reference](storage-import-export-tool-how-to-v1.md) for details.</span></span>

 <span data-ttu-id="102a4-145">Csomag hello meghajtók egyetlen csomagot, majd küldje el azokat toohello hello beolvasott cím korábbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="102a4-145">Package hello drives in a single package and ship them toohello address obtained in hello earlier step.</span></span> <span data-ttu-id="102a4-146">Vegye figyelembe a hello nyomon követése a következő lépéshez hello csomag száma.</span><span class="sxs-lookup"><span data-stu-id="102a4-146">Note hello tracking number of your package for hello next step.</span></span>

> [!NOTE]
>  <span data-ttu-id="102a4-147">A meghajtók, amely előállít egy azonosítószám a csomag támogatott szolgáltatónként szolgáltatáson keresztül kell küldje el.</span><span class="sxs-lookup"><span data-stu-id="102a4-147">You must ship your drives via a supported carrier service, which will provide a tracking number for your package.</span></span>

## <a name="updating-hello-export-job-with-your-package-information"></a><span data-ttu-id="102a4-148">A csomagadatok hello exportálási feladatok frissítése</span><span class="sxs-lookup"><span data-stu-id="102a4-148">Updating hello export job with your package information</span></span>
 <span data-ttu-id="102a4-149">Miután a nyilvántartási szám, hívja hello [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) művelet tooupdated hello szolgáltatónként neve és nyomon követni a hello feladat számát.</span><span class="sxs-lookup"><span data-stu-id="102a4-149">After you have your tracking number, call hello [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation tooupdated hello carrier name and tracking number for hello job.</span></span> <span data-ttu-id="102a4-150">Opcionálisan megadhat meghajtók, hello válaszcím és hello szállítási dátumot is hello száma.</span><span class="sxs-lookup"><span data-stu-id="102a4-150">You can optionally specify hello number of drives, hello return address, and hello shipping date as well.</span></span>

## <a name="receiving-hello-package"></a><span data-ttu-id="102a4-151">A fogadó hello csomag</span><span class="sxs-lookup"><span data-stu-id="102a4-151">Receiving hello package</span></span>
 <span data-ttu-id="102a4-152">Az exportálási feladat feldolgozása után a meghajtók visszatér a titkosított adatok tooyou.</span><span class="sxs-lookup"><span data-stu-id="102a4-152">After your export job has been processed, your drives will be returned tooyou with your encrypted data.</span></span> <span data-ttu-id="102a4-153">Hello BitLocker kulcs le hello meghajtókhoz által hívó hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) műveletet.</span><span class="sxs-lookup"><span data-stu-id="102a4-153">You can retrieve hello BitLocker key for each of hello drives by calling hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="102a4-154">Majd fel is oldhatja hello meghajtó hello kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="102a4-154">You can then unlock hello drive using hello key.</span></span> <span data-ttu-id="102a4-155">hello meghajtó jegyzékfájl egyes fájlok hello meghajtó, valamint hello eredeti blob cím az egyes fájlok hello listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="102a4-155">hello drive manifest file on each drive contains hello list of files on hello drive, as well as hello original blob address for each file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="102a4-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="102a4-156">Next steps</span></span>

* [<span data-ttu-id="102a4-157">Hello Import/Export szolgáltatás REST API használatával</span><span class="sxs-lookup"><span data-stu-id="102a4-157">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
